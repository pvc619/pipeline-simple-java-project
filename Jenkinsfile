pipeline {
    agent any 

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
                    ]
                    
                  )
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
                        nexusUrl: 'http://nexus.example.com', // Nexus repository URL
                        groupId: 'com.example', // Group ID of the artifact
                        version: '1.0-SNAPSHOT', // Version of the artifac
                        repository: 'Deepmatrix_test', // Repository in Nexus
                        credentialsId: 'df93fb3d-ad1c-450e-bd8f-e9642606b087', // Jenkins credentials ID for Nexus authentication
                        artifacts: [
                            [artifactId: 'pipeline-simple-java-project', file: '${WORKSPACE}/target/pipeline-simple-java-project-1.0-SNAPSHOT.jar', type: 'jar'] // Path to the generated artifact
                            // Add more artifacts as needed
                        ]
                    )
                }
            }
        }
    

    }

}






    
   /* insert Declarative Pipeline here */