// Jenkinsfile for Python application
// Author: Amin Davari A01237887
// Date: 04/15/2024
// Description: Jenkins pipeline for building, testing, and packaging a Python application.

pipeline {
    agent any
    parameters {
        booleanParam(defaultValue: false, description: 'Run Unit Tests', name: 'TEST')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Installing the Python Requirements...'
                sh 'pip install -r requirements.txt'
                echo 'Requirement complete.'
            }
        }
        stage('Code Quality') {
            steps {
                // Assuming pylint-fail-under is a script or command available in your environment
                sh 'pylint-fail-under --fail_under 7 *.py'
            }
        }
        stage('Code Quantity') {
            steps {
                script {
                    def count = sh(script: "ls -1 *.py | wc -l", returnStdout: true).trim()
                    echo "Number of Python files in the project: ${count}"
                    if (count.toInteger() != 8) {
                        error "Expected 8 Python files, but found ${count}"
                    }
                    // Groovy for-loop to iterate through each Python file and display its name
                    def files = sh(script: "ls -1 *.py", returnStdout: true).trim().split("\n")
                    for (file in files) {
                        echo "Python file: ${file}"
                    }
                }
            }
        }
        stage('Test') {
            when {
                expression {
                    return params.TEST
                }
            }
            steps {
                sh 'python -m unittest test_book_manager.py'
            }
            post {
                always {
                    junit 'test-results.xml' // Make sure your tests output a JUnit compatible XML report
                }
            }
        }
        stage('Package') {
            steps {
                sh 'zip package.zip *.py'
                archiveArtifacts artifacts: 'package.zip', fingerprint: true
            }
        }
    }
    post {
        always {
            echo "Student Number: [Your Student Number]"
            echo "Group Number: [Your Group Number]"
        }
    }
}
