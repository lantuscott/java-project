properties([pipelineTriggers([githubPush()])])

node('linux'){
    stage('Build'){
        git 'https://github.com/lantuscott/java-project.git'
        sh "ant"
    }
}

pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                sh 'ant -f test.xml -v'
            }
        }
        stage('Build') {
            steps {
                sh 'ant -f build.xml -v'
            }
        }
        stage('Deploy') {
            steps {
                sh 'aws s3 cp /workspace/java-pipeline/dist/* s3://seis665ust-mfarinha'
            }
        }
        stage('Report') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-jenkins', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
                }
            }
        }
    }
}
