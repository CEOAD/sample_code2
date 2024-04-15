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
                bat 'pip install -r requirements.txt'
                echo 'Requirement complete.'
            }
        }
        stage('Code Quality') {
            steps {
                // Ensure pylint-fail-under is a Windows compatible script or command
                bat 'pylint-fail-under --fail_under 7 *.py'
            }
        }
        stage('Code Quantity') {
            steps {
                script {
                    def pythonFiles = findFiles(glob: '*.py')
                    def count = pythonFiles.length
                    echo "Number of Python files in the project: ${count}"
                    if (count != 8) {
                        error "Expected 8 Python files, but found ${count}"
                    }
                    pythonFiles.each {
                        echo "Python file: ${it.name}"
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
                bat 'python -m unittest test_book_manager.py'
            }
            post {
                always {
                    // Update test-results.xml path if necessary
                    junit 'test-results.xml'
                }
            }
        }
        stage('Package') {
            steps {
                script {
                    def pythonFiles = findFiles(glob: '*.py')
                    zip zipFile: 'package.zip', archive: true, glob: '*.py'
                }
                archiveArtifacts artifacts: 'package.zip', fingerprint: true
            }
        }
    }
    post {
        always {
            echo "Student Number: A01237887"
            echo "Group Number: [Your Group Number]" // Replace with your actual group number
        }
    }
}
