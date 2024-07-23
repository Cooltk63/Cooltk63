// Login Controller @Author : V1012297
package com.tcs.controller;

import com.tcs.bean.AdminHome;
import com.tcs.bean.User;
import com.tcs.service.AdminHomeService;
import com.tcs.service.DashUserService;
import com.tcs.service.LoginService;
import org.apache.log4j.Logger;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.dao.DataAccessException;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;
import org.springframework.web.servlet.view.RedirectView;
import utils.CommonConstants;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import javax.sql.DataSource;
import java.security.Principal;
import java.util.List;
import java.util.Map;

@RequestMapping(value = "/Security")
@Controller
public class LoginController {

    private final static Logger log = Logger.getLogger(LoginController.class);
    private static final String displayMessage = "displayMessage";
    final
    LoginService loginService;
    final DashUserService dashUserService;
    final
    DataSource dataSource;
    final AdminHomeService adminHomeService;
    @Value("#{applicationProp}")
    Map<String, Object> MessageMap;

    public LoginController(LoginService loginService, DashUserService dashUserService, AdminHomeService adminHomeService, DataSource dataSource) {
        this.loginService = loginService;
        this.dashUserService = dashUserService;
        this.adminHomeService = adminHomeService;
        this.dataSource = dataSource;
    }

    // User Login Credentials Authorize
    //@Author : V1012297
    @PostMapping(value = "/home")
    public ModelAndView login(HttpServletRequest request, @ModelAttribute("pfId") String userID, RedirectAttributes redirectAttributes) {

        // If Session Not Existed Create New Session
        HttpSession httpSession = request.getSession(true);
        httpSession.setAttribute(displayMessage, "");

        //On Login IF Session Exists then Invalidate the Session.
        if (httpSession.getAttribute(CommonConstants.USER_ID) != null) {
                log.info("@@@ SESSION EXIST TERMINATING SESSION !!!!");
                httpSession.invalidate();
            // After invalidating old session creating new session
            httpSession = request.getSession(true);
        }

        // If user id is null / empty
        if (userID == null || userID.isEmpty()) {
            RedirectView view = new RedirectView("/", true);
            view.setExposeModelAttributes(false);
            return new ModelAndView(view);

        } else {

            // Get User Details from DB
            User userDetails = loginService.validateUser(userID);

            if (userDetails == null) {
                // Validate If User Creation Request Exists for UserID
                Boolean UserRequestExists = loginService.IsUserRequestExists(userID);
                log.info("Return Result :" + UserRequestExists);
                RedirectView view = new RedirectView("/", true);
                view.setExposeModelAttributes(false);

                httpSession.setAttribute(displayMessage, MessageMap.get("EUSDTNT"));
                // Validate Creation Request Exists for UserID
                if (UserRequestExists) {
                    //view.addStaticAttribute(displayMessage,MessageMap.get("EUSRQPD"));
                    httpSession.setAttribute(displayMessage, MessageMap.get("EUSRQPD"));
                }
                return new ModelAndView(view);

            } else if (userDetails.getUserStatus().equalsIgnoreCase("I")) {

                RedirectView view = new RedirectView("/", true);
                view.setExposeModelAttributes(false);

                httpSession.setAttribute(displayMessage, MessageMap.get("EUSAUTH"));

                return new ModelAndView(view);

            } else {
                ModelAndView view;
                // Validate User & Setting Global Session ID
                loginService.setGlobalSession(userDetails, httpSession);
                //User Role wise Redirection
                String userRole = userDetails.getUserRole();
                switch (userRole) {
                    case "A":
                        view = new ModelAndView("AdminHome");
                        view.addObject("pendingCreateReq", adminHomeService.pendingCreateRequest());
                        view.addObject("pendingModifyDeleteReq", adminHomeService.pendingModifyDeleteRequest());
                        break;
                    case "D":
                        view = new ModelAndView("DashUserHome");
                        view.addObject("max_date", dashUserService.minMaxDate().split("~")[0]);
                        view.addObject("min_date", dashUserService.minMaxDate().split("~")[1]);
                        break;
                    default:
                        log.info("This is default method.");
                        return null;
                }
                view.addObject("userName", userDetails.getUserName());
                view.addObject("userPfId", userDetails.getUserID());
                view.addObject("userRole", userDetails.getUserRole());
                return view;
            }
        }

    }


    @PostMapping("/getUserStatus")
    @ResponseBody
    public Map<String, Object> getUserList() {
        return adminHomeService.getRequestList();
    }

    @PostMapping("/getDashStatus")
    @ResponseBody
    public Map<String, Object> getDashStatus() {
        return adminHomeService.getDashList();
    }

    @PostMapping("/getTopFiveViews")
    @ResponseBody
    public List<AdminHome> getTopFiveDashboards() {
        log.info("dataList :" + adminHomeService.getTopViewList());
        return adminHomeService.getTopViewList();
    }


    //Logout & Invalidate Session
    //@Author : V1012297
    @RequestMapping(value = "/logout", method = RequestMethod.GET)
    public String logoutSuccessfulPage(Model model, HttpServletRequest request) {
        log.info("Inside logout ...!");

        String UserId = (String) request.getSession().getAttribute("userID");

        loginService.deleteSessionId(UserId);
        HttpSession session = request.getSession(false);
        session.invalidate();
        model.addAttribute("title", "Logout");
        return "Logout";
    }

    // Error Page
    //@Author : V1012297
    @RequestMapping(value = "/403", method = RequestMethod.GET)
    public String accessDenied(Model model, Principal principal) {

        if (principal != null) {
            model.addAttribute("message", "Hi " + principal.getName()
                    + "<br> You do not have permission to access this page!");
        } else {
            model.addAttribute("msg",
                    "You do not have permission to access this page!");
        }
        return "view/Misc/404error";
    }

    /*Methode for getting chart of dashUser home screen
     * @Author: V1010939 */
    @PostMapping(value = "/getDepositsData")
    @ResponseBody
    public List<List<Map<String, Object>>> getDashUeserchart(@RequestBody Map map) {
        return dashUserService.getRequestList(map);
    }
    /*### END ###*/

    /*Methode for getting chart of dashUser home screen
     * @Author: V1010939 */
    @PostMapping(value = "/getSplineList")
    @ResponseBody
    public List<List<Map<String, Object>>> getSplinesData(@RequestBody Map map) {
        return dashUserService.getSplinesList(map);
    }
    /*### END ###*/

    @PostMapping("/getLoadingStatus")
    @ResponseBody
    public List<Map<String, Object>> filesStatusData() {
        return adminHomeService.filesStatusData();
    }

    /*Methode for getting previous date for percentage calculations
     * @Author: V1010939 */
    @PostMapping(value = "/getPreviousData")
    @ResponseBody
    public Map<String, Object> getPreviousData(@RequestBody Map map) {
        return dashUserService.getPreviousData(map);
    }
    /*### ENDS ###*/

    /*Methode for getting chart of dashUser home screen
     * @Author: V1010939 */
    @PostMapping(value = "/getSparkLineData")
    @ResponseBody
    public Map<String, Object> getSparkLineData(@RequestBody Map map) {
        return dashUserService.getSparkLineList(map);
    }
    /*### END ###*/

    /*Methode for getting chart of dashUser home screen
     * @Author: V1010939 */
    @PostMapping(value = "/getIncomeExpenseTrend")
    @ResponseBody
    public Map<String, Object> getIncomeExpenseTrend(@RequestBody Map map) {
        return dashUserService.getIncExsTrend(map);
    }
    /*### END ###*/
}
