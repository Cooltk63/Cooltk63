 @PostMapping("/ModifyUserRequest")
    public String modifyUserRequest(@RequestBody Map<String, Object> data, HttpServletRequest request) {
        ///////////
        log.info("Data Received >>>> " + data.toString());
        HashMap valueMap=new HashMap<>();
        try {
//            Map<String, String> data = formData.toSingleValueMap();

            String jsonData = AesGcm256.decrypt(
                    (String) data.get("iv"),
                    (String) data.get("salt"),
                    (String) request.getSession().getAttribute(CommonConstants.PASS_PHRASE),
                    (String) data.get("data")
            );
            log.info("Decrypted form data " + jsonData.toString());

            // Creating an object of ObjectMapper class
            ObjectMapper mapper = new ObjectMapper();
            mapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
            valueMap = mapper.readValue(jsonData, HashMap.class);

            log.info("After Decryption Values :");


        } catch (JsonProcessingException e) {
            log.info("Exception Occurred JsonProcessingException"+e.getMessage());
            log.info("Exception Occurred Cause"+e.getCause());
        }
        //////////
        return adminService.modifyUserRequest(valueMap);

    }
    xxxxxxxxxxx

    async function modifyRequest() {
      const iv = crypto.getRandomValues(new Uint8Array(12));
      const ivBase64 = btoa(String.fromCharCode.apply(null, iv));
      const salt = crypto.getRandomValues(new Uint8Array(16));
      const saltBase64 = btoa(String.fromCharCode.apply(null, salt));


      var data = {
          'name': document.getElementById('fullName').value,
          'pf': document.getElementById('pfId').value,
          'designation': document.getElementById('designation').value,
          'department': document.getElementById('department').value,
          'email': document.getElementById('email').value,
          'mobile': document.getElementById('phoneNo').value,
          'REQ_ID': document.getElementById('REQ_ID').value,
          'ddRoleValue': document.getElementById('ddRoleValue').value
      }
      

      console.log("jsonFormData 1111 >>>> " + JSON.stringify(data));
      try {
          await encrypt(iv, salt, $("#PASS_PHRASE").val(), data).then(function (result) {
              data = result;
          });
      } catch (e) {
          alert("Error While Encryption" +e)
      }
      console.log("jsonFormData 222 >>>> " + JSON.stringify(data));

      var param = {
          "iv": ivBase64,
          "salt": saltBase64,
          "data": data
      }
      console.log("Param Data after encryption :" + JSON.stringify(param));

      $.ajax({
          type: "POST",
          url: "./ModifyUserRequest",
          data: JSON.stringify(param),
          contentType: 'application/json;charset=utf-8',
          dataType: 'text',
          async: false,
          success: function (data) {
              $('#ModalModify').modal('hide');
              // Responce Data Sanitizing
              var sanitizeData = DOMPurify.sanitize(data);
              showModal(sanitizeData);
              console.log("Data Received for ARD Request : " + data);

              console.log('modifyUser success result  :' + JSON.stringify(data))
          },
          error: function (jqXHR, textStatus, errorMessage) {

          },
          complete: function () {
              console.log('ajax call completed')
          },
      });
  }
