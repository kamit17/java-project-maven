pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kamit17/java-project-maven.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
            }
        }
        stage('Artifcats') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy to tomcat') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcatcredentials', path: '', url: 'http://tomcat:8080/')], contextPath: 'myapp', war: '**/*.war'
            }
        }
        stage('Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'myapp', classifier: '', file: 'target/myapp.war', type: '.war']], credentialsId: 'nexus', groupId: 'com.mycompany', nexusUrl: 'nexus:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'hotstar', version: '8.3.3-SNAPSHOT'
            }
        }
        stage('Upload to S3') {
            steps {
                s3Upload(
                    consoleLogLevel: 'INFO',
                    dontSetBuildResultOnFailure: false,
                    dontWaitForConcurrentBuildCompletion: false,
                    entries: [[
                        bucket: '<bucket_name>',
                        excludedFile: '',
                        flatten: false,
                        gzipFiles: false,
                        keepForever: false,
                        managedArtifacts: false,
                        noUploadOnFailure: false,
                        selectedRegion: 'us-east-2',
                        showDirectlyInBrowser: false,
                        sourceFile: '**/*.war',
                        storageClass: 'STANDARD',
                        uploadFromSlave: false,
                        useServerSideEncryption: false
                    ]],
                    pluginFailureResultConstraint: 'FAILURE',
                    profileName: 'S3Profile',
                    userMetadata: []
                )
            }
        }
    }
    
}