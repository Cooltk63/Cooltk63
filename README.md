package utils.rateLimit;

import com.github.benmanes.caffeine.cache.Cache;
import com.github.benmanes.caffeine.cache.Caffeine;
import org.springframework.stereotype.Service;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

@Service
public class RateLimiterService {

    private final Cache<String, AtomicInteger> requestCountsPerIpAddress;

    public RateLimiterService() {
        this.requestCountsPerIpAddress = Caffeine.newBuilder()
                .expireAfterWrite(1, TimeUnit.MINUTES)
                .build();
    }

    public boolean isRateLimitExceeded(String clientIpAddress) {
        AtomicInteger requests = requestCountsPerIpAddress.get(clientIpAddress, k -> new AtomicInteger(0));
        if (requests.incrementAndGet() > 2) {
//        if (requests.incrementAndGet() > 60) {
            return true;
        }
        return false;
    }
}
