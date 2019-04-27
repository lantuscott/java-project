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
                sh 'aws s3 cp /workspace/JavaPipeline/reports/* s3://seis665ust-mfarinha'
            }
        }
        stage('Report') {
            steps {
                echo 'Reporting....'
            }
        }
    }
}
