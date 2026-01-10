pipeline {
    agent any

    tools {
        maven 'Maven-3.9'   // use the exact Maven name from Jenkins
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Code Review (PMD)') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Coverage Analytics') {
            steps {
                sh 'mvn verify'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
