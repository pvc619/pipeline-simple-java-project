pipeline {
    agent any  


    stages {


        stage('Checkout') {
            steps {
                script {
                    //sh "git config --global credential.username ${env.GIT_USERNAME}"
                    //sh "git config --global credential.helper store" // Save credentials temporarily
                    //sh "git checkout https://yourusername@github.com/yourusername/yourrepository.git"
                    git branch: 'main', credentialsId: 'test-pipeline-credentials', url: 'https://github.com/pvc619/pipeline-simple-java-project.git'
                    //withCredentials([usernamePassword(credentialsId: 'test-pipeline-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        //sh "git config --global credential.helper store" // Save credentials temporarily
                        //sh "git checkout https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/pvc619/pipeline-simple-java-project.git"
                    
                    // Read the content of the file
                    def fileContent = readFile 'pipeline-simple-java-project/src/main/java/com/example/App.java'  // Specify the path to your file
                    echo "Content of the file:"
                    echo fileContent               
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
                    sh "${mavenCommand} -f /pipeline-simple-java-project/pom.xml clean package"
                }
            }
    }


    
   /* insert Declarative Pipeline here */