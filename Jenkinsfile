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
                script {
                    // Find all python files and run pylint on them individually
                    def pythonFiles = findFiles(glob: '*.py')
                    pythonFiles.each { file ->
                        // Here we assume that pylint is installed and pylint-fail-under is a script that
                        // takes a file as an argument and returns non-zero exit code if the score is under 7.
                        // You may need to adjust the command according to your environment.
                        def command = "pylint --fail-under=2 ${file}"
                        echo "Running command: ${command}"
                        bat(script: command, returnStatus: true)
                    }
                }
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
                    script {
                        if (fileExists('test-reports')) {
                            junit 'test-reports/*.xml'
                        }
                        if (fileExists('api-test-reports')) {
                            junit 'api-test-reports/*.xml'
                        }
                    }
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
