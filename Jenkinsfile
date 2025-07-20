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
																																								            stage('nexus') {
																																									                steps {
																																											                nexusArtifactUploader artifacts: [[artifactId: 'myapp', classifier: '', file: 'target/myapp.war', type: '.war']], credentialsId: 'nexus', groupId: 'com.mycompany', nexusUrl: 'nexus:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'hotstar', version: '8.3.3-SNAPSHOT'
																																													            }
																																														            }
																																															        }
																																																}

