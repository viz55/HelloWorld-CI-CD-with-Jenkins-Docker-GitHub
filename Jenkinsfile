pipeline {
    agent none

    stages {
        stage('Build') {
            agent {
                docker { image 'openjdk:17-alpine' }
            }
            steps {
                echo 'Compiling Java Program inside Docker container...'
                sh 'javac hello_world.java'
            }
        }

        stage('Run') {
            agent {
                docker { image 'openjdk:17-alpine' }
            }
            steps {
                echo 'Running Java Program inside Docker container...'
                sh 'java HelloWorld'
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed. Container cleaned up automatically.'
        }
    }
}
