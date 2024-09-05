pipeline {
    agent any
    parameters {
        choice choices: ['Apply', 'Destroy', 'Plan'], description: 'Choose Terraform Action to perform, use destroy with caution', name: 'Action'
        }
    stages {
        stage('tf-mainfest-checkout') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/pankaj0825/tf-vpc-module.git'
            }
        }
        stage('terraform fmt') {
            steps {
                sh 'terraform fmt'
            }
        }
        stage('terraform init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('terraform plan') {
            when { expression { params.Action == 'Plan' || params.Action == 'Apply'} }
            steps {
                sh 'terraform plan'
            }
        }
        stage('terraform apply') {
            when { expression { params.Action == 'Apply' } }
            steps {
                input message: 'Do you want to apply the Terraform changes?', ok: 'Apply'
                sh 'echo "yes" | terraform apply'
            }
        }
        stage('terraform destroy-plan') {
            when { expression { params.Action == 'Destroy'} }
            steps {
                sh 'terraform plan -destroy'
            }
        }
        stage('terraform destroy') {
            when { expression { params.Action == 'Destroy' } }
            steps {
                input message: 'Do you want to destroy the Terraform changes?', ok: 'Destroy'
                sh 'echo "yes" | terraform destroy'
            }
        }
    }
    post {
        always {
            // Clean up the workspace after the pipeline finishes
            cleanWs()
        }
    }
}
