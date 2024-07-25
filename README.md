@PostMapping("/ModifyUser")
    public ModelAndView modifyUser(HttpServletRequest request,@RequestBody MultiValueMap<String, String> formData) {
        log.info("Add USER requestMap data " + formData.toSingleValueMap());
        log.info("Add USER requestMap data " + formData);
        Map<String, String> data = formData.toSingleValueMap();
        String jsonData = AesGcm256.decrypt(
                data.get("iv"),
                data.get("salt"),
                (String) request.getSession().getAttribute(CommonConstants.PASS_PHRASE),
                data.get("data")
        );
        log.info("Decrypted form data " + jsonData);

        JSONObject jsonObject = new JSONObject(jsonData);

        jsonObject.get("Req_Type");

//        String op = request.getParameter("Req_Type");
        String op = (String) jsonObject.get("Req_Type");
        if (op.equalsIgnoreCase("M") || op.equalsIgnoreCase("D"))
        {

            ModelAndView viewUM = new ModelAndView(USER_MANAGEMENT_VIEW);
            viewUM.addObject("Designation",adminService.getDesignationList());
            viewUM.addObject("Department",adminService.getDepartmentList());
            viewUM.addObject("UserData", adminService.getUserData());
            String result = adminService.modifyDeleteUserRequest(request, jsonObject);
            viewUM.addObject(DISPLAY_MESSAGE, result);
            return viewUM;
        } else {
            ModelAndView viewUM = new ModelAndView(MODIFY_USER_VIEW);
            viewUM.addObject("Designation",adminService.getDesignationList());
            viewUM.addObject("Department",adminService.getDepartmentList());
            return viewUM;
        }
    }

above this the method i am using 
& below data i am getting in my console 

INFO AdminController:224 - Add USER requestMap data {roleModal=A, pf1=11111111, email=bkkkkkkk@sbi.co.in, designation=Manager, name1=A D, Req_Type=V, mobile1=3333333333, department=Insurance}
15:22:34,633  INFO AdminController:225 - Add USER requestMap data {roleModal=[A], pf1=[11111111], email=[bkkkkkkk@sbi.co.in], designation=[Manager], name1=[A D], Req_Type=[V], mobile1=[3333333333], department=[Insurance]}

and after that controller execution i am getting the error 
org.json.JSONException: A JSONObject text must begin with '{' at 0 [character 1 line 1]
	at org.json.JSONTokener.syntaxError(JSONTokener.java:507)
	at org.json.JSONObject.(JSONObject.java:222)
	at org.json.JSONObject.(JSONObject.java:406)
	at com.tcs.controller.AdminController.modifyUser(AdminController.java:235)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)


