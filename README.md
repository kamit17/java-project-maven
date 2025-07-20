# Java Maven Project - Jenkins Pipeline

This project contains a Jenkins pipeline for building, testing, analyzing, packaging, and deploying a Java Maven application.

## Pipeline Stages

1. **Checkout**  
   Clones the source code from the `main` branch of the GitHub repository:

   ```
   https://github.com/kamit17/java-project-maven.git
   ```

2. **Compile**  
   Compiles the project using:

   ```
   mvn compile
   ```

3. **Test**  
   Runs unit tests:

   ```
   mvn test
   ```

4. **SonarQube Analysis**  
   Performs static code analysis with SonarQube using:

   ```
   mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar
   ```

5. **Artifacts**  
   Packages the application as a WAR file:

   ```
   mvn clean package
   ```

6. **Deploy to Tomcat**  
   Deploys the WAR file to a Tomcat 9 server using Jenkins' deploy plugin. The deployment context is:

   ```
   /myapp
   ```

7. **Upload to Nexus**  
   Uploads the WAR file to a Nexus 3 repository:
   - Group ID: `com.mycompany`
   - Artifact ID: `myapp`
   - Version: `8.3.3-SNAPSHOT`
   - Repository: `hotstar`

8. **Upload to S3**  
   Uploads the WAR file to a bucket in AWS S3 .

## Requirements

- Jenkins with required plugins:
  - Maven
  - SonarQube Scanner
  - Deploy to Container
  - Nexus Artifact Uploader
  - S3 Uploader
- Configured credentials in Jenkins:
  - `tomcatcredentials` (Tomcat)
  - `nexus` (Nexus)
  - `S3Profile` (AWS)
