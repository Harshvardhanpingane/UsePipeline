pipeline {
    agent any
    
    parameters {
    choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Choose target environment')
  }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Harshvardhanpingane/UsePipeline.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                // Example: sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Tests...'
                // Example: sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Example: sh './deploy.sh'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
