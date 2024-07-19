rateLimitInfo.timestamppackage utils.rateLimit;

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

    private final ConcurrentHashMap<String, RateLimitInfo> requestCountsPerIpAddress = new ConcurrentHashMap<>();
    private final int MAX_REQUESTS = 2; // Maximum requests per interval
    private final long TIME_WINDOW = TimeUnit.MINUTES.toMillis(1); // Time window in milliseconds

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // Initialization logic, if any
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {

        if (request instanceof HttpServletRequest && response instanceof HttpServletResponse) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            HttpServletResponse httpResponse = (HttpServletResponse) response;

            String clientIpAddress = getClientIpAddress(httpRequest);
            if (clientIpAddress == null) {
                clientIpAddress = "unknown";
            }

            if (isRateLimitExceeded(clientIpAddress)) {
                httpResponse.setStatus(HttpServletResponse.SC_TOO_MANY_REQUESTS);
                httpResponse.getWriter().write("Rate limit exceeded. Try again later.");
                return;
            }
        }

        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {
        // Cleanup logic, if any
    }

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
                return ip.split(",")[0].trim();
            }
        }

        return request.getRemoteAddr();
    }

    private boolean isRateLimitExceeded(String clientIpAddress) {
        long currentTime = System.currentTimeMillis();
        RateLimitInfo rateLimitInfo = requestCountsPerIpAddress.computeIfAbsent(clientIpAddress, ip -> new RateLimitInfo(currentTime));

        synchronized (rateLimitInfo) {
            if (currentTime - rateLimitInfo.timestamp > TIME_WINDOW) {
                rateLimitInfo.reset(currentTime);
            }

            rateLimitInfo.requestCount.incrementAndGet();
            return rateLimitInfo.requestCount.get() > MAX_REQUESTS;
        }
    }

    private static class RateLimitInfo {
        long timestamp;
        AtomicInteger requestCount;

        RateLimitInfo(long timestamp) {
            this.timestamp = timestamp;
            this.requestCount = new AtomicInteger(0);
        }

        void reset(long timestamp) {
            this.timestamp = timestamp;
            this.requestCount.set(0);
        }
    }
}