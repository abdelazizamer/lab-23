@Library('ivolve-lab22') _

pipeline {
    agent { label 'lab22-agent' }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build App') {
            steps {
                BuildApp()
            }
        }

        stage('Build Image, Scan and Push') {
            steps {
                BuildImage()
            }
        }

        stage('Update Manifest for Argo CD') {
            steps {
                DeployOnK8s()
            }
        }
    }

    post {
        success {
            echo 'Lab 23 GitOps pipeline completed successfully.'
        }
        failure {
            echo 'Lab 23 GitOps pipeline failed. Check the Console Output.'
        }
    }
}