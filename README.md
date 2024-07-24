Exception Occurred JsonProcessingException: Cannot deserialize value of type `java.util.LinkedHashMap<java.lang.String,java.lang.Object>` from Array value (token `JsonToken.START_ARRAY`)
 at [Source: REDACTED (`StreamReadFeature.INCLUDE_SOURCE_IN_LOCATION` disabled); line: 1, column: 1]
com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot deserialize value of type `java.util.LinkedHashMap<java.lang.String,java.lang.Object>` from Array value (token `JsonToken.START_ARRAY`)


This the error i am getting for your previously provided code and below i am providing my encryption method please check the front end and backnd changes which i have required to make


function encrypt(iv, salt, passphrase, data) {
    return encryptData(iv, salt, passphrase, data)
        .then(value => {

            // Convert the ArrayBuffer to a base64 encoded string
            const ivBase64 = btoa(String.fromCharCode.apply(null, value.iv));
            const saltBase64 = btoa(String.fromCharCode.apply(null, value.salt));
            const encryptedDataBase64 = btoa(String.fromCharCode.apply(null, new Uint8Array(value.encryptedData)));

            //console.log("ivBase64 >>>>>>> " + ivBase64);
            //console.log("saltBase64 >>>>>>> " + saltBase64);
            //console.log("encryptedDataBase64 >>>>>  " + encryptedDataBase64);

            //$("#"+feild).val(encryptedDataBase64)
            // $("#user").append('<input type="hidden" name="' + field + 'Enc" value="' + encryptedDataBase64 + '">');
            //return { ivBase64, saltBase64, encryptedDataBase64};
            return encryptedDataBase64;
            //return value.encryptedData;

        })
        .catch(() => {});
}

