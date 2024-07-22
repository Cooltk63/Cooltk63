import java.util.Deque;
import java.util.LinkedList;

public class URLChecker {
    private static final int MAX_SIZE = 5;
    private Deque<String> urlStack = new LinkedList<>();

    public void addUrl(String url) {
        if (urlStack.size() == MAX_SIZE) {
            urlStack.pollFirst();
        }
        urlStack.offerLast(url);

        if (urlStack.size() == MAX_SIZE && areAllUrlsSame()) {
            eliminate();
            urlStack.clear();
        }
    }

    private boolean areAllUrlsSame() {
        String firstUrl = urlStack.peekFirst();
        for (String url : urlStack) {
            if (!url.equals(firstUrl)) {
                return false;
            }
        }
        return true;
    }

    private void eliminate() {
        // Your elimination logic here
        System.out.println("All 5 URLs are the same. Running eliminate()...");
    }

    public static void main(String[] args) {
        URLChecker checker = new URLChecker();
        checker.addUrl("http://example.com");
        checker.addUrl("http://example.com");
        checker.addUrl("http://example.com");
        checker.addUrl("http://example.com");
        checker.addUrl("http://example.com");  // This will trigger eliminate()
        checker.addUrl("http://example2.com"); // This will reset the stack
    }
}