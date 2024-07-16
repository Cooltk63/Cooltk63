package utils;

import com.tcs.bean.User;
import com.tcs.service.LoginService;
import org.apache.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.dao.DataAccessException;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.ReentrantLock;

// @Author V1012297
public class LoginFilter implements Filter {

    private static Map<Integer, String> urlMap = new HashMap<>();
    private static final Logger log = Logger.getLogger(LoginFilter.class);
    private static ReentrantLock lock = null;
    private static String userId, userSessionId, currentUserSessionId;

    @Autowired
    LoginService loginService;

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // Initialization logic if needed
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain)
            throws IOException, ServletException {
        if (null == lock) {
            lock = new ReentrantLock();
        }

        HttpServletRequest httpRequest = (HttpServletRequest) servletRequest;
        HttpServletResponse httpResponse = (HttpServletResponse) servletResponse;

        String requestURL = httpRequest.getRequestURI();
        if (requestURL.endsWith(".js") || requestURL.contains(".css") || requestURL.endsWith(".jpg") || requestURL.endsWith(".png")) {
            filterChain.doFilter(servletRequest, servletResponse);
            return;
        }

        httpResponse.setHeader("cache-control", "no-cache, no-store, must-revalidate");
        httpResponse.setHeader("pragma", "no-cache");
        httpResponse.setDateHeader("expires", -1);
        httpResponse.setHeader("X-Content-Type-Options", "nosniff");
        httpResponse.setHeader("X-Frame-Options", "DENY");

        HttpSession session = httpRequest.getSession(false);
        if (session != null && session.getAttribute(CommonConstants.USER_ID) != null) {
            userId = session.getAttribute(CommonConstants.USER_ID).toString();
            try {
                userSessionId = getUserSessionID(userId, lock);
            } catch (SQLException e) {
                log.error("SQL Exception Occurred: " + e.getMessage());
            }

            currentUserSessionId = session.getAttribute(CommonConstants.USER_SESSION_ID).toString();

            if (userSessionId != null && currentUserSessionId != null && !userSessionId.equals(currentUserSessionId)) {
                log.info("@@ Duplicate Session Detected ..!");
                terminateUserSession(httpRequest, httpResponse);
                return;
            } else if (userSessionId == null || currentUserSessionId == null) {
                log.info("db session null!");
                terminateUserSession(httpRequest, httpResponse);
                return;
            }

            // Check user role in the URL
            String userRole = (String) session.getAttribute(CommonConstants.USER_ROLE);
            if (!isAuthorized(requestURL, userRole)) {
                httpResponse.sendError(HttpServletResponse.SC_FORBIDDEN, "You do not have permission to access this resource.");
                return;
            }
        } else {
            httpResponse.sendRedirect("http://localhost:8001/");
            return;
        }

        filterChain.doFilter(servletRequest, servletResponse);
    }

    private boolean isAuthorized(String requestURL, String userRole) {
        // Check if the URL contains the role identifier
        if (("A".equals(userRole) && !requestURL.contains("admin")) || ("U".equals(userRole) && !requestURL.contains("user"))) {
            return false;
        }
        return true;
    }

    private String getUserSessionID(String userID, ReentrantLock lock) throws SQLException {
        Connection con = new DBConn().getConnectionFromJNDI();
        String sessionId = null;
        try {
            String query = "select LOGIN_TOKEN from DM_LOGIN where LOGIN_USER= ?";
            lock.lock();
            PreparedStatement ps = con.prepareStatement(query);
            ps.setString(1, userID);
            ResultSet rs = ps.executeQuery();
            if (rs.next()) {
                sessionId = rs.getString(CommonConstants.LOGIN_TOKEN);
            }
            rs.close();
            con.close();
        } catch (SQLException e) {
            log.error(CommonConstants.SQL_EXCEPTION + " : " + e.getMessage());
        } finally {
            if (null != con) {
                try {
                    con.close();
                } catch (SQLException e) {
                    log.error(CommonConstants.SQL_EXCEPTION + " : " + e.getMessage());
                }
            }
            lock.unlock();
        }
        return sessionId;
    }

    private void terminateUserSession(HttpServletRequest request, HttpServletResponse response) {
        HttpSession session = request.getSession(false);
        if (session != null) {
            session.invalidate();
        }
        try {
            response.sendRedirect("http://localhost:8001/");
        } catch (IOException e) {
            log.error("IO Exception Occurred");
        }
    }

    static {
        urlMap.put(1, "AdminUser");
        urlMap.put(2, "DashUser");
    }
}