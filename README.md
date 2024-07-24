async function encryptData(iv, salt, passphrase, data) {
    //console.log("Encrypted Data >>> " + data);
    //console.log("passphrase Data >>> " + passphrase);

    //var passphrase = 'irUd1jJOx2mbLB1owTbU';
    //var passphraseSess = '${sessionScope.pfId}';
    //console.log("passphraseSess Data >>> " + passphraseSess);

    const encoder = new TextEncoder();
    const passphraseBuffer = encoder.encode(passphrase);
    //const salt = crypto.getRandomValues(new Uint8Array(16));

    ///////////////////////////////////////////////////////////////
    const keyMaterial = await crypto.subtle.importKey(
        'raw',
        passphraseBuffer,
        { name: 'PBKDF2' },
        false,
        ['deriveKey']
    );

    const derivedKey = await crypto.subtle.deriveKey(
        {
            name : "PBKDF2",
            salt : salt,  //crypto.getRandomValues(new Uint8Array(16)),
            iterations : 1000,
            hash : "SHA-256"
        },
        keyMaterial,
        {
            name : "AES-GCM",
            length : 256
        },
        true,
        ["encrypt","decrypt"]
    );
    //////////////////////////////////////////////////////////////////////////

    //const iv = crypto.getRandomValues(new Uint8Array(12));
    const encodedData = new TextEncoder().encode(data);
    const encryptedData = await crypto.subtle.encrypt({ name: 'AES-GCM', iv }, derivedKey, encodedData);

    return { iv, salt, encryptedData};
}




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


//const data = 'Welcome abroad!';
//myMethod(data);


async function decryptData(iv, salt, passphrase, encryptedData) {
    console.log("111111 >>>>> ");
    const encoder = new TextEncoder();
    const passphraseBuffer = encoder.encode(passphrase);
    console.log("2222222 >>>>> ");


    //const encryptedArray = new Uint8Array(encryptedData);
    const encryptedArray = base64ToArrayBuffer(encryptedData);



    ///////////////////////////////////////////////////////////////
    const keyMaterial = await crypto.subtle.importKey(
        'raw',
        passphraseBuffer,
        { name: 'PBKDF2' },
        false,
        ['deriveKey']
    );
    console.log("3333333333 >>>>> ");
    const derivedKey = await crypto.subtle.deriveKey(
        {
            name : "PBKDF2",
            salt : salt,  //crypto.getRandomValues(new Uint8Array(16)),
            iterations : 1000,
            hash : "SHA-256"
        },
        keyMaterial,
        {
            name : "AES-GCM",
            length : 256
        },
        true,
        ["encrypt","decrypt"]
    );
    //////////////////////////////////////////////////////////////////////////
    console.log("4444444444 >>>>> ");
    //const iv = crypto.getRandomValues(new Uint8Array(12));
    //const encodedData = new TextEncoder().encode(encryptedData);

        const decryptedData = await crypto.subtle.decrypt({name: 'AES-GCM', iv}, derivedKey, encryptedArray);
        const dec = new TextDecoder();
        console.log("5555555555 >>>>> " + dec.decode(decryptedData));

    console.log("666666666666666 >>>>> ");
    //return { iv, salt, dec.decode(decryptedData)};
    return dec.decode(decryptedData)
}

function decrypt(iv, salt, passphrase, data) {
    return decryptData(iv, salt, passphrase, data)
        .then(value => {
            console.log("decryptData  asdfad" );
            try { // Convert the ArrayBuffer to a base64 encoded string
                //const ivBase64 = btoa(String.fromCharCode.apply(null, value.iv));
                //const saltBase64 = btoa(String.fromCharCode.apply(null, value.salt));
                console.log("value.decryptedData >>>>> " + value);
                //console.log("value.decryptedData >>>>> " + value.decryptedData);
                //const decryptedDataBase64 = btoa(String.fromCharCode.apply(null, new Uint8Array(value.decryptedData)));

                return value;
            } catch (e) {
                console.log("decrypt  " + e);
            }

        })
        .catch(() => {});
}

function base64ToArrayBuffer(base64Text) {
    const binaryString = atob(base64Text);
    const len = binaryString.length;
    const bytes = new Uint8Array(len);
    for (let i = 0; i < len; i++) {
        bytes[i] = binaryString.charCodeAt(i);
    }
    return bytes.buffer;
}
