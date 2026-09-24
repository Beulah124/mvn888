pipeline {
    agent any

    tools {
        // Ensure 'Maven' and 'JDK' match the tool names configured in:
        // Manage Jenkins -> Tools (or Global Tool Configuration)
        maven 'Maven 3'
        jdk 'JDK 21'
    }

    environment {
        // Defines the app name or version if needed
        APP_NAME = 'demo8888'
    }

    stages {
        stage('Clean & Workspace Check') {
            steps {
                echo 'Cleaning up previous build workspace...'
                sh 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling Java source code...'
                sh 'mvn compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Executing JUnit tests...'
                // Allows build to continue gathering reports even if test step fails
                sh 'mvn test'
            }
            post {
                always {
                    // Publishes test results in Jenkins UI
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package Application') {
            steps {
                echo 'Building production JAR package...'
                // -DskipTests speeds up packaging since tests ran in the previous stage
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving built JAR files...'
                // Stores generated JAR file in Jenkins for download/deployment
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully for ${env.JOB_NAME} #${env.BUILD_NUMBER}!"
        }
        failure {
            echo "Pipeline failed. Please check the console logs for errors."
        }
    }
}
