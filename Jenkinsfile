pipeline {
    agent {
        docker {
            image 'mavenbuildcontainer:1.0'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code inside the Docker container
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Run the Python script without changing the directory
                sh "python3 /app/info.py"
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    // Run SonarQube analysis
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn clean install sonar:sonar'
                    }
                }
            }
        }

        stage('Publish Artifact') {
            steps {
                // Run the artifactory.py script inside the Docker container
                sh "python3 /app/artifactory.py"
            }
        }
    }
}
