pipeline {
    agent any 
    environment {
        USERNAME = credentials('USERNAME')
        PASSWORD = credentials('PASSWORD')
    }
    stages {
        stage('install kitops') {
            steps {
                cleanWs()
                git(url: 'https://github.com/shivay-couchbase/qwen.git', branch: 'main')
                sh 'ls'
                // sh 'curl -O https://github.com/jozu-ai/kitops/releases/latest/download/kitops-darwin-arm64.tar.gz'
                // sh 'tar -xzvf kitops-darwin-arm64.tar.gz'
                sh 'wget https://github.com/jozu-ai/kitops/releases/latest/download/kitops-linux-x86_64.tar.gz'
                sh 'tar -xvf ./kitops-linux-x86_64.tar.gz'
                sh './kit version'
            }
        }
        stage('Login to JozuHub'){
            steps {
                sh './kit login jozu.ml -u $USERNAME -p $PASSWORD'
                echo 'Successfully logged in to jozuhub'
            }
        }
        stage('Tagging and pushing to remote repository'){
            steps{
                sh './kit unpack jozu.ml/jozu/qwen2-0.5b:0.5b-instruct-q2_K --model -d models/qwen2-0_5b-instruct-q2_k.gguf'
                sh './kit pack . -t jozu.ml/shivaylamba/yolov3:latest'
                sh './kit push jozu.ml/shivaylamba/yolov3:latest'
            }
        }
    }
}
