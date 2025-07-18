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

        stage('Build Artifacts') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload to S3') {
            steps {
                s3Upload(
                    consoleLogLevel: 'INFO',
                    dontSetBuildResultOnFailure: false,
                    dontWaitForConcurrentBuildCompletion: false,
                    entries: [[
                        bucket: 'bucket',
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
                    profileName: 'AWS',
                    userMetadata: []
                )
            }
        }
    }
}
