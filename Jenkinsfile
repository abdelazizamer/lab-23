@Library('ivolve-lab23') _

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
                buildApp()
            }
        }

        stage('Build Image, Scan and Push') {
            steps {
                buildImage()
            }
        }

        stage('Update Manifest for Argo CD') {
            steps {
                deployOnK8s()
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
