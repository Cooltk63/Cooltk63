async function modifyRequest() {
    // Generate initialization vector (IV) and salt using crypto API
    const iv = crypto.getRandomValues(new Uint8Array(12));
    const ivBase64 = btoa(String.fromCharCode.apply(null, iv)); // Convert IV to Base64
    const salt = crypto.getRandomValues(new Uint8Array(16));
    const saltBase64 = btoa(String.fromCharCode.apply(null, salt)); // Convert salt to Base64

    // Collect form data into an object
    var data = {
        'name': document.getElementById('fullName').value,
        'pf': document.getElementById('pfId').value,
        'designation': document.getElementById('designation').value,
        'department': document.getElementById('department').value,
        'email': document.getElementById('email').value,
        'mobile': document.getElementById('phoneNo').value,
        'REQ_ID': document.getElementById('REQ_ID').value,
        'ddRoleValue': document.getElementById('ddRoleValue').value
    };

    console.log("jsonFormData 1111 >>>> " + JSON.stringify(data));
    try {
        // Encrypt the data using the provided encryption function
        const encryptedData = await encrypt(iv, salt, $("#PASS_PHRASE").val(), data);
        data = encryptedData;
    } catch (e) {
        alert("Error While Encryption: " + e);
    }
    console.log("jsonFormData 222 >>>> " + JSON.stringify(data));

    // Prepare the parameters to send in the POST request
    var param = {
        "iv": ivBase64,
        "salt": saltBase64,
        "data": data
    };
    console.log("Param Data after encryption: " + JSON.stringify(param));

    // Make an AJAX POST request to send the encrypted data to the server
    $.ajax({
        type: "POST",
        url: "./ModifyUserRequest",
        data: JSON.stringify(param),
        contentType: 'application/json;charset=utf-8',
        dataType: 'text',
        async: false,
        success: function (data) {
            $('#ModalModify').modal('hide');
            // Sanitize and display the response data
            var sanitizeData = DOMPurify.sanitize(data);
            showModal(sanitizeData);
            console.log("Data Received for ARD Request: " + data);

            console.log('modifyUser success result: ' + JSON.stringify(data));
        },
        error: function (jqXHR, textStatus, errorMessage) {
            console.error('Error: ' + errorMessage);
        },
        complete: function () {
            console.log('ajax call completed');
        },
    });
}


xxx

@PostMapping("/ModifyUserRequest")
public String modifyUserRequest(@RequestBody Map<String, Object> data, HttpServletRequest request) {
    log.info("Data Received >>>> " + data.toString());
    Map<String, Object> valueMap = new HashMap<>();
    try {
        // Retrieve decryption parameters from the request
        String iv = (String) data.get("iv"); // Base64 encoded IV
        String salt = (String) data.get("salt"); // Base64 encoded salt
        String passPhrase = (String) request.getSession().getAttribute(CommonConstants.PASS_PHRASE); // Passphrase from session
        String encryptedData = (String) data.get("data"); // Encrypted data

        // Logging the parameters for debugging purposes
        log.info("IV: " + iv);
        log.info("Salt: " + salt);
        log.info("Pass Phrase: " + passPhrase);
        log.info("Encrypted Data: " + encryptedData);

        // Decrypt the data
        String jsonData = AesGcm256.decrypt(iv, salt, passPhrase, encryptedData); // Decrypt the data
        log.info("Decrypted form data: " + jsonData);

        // Print the decrypted JSON string to ensure its structure
        log.info("Decrypted JSON data: " + jsonData);

        // Create an ObjectMapper instance and configure it
        ObjectMapper mapper = new ObjectMapper();
        mapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES); // Ignore unknown properties during deserialization

        // Deserialize the JSON string into a Map<String, Object>
        valueMap = mapper.readValue(jsonData, new TypeReference<Map<String, Object>>() {});
        log.info("After Decryption Values: " + valueMap);

    } catch (JsonProcessingException e) {
        log.error("Exception Occurred JsonProcessingException: " + e.getMessage(), e);
    } catch (Exception e) {
        log.error("General Exception Occurred: " + e.getMessage(), e);
    }

    // Pass the deserialized map to the service method
    return adminService.modifyUserRequest(valueMap);
}