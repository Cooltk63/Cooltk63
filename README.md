// User Login Credentials Authorize
@PostMapping(value = "/home")
public ModelAndView login(HttpServletRequest request, @ModelAttribute("pfId") String userID, RedirectAttributes redirectAttributes) {
    // If Session Not Existed Create New Session
    HttpSession httpSession = request.getSession(true);
    httpSession.setAttribute(displayMessage, "");

    // On Login IF Session Exists then Invalidate the Session.
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
            // Set the user attribute in the session
            httpSession.setAttribute("user", userDetails);

            // User Role wise Redirection
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