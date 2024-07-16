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
    private static final  Logger log = Logger.getLogger(LoginFilter.class);
    private static ReentrantLock lock = null;
    private static String userId, userSessionId, currentUserSessionId;

    @Autowired
    LoginService loginService;

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
//        log.info("Inside Login Filter INIT");
//        Filter.super.init(filterConfig);
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        //log.info("Inside Login DO FILTER <><START><>");

        // Step 1
        if (null == lock) {
            lock = new ReentrantLock();
        }

        // Step 2 & Step 3
        HttpServletRequest httpRequest = (HttpServletRequest) servletRequest;
        HttpServletResponse httpResponse = (HttpServletResponse) servletResponse;
        ////////////////////////////

        httpRequest.getSession().getMaxInactiveInterval();
        //log.info("Time to Logout : ");

        String requestURL = httpRequest.getRequestURI();
        if (requestURL.endsWith("js") || requestURL.contains(".css") || requestURL.endsWith(".jpg") || requestURL.endsWith(".png")) {
            filterChain.doFilter(servletRequest, servletResponse);
        }

        ////////////////////////
        //log.info("@@  Start");
        httpResponse.setHeader("cache-control", "no-cache, no-store, must-revalidate");
        httpResponse.setHeader("pragma", "no-cache");
        httpResponse.setDateHeader("expires", -1);
        httpResponse.setHeader("X-Content-Type-Options", "nosniff");
        httpResponse.setHeader("X-Frame-Options", "DENY");
        //log.info("@@ END");
        // Step 4

        // If Session is not NULL then verify DB & Current Session ID
        if (httpRequest.getSession() != null && httpRequest.getSession().getAttribute(CommonConstants.USER_ID) != null) {
            //log.info("INSIDE ><><><>< If Session is not NULL then verify DB & Current Session ID ><><><><");
            userId = httpRequest.getSession().getAttribute(CommonConstants.USER_ID).toString();
            //log.info("UserID at LoginFilter Session ! NULL :" + userId);
            try {
                //<><>Getting Stored User Session ID from DB
                userSessionId = getUserSessionID(userId, lock);
                //log.info(" @@ Session ID from DB :" + userSessionId);

            } catch (SQLException e) {
                log.error("SQL Exception Occurred"+ " : " + e.getMessage());
            }


            //<><>Getting Current User SessionID from HTTP Session
            currentUserSessionId = httpRequest.getSession().getAttribute(CommonConstants.USER_SESSION_ID).toString();
            //log.info("Current Login User Session ID Is: " + currentUserSessionId);
            


            //Comparing DB SessionID with Current Login SessionID
            // *** IF Different Session ID for Current SessionID & DB Stored SessionID then Logout User ***
            if (userSessionId != null && currentUserSessionId != null && !userSessionId.equals(currentUserSessionId)) {
                log.info("@@ Duplicate Session Detected ..!");
                // DB & HTTP Session ID Matched Terminating Session
                terminateUserSession(httpRequest, httpResponse);
                //log.info("@@ Duplicate user session has been terminated.");
            } else if (userSessionId == null ||  currentUserSessionId == null) {
                log.info("db session null!");
                terminateUserSession(httpRequest, httpResponse);
            }
//            log.info("OUTSIDE ><><><>< DB & LOCAL Session ID Compare ><><><><");
        }
        //log.info("Exit from Login Filter <><END><>");
        filterChain.doFilter(servletRequest, servletResponse);
    }

   /* private String getUrlFilterKey(String curl) {
        //System.out.println("********************** "+curl);
        curl = curl.substring(curl.indexOf("/") + 5, curl.length());
        String[] part = curl.split("/");
        return part[0];
    }*/


    // Getting User Session ID TimeStamp Status from DB
    private String getUserSessionID(String userID, ReentrantLock lock) throws SQLException {
        //log.info("<><><> UserID Inside getUserSessionID Method:" + "USER ID FOR QUERY :" + userID);
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
                //log.info("DB Session ID :" + sessionId);
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

    //Terminating User Session
    private void terminateUserSession(HttpServletRequest request, HttpServletResponse response) {
        //log.info("Inside the terminate");
        HttpSession session = request.getSession(false);

            if (session != null) {
                //log.info("Inside terminate User Session Method #SESSION NOT NULL#");
                String userID = (String) session.getAttribute(CommonConstants.USER_ID);

                if (userID != null) {
                    //log.info("Inside terminate User Session Method #USERID NOT NULL#" +userID);
                    //log.info("");
                    session.invalidate();

                }
            } else {
                //log.info("Inside Else ##Session is NULL ## ");
                User userBean = new User();
                userBean.setLoginFlag("N");
            }

            try {
                //log.info("Redirecting to login from Filter");
                response.sendRedirect("/../Security/logout");
            } catch (IOException e) {
                log.error("IO Exception Occurred ");
            }


    }

    
    static {

        urlMap.put(1, "AdminUser");
        urlMap.put(2, "DashUser");
    }


}
