pipeline {
    agent any

    environment {
        GRADLE_HOME = '/usr/local/gradle'
        PATH = "${GRADLE_HOME}/bin:${env.PATH}"
        GITHUB_TOKEN = credentials('7da4655a-cfd6-4983-ad62-08f7fbfe6c34')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Coverage') {
            steps {
                sh './gradlew jacocoTestReport'
            }
        }

        stage('Verify Coverage') {
            steps {
                sh './gradlew check'
            }
        }
    }

    post {
        always {
            junit '**/build/test-results/**/*.xml'
            jacoco execPattern: '**/build/jacoco/*.exec', classPattern: '**/build/classes/java/main', sourcePattern: 'src/main/java'
        }
        failure {
            echo 'Build failed!'
        }
        success {
            echo 'Build succeeded!'
        }
    }
}