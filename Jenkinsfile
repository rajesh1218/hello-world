pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/rajesh1218/hello-world.git'
                sh "mvn clean install"
            }
        }
        stage('docker publish'){
            steps {
                script {
                    sh "docker login -u rajesh1218 -p Anki@1218"
                    sh "docker build -t tomcat1218 ."
                    sh "docker tag tomcat1218 rajesh1218/tomcat1218:latest"
                }
            }
        }
        stage('docker push') {
            steps {
                script {
                    sh "docker login -u rajesh1218 -p Anki@1218"
                    sh "docker push rajesh1218/tomcat1218:latest"
                }
            }
        }
        stage('deploy tomcat conatiner') {
            steps {
                sshagent(['docker-host']) {
                    sh "scp -o StrictHostKeyChecking=no webapp/target/webapp.war ec2-user@3.7.71.83:/opt/apache-tomcat-9.0.71/webapps"
                }
            }
        }
    }
}