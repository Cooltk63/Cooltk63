public boolean fileDecryption(String fileType,String fileName,String sessionDate,String fileKeyName ,String decFileName,String extFileTxt)
            throws ConfigurationException, IOException, InvalidKeySpecException, NoSuchPaddingException, IllegalBlockSizeException,
            NoSuchAlgorithmException, BadPaddingException, InvalidKeyException {

        log.info("into decryptFile -  ");
        PropertiesConfiguration config = new PropertiesConfiguration("common.properties");
        Path path = Paths.get(config.getProperty(DASHBOARD_PATH).toString()+"/"+fileKeyName);
        log.info("path-");
        byte[] data = Files.readAllBytes(path);
        log.info("path- 2 ");

        PrivateKey tcsPrivateKey = getPrivateKey(config.getProperty(DASHBOARD_PATH).toString()+"/"+"privateKey/PrivateKeyNew.der");
        byte[] dec = decryptKeyFile(tcsPrivateKey, data);


        String str = new String(dec, StandardCharsets.UTF_8); // for UTF-8 encoding
        String encryptFileHash = str.split("\\|\\|")[1];

        log.info("myField   : ");
        log.info("encryptFileHash : ");


        byte[] cipher = new byte[16];
        ByteBuffer bb = ByteBuffer.wrap(dec);
        bb.get(cipher, 0, cipher.length);

        // Extracting IV
        byte[] byteArrCopyNew  = Arrays.copyOfRange(dec, 64, dec.length);


        try {
            decryptFile(config.getProperty(DASHBOARD_PATH).toString()+"/"+fileName,
                    config.getProperty(DASHBOARD_PATH).toString()+"/"+decFileName, cipher, byteArrCopyNew);

            log.info("after decryptFilev fn ");
            String decryptFileHash = getFileHash(config.getProperty(DASHBOARD_PATH).toString()+"/"+decFileName);
            log.info("Input (Hex) - decryptFileHash - :");

            if(encryptFileHash.equals(decryptFileHash)) {
                log.info("Hash Match");

            } else {

                log.info("Hash Does Not Match");
            }

            log.info("after unzip ascii files");
            gunzipIt(decFileName,extFileTxt);
            log.info("Aftere Gz extraction ");
        } catch (FileNotFoundException e) {
            log.error("FileNotFound Exception Occurred");
            return false;
        } catch (Exception e) {
            log.error("Exception Occurred");
            return false;
        }

        return true;
    }
