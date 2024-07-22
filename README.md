package com.tcs.security.rateLimit;


import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Deque;
import java.util.LinkedList;
import java.util.Stack;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.TimeUnit;

public class RateLimitFilter implements Filter {

    // Store the request counts and timestamps per IP address per URL
    private final ConcurrentHashMap<String, ConcurrentHashMap<String, RateLimitInfo>> requestCountsPerIpAddress = new ConcurrentHashMap<>();

    // Maximum requests allowed per interval per URL
    private static final int MAX_REQUESTS = 5;

    // Time window in milliseconds (1 minute)
    private static final long TIME_WINDOW = TimeUnit.MINUTES.toMillis(1);
    
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // Initialization logic if needed
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {

        if (request instanceof HttpServletRequest && response instanceof HttpServletResponse) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            HttpServletResponse httpResponse = (HttpServletResponse) response;

            //ByPass URL String requestURL = httpRequest.getRequestURI();
            String requestURL = httpRequest.getRequestURI();
                        if (requestURL.endsWith("js") || requestURL.contains(".css")|| requestURL.contains(".min.js") ||
                                requestURL.endsWith(".jpg") || requestURL.endsWith(".png") || requestURL.endsWith(".woff") ||
                                requestURL.endsWith(".ttf") || requestURL.endsWith(".svg")|| requestURL.endsWith(".ico") || requestURL.endsWith(".eot")) {
                            chain.doFilter(request, response);
                            return;
                        }
            // Get the client's IP address
            String clientIpAddress = getClientIpAddress(httpRequest);
            if (clientIpAddress == null) {
                clientIpAddress = "unknown";
            }

            // Get the requested URL
            String requestURI = httpRequest.getRequestURI();
            
            // Check if the rate limit is exceeded for this IP address and URL
            if (isRateLimitExceeded(clientIpAddress, requestURI)) {
                httpResponse.setStatus(429);
                httpResponse.getWriter().write("Rate limit exceeded for this URL. Try again later.");
                return;
            }

            // Proceed with the filter chain if the rate limit is not exceeded
            chain.doFilter(request, response);
        }
    }

    @Override
    public void destroy() {
        // Cleanup logic if needed
    }

    // Method to get the client's IP address
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

    // Method to check if the rate limit is exceeded for a given IP address and URL
    private boolean isRateLimitExceeded(String clientIpAddress, String requestURI) {
        long currentTime = System.currentTimeMillis();

        // Get or create the URL-specific rate limit info for the given IP address
        ConcurrentHashMap<String, RateLimitInfo> urlRateLimits = requestCountsPerIpAddress.computeIfAbsent(clientIpAddress, k -> new ConcurrentHashMap<>());
        RateLimitInfo rateLimitInfo = urlRateLimits.computeIfAbsent(requestURI, k -> new RateLimitInfo(currentTime));
        System.out.println("rateLimitInfo requestCount: " + rateLimitInfo.requestCount.toString());

        synchronized (rateLimitInfo) {
            // Reset the count if the time window has passed
            if (currentTime - rateLimitInfo.timestamp > TIME_WINDOW) {
                rateLimitInfo.reset(currentTime);
            }

            // Increment the request count and check if it exceeds the max requests
            int currentRequestCount = rateLimitInfo.requestCount.incrementAndGet();
            return currentRequestCount > MAX_REQUESTS;
        }
    }

    // Inner class to hold rate limit information
    private static class RateLimitInfo {
        long timestamp; // The timestamp of the first request in the current window
        AtomicInteger requestCount; // The number of requests in the current window

        RateLimitInfo(long timestamp) {
            this.timestamp = timestamp;
            this.requestCount = new AtomicInteger(0);
        }

        // Reset the rate limit information
        void reset(long timestamp) {
            this.timestamp = timestamp;
            this.requestCount.set(0);
        }
    }
}
