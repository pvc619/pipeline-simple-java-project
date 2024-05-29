pipeline {
    agent any 

    environment {
        EMAIL_RECIPIENTS = 'pavan.chawthay@deepmatrix.io' // Replace with your email
    }

    triggers {
        githubPush() // Trigger the pipeline on GitHub push events
    }


    stages {


        stage('Checkout') {
            steps {
                script {
                    // Custom checkout of the repository with additional branch configuration
                    checkout([$class: 'GitSCM', 
                              branches: [[name: 'main']],
                              doGenerateSubmoduleConfigurations: false,
                              extensions: [],
                              userRemoteConfigs: [[
                                  url: 'https://github.com/pvc619/pipeline-simple-java-project.git', 
                                  credentialsId: 'git_test_pipeline',
                                  refspec: '+refs/heads/main:refs/remotes/origin/main'
                              ]],
                              browser: [$class: 'GithubWeb', url: 'https://github.com/pvc619/simple-java-project/blob/main/src/main/java/com/example/App.java']
                    ])
                } catch (Exception e) {
                    error("Checkout Failed: ${e.message}")
                }
            }

        }

    stage('Build') {
            steps {
                script {
                    // Define the path to Maven
                    def mavenHome = '/opt/maven'
                    def mavenCommand = "${mavenHome}/bin/mvn"
                    
                    // Execute Maven build with clean package goals
                    sh "${mavenCommand} -f /${WORKSPACE}/pom.xml clean package"
                } catch (Exception e) {
                    error("Build Failed: ${e.message}")
                }
            }
    }
    
    stage('Deploy to Nexus') {
            steps {
                script {
                    // Nexus Artifact Uploader configuration
                    nexusArtifactUploader( 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        nexusUrl: '3.110.221.63:8081', // Nexus repository URL
                        groupId: 'com.example', // Group ID of the artifact
                        version: '1.0-SNAPSHOT', // Version of the artifac
                        repository: 'Deepmatrix_test', // Repository in Nexus
                        credentialsId: 'df93fb3d-ad1c-450e-bd8f-e9642606b087', // Jenkins credentials ID for Nexus authentication
                        artifacts: [
                            [artifactId: 'pipeline-simple-java-project', file: '/var/lib/jenkins/workspace/pipeline-simple-java-project/target/pipeline-simple-java-project-1.0-SNAPSHOT.jar', type: 'jar'] // Path to the generated artifact
                            // Add more artifacts as needed
                        ]
                    )
                } catch (Exception e) {
                    error("Build Failed: ${e.message}")
                }
            }
        }
    

    }
    post {
        failure {
            script {
                def failureReason = currentBuild.description ?: 'Unknown'
                emailext(
                    to: "${env.EMAIL_RECIPIENTS}",
                    subject: "Jenkins Build Failure: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    body: """<p>Build failed in stage: ${currentBuild.currentResult}</p>
                             <p>Failure reason: ${failureReason}</p>
                             <p>Check console output for more details: ${env.BUILD_URL}console</p>"""
                )
            }
        }
    }

}






    
   /* insert Declarative Pipeline here */