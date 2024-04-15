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
                        def command = "pylint --fail-under=8 ${file}"
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
                    // Check if package.zip exists and remove it if it does
                    if (fileExists('package.zip')) {
                        bat 'del /F package.zip'
                    }
                    zip zipFile: 'package.zip', glob: '**/*.py'
                }
                // Archive the package.zip file as an artifact
                archiveArtifacts artifacts: 'package.zip', fingerprint: true
            }
        }
    }
    post {
        always {
            echo "Student Number: A01237887"
            echo "Group Number: 44"
        }
    }
}
