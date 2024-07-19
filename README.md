package utils.rateLimit;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.TimeUnit;

public class RateLimitFilter implements Filter {

    // Store the request counts and timestamps per IP address
    private final ConcurrentHashMap<String, RateLimitInfo> requestCountsPerIpAddress = new ConcurrentHashMap<>();
    
    // Maximum requests allowed per interval
    private final int MAX_REQUESTS = 20; 
    
    // Time window in milliseconds (1 minute)
    private final long TIME_WINDOW = TimeUnit.MINUTES.toMillis(1); 

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // Initialization logic if needed
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {

        // Check if the request and response are HTTP requests/responses
        if (request instanceof HttpServletRequest && response instanceof HttpServletResponse) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            HttpServletResponse httpResponse = (HttpServletResponse) response;

            // Get the client's IP address
            String clientIpAddress = getClientIpAddress(httpRequest);
            if (clientIpAddress == null) {
                clientIpAddress = "unknown";
            }

            // Check if the rate limit is exceeded for the client's IP address
            if (isRateLimitExceeded(clientIpAddress)) {
                httpResponse.setStatus(HttpServletResponse.SC_TOO_MANY_REQUESTS);
                httpResponse.getWriter().write("Rate limit exceeded. Try again later.");
                return;
            }
        }

        // Proceed with the filter chain if the rate limit is not exceeded
        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {
        // Cleanup logic if needed
    }

    // Helper method to get the client's IP address from various headers or remote address
    private String getClientIpAddress(HttpServletRequest request) {
        String[] headers = {
                "X-Forwarded-For",
                "Proxy-Client-IP",
                "WL-Proxy-Client-IP",
                "HTTP_CLIENT_IP",
                "HTTP_X_FORWARDED_FOR"
        };

        for (String header : headers) {
            String ip = request.getHeader(header);
            if (ip != null && !ip.isEmpty() && !"unknown".equalsIgnoreCase(ip)) {
                return ip.split(",")[0].trim(); // Get the first IP if there are multiple
            }
        }

        return request.getRemoteAddr(); // Fallback to remote address
    }

    // Method to check if the rate limit is exceeded for the given IP address
    private boolean isRateLimitExceeded(String clientIpAddress) {
        long currentTime = System.currentTimeMillis();
        RateLimitInfo rateLimitInfo = requestCountsPerIpAddress.computeIfAbsent(clientIpAddress, ip -> new RateLimitInfo(currentTime));

        synchronized (rateLimitInfo) {
            // Reset the request count if the time window has passed
            if (currentTime - rateLimitInfo.timestamp > TIME_WINDOW) {
                rateLimitInfo.reset(currentTime);
            }

            // Increment the request count and check if it exceeds the max requests
            int currentRequestCount = rateLimitInfo.requestCount.incrementAndGet();
            return currentRequestCount > MAX_REQUESTS;
        }
    }

    // Inner class to hold rate limit information for an IP address
    private static class RateLimitInfo {
        long timestamp; // The timestamp of the first request in the current window
        AtomicInteger requestCount; // The number of requests in the current window

        RateLimitInfo(long timestamp) {
            this.timestamp = timestamp;
            this.requestCount = new AtomicInteger(0);
        }

        // Reset the timestamp and request count
        void reset(long timestamp) {
            this.timestamp = timestamp;
            this.requestCount.set(0);
        }
    }
}