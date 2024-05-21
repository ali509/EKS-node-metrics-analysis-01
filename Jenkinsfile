pipeline {
    agent any
    environment {
        JOB_NAME = "eks"  // Set JOB_NAME to the actual job name
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }
    stages {
        stage('GIT Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/ali509/EKS-node-metrics-analysis-01.git'
            }
        }
        stage('Helm Package') {
            steps {
                script {
                    dir("""/var/lib/jenkins/workspace/${env.JOB_NAME}/node-exporter/""") {
                        catchError {
                          sh 'helm package .'
                        }
                  }
                }
            }
        }
        stage('Helm Deploy'){
            steps {
                dir("""/var/lib/jenkins/workspace/${env.JOB_NAME}/node-exporter/""") {
                        catchError {
                            sh 'helm upgrade node-exporter .'
                        }
                        sh 'rm -rf *.tgz'
                  }
            }
        }
    }
    
    post {
        success {
            cleanWs()
        }
    }
}

