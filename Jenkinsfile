pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Choose environment to deploy')
    }

    triggers {
        // Poll SCM काढून फक्त webhook वापरायचं असेल तर इथे काहीच ठेवू नकोस
         pollSCM('H/5 * * * *') // जर backup म्हणून हवे असेल तर ठेऊ शकतो
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Harshvardhanpingane/UsePipeline.git'
            }
        }
        stage('Build') {
          steps {
            bat 'exit /b 1'
        }


        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/*.txt', onlyIfSuccessful: true
                echo "📦 All artifacts archived"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def deployDir = "C:\\deploy\\${params.ENV}"
                    bat """
                        if not exist "${deployDir}" mkdir "${deployDir}"
                        copy build\\*.txt "${deployDir}\\"
                    """
                    echo "🚀 Artifacts copied to ${deployDir}"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline succeeded for ENV = ${params.ENV}"
            emailext(
                subject: "Jenkins SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Good news!\nBuild for ENV = ${params.ENV} succeeded.\nArtifacts deployed to C:\\deploy\\${params.ENV}",
                to: "harshvardhanpingane2002@gmail.com"
            )
        }
        failure {
            echo "❌ Pipeline failed!"
            emailext(
                subject: "Jenkins FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build for ENV = ${params.ENV} failed.\nCheck Jenkins console output.",
                to: "harshvardhanpingane2002@gmail.com"
            )
        }
    }
}
