def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]






pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                echo 'Cloning project codebase...'
                git branch: 'main', url: 'https://github.com/DoreenLem/airbnb-infrstructure.git'
                sh 'ls'
            }
        }
        
        stage('verify terraform version') {
            steps {
                echo 'verifying the terrform version...'
                sh 'terraform --version'
            }
        }
        stage('Terraform init') {
            steps {
                echo 'Initializibg terraform project...'
                sh 'terraform init'
            }
        }
        stage('Terraform validate') {
            steps {
                echo 'Initializibg terraform project...'
                sh 'terraform init'
            }
        }
        stage('Terraform plan') {
            steps {
                echo 'Terraform plan for dry run...'
                sh 'terraform validate'
            }
        }
         stage('checkov scan') {
            steps {
                
                sh """
                sudo pip3 install checkov
                
                checkov -d . --skip-check CKV_AWS_79
                """
            }
        }
        stage('Manual approval') {
            steps {
                echo 'Terraform plan for dry run...'
                sh 'terraform validate'
                input 'Approval required for deployment'
            }
        }
         stage('Terraform apply') {
            steps {
                echo 'Terraform apply..'
                sh 'sudo terraform apply --auto-approve'
            }
        }
    }
    post {
        always {
            echo 'I will always say Hello again!'
            slackSend channel: '#glorious-w-f-devops-alerts', color: COLOR_MAP[currentBuild.currentResult], message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
}