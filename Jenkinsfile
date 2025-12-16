pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning source code from GitHub...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project using Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo 'Deploying application to AWS EC2...'
                sshagent(['ec2-key']) {
                    sh '''
                    scp target/jenkins-demo.jar ubuntu@EC2_PUBLIC_IP:/home/ubuntu/app/
                    ssh ubuntu@EC2_PUBLIC_IP "java -jar /home/ubuntu/app/jenkins-demo.jar &"
                    '''
                }
            }
        }
    }
}
