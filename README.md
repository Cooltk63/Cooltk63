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