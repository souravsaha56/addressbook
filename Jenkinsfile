pipeline {
    agent any

    tools {
        maven 'mymaven'   // Change to your configured Maven name in Jenkins
        jdk 'JDK-17'        // Change if you use a different JDK
    }

    environment {
        MAVEN_OPTS = "-Xmx1024m"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling source code...'
                sh 'mvn compile'
            }
        }

        stage('Code Review (PMD)') {
            steps {
                echo 'Running static code analysis using PMD...'
                sh 'mvn pmd:pmd'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/site/pmd.xml', allowEmptyArchive: true
                }
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Executing unit tests...'
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
                echo 'Running code coverage analysis...'
                sh 'mvn verify'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/site/**', allowEmptyArchive: true
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                sh 'mvn package'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully ✅'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
        always {
            cleanWs()
        }
    }
}

