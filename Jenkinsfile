pipeline {
    agent {
        node {
            label 'js-1'
        }
    }
    environment {
        PATH = "/opt/apache-maven-3.9.12/bin:$PATH"
        IMAGE_NAME = "venkatasai9876/ttrend"
        VERSION    = "2.0.${BUILD_NUMBER}"
    }
    stages {
        stage("Build") {
            steps {
                echo "----------- Build Started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----------- Build Completed ----------"
            }
        }
        stage("Test") {
            steps {
                echo "----------- Unit Test Started ----------"
                sh 'mvn surefire-report:report'
                echo "----------- Unit Test Completed ----------"
            }
        }
        stage("Docker Build") {
            steps {
                script {
                    echo '<--------------- Docker Build Started --------------->'
                    
                    // Get short Git SHA for traceable tag
                    def gitSha = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    env.GIT_SHA = gitSha
                    
                    // Build Docker image with VERSION tag
                    app = docker.build("${env.IMAGE_NAME}:${env.VERSION}")
                    
                    echo '<--------------- Docker Build Completed --------------->'
                }
            }
        }
        stage("Docker Publish") {
            steps {
                script {
                    echo '<--------------- Docker Publish Started --------------->'

                    docker.withRegistry('', 'docker') {
                        // Push VERSION tag
                        app.push("${env.VERSION}")

                        // Push latest tag
                        app.push("latest")

                        // Push Git SHA tag for traceability
                        app.push("${env.GIT_SHA}")
                    }

                    echo '<--------------- Docker Publish Completed --------------->'
                }
            }
        }
    }
}
