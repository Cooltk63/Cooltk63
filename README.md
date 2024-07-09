The error message you're encountering, `SunCertPathBuilderException: unable to find valid certification path to requested target`, indicates that Java is unable to establish a secure connection because it does not trust the certificate chain presented by the Maven repository server.

Here's a checklist of steps you can take to resolve this issue:

1. **Verify Java Installation**:
   Ensure that the correct version of Java (Java 22) is installed and being used by your project.

2. **Import the Certificate Correctly**:
   Double-check that the certificate for the Maven repository has been correctly imported into the Java `cacerts` file.

   ```sh
   keytool -import -alias mavenRepo -file /path/to/repo-certificate.crt -keystore $JAVA_HOME/lib/security/cacerts
   ```

   Make sure you use the correct path to your Java `cacerts` file and the alias name.

3. **Check Environment Variables**:
   Ensure the `JAVA_HOME` environment variable points to the correct Java installation.

4. **Maven Settings**:
   Ensure your Maven settings are correctly configured to use the right proxy settings and trust store.

   Edit `settings.xml` located in `{MAVEN_HOME}/conf` or `{USER_HOME}/.m2/` and configure the `<proxies>` section if you are behind a corporate proxy.

5. **Proxy Configuration**:
   If you're behind a corporate proxy, make sure your proxy settings are properly configured in both the Maven settings and in your system environment variables.

6. **Clear Maven Cache**:
   Sometimes clearing the Maven cache can help resolve such issues.

   ```sh
   mvn clean install
   ```

7. **Use `-Djavax.net.ssl.trustStore` JVM Argument**:
   Run Maven with the `-Djavax.net.ssl.trustStore` and `-Djavax.net.ssl.trustStorePassword` arguments to specify the custom trust store.

   ```sh
   mvn clean install -Djavax.net.ssl.trustStore=/path/to/your/truststore.jks -Djavax.net.ssl.trustStorePassword=yourPassword
   ```

If none of these steps resolve the issue, there might be a deeper configuration problem. You may need to check with your organization's IT department to ensure all network configurations and certificates are correctly set up.