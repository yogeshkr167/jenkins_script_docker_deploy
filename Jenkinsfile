pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('clone_project_A') {
            steps {
                echo 'clone project A'
                git 'https://github.com/vincloud2/Helloworld-latest.git'
            }
        }
        stage('build_project_A') {
            steps {
                echo 'build_projectA'
                sh 'apt install maven -y'
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
        } 
        stage('Docker_build') {
            steps {
                echo 'Docker build_projectA'
                sh 'docker build -t projecta .' 
            }
        }
        stage('login to dockerhub') {
            steps {
                echo 'login to dockerhub'
                sh 'docker login -u vnom1985 -p abc@12345'
            }
        } 
        stage('Tag the Image') {
            steps {
                echo 'Tag the Image'
                sh 'docker tag  projecta vnom1985/projecta'
            }
        } 
        stage('Deploy to docker hub') {
            steps {
                echo 'Deploy to docker hub'
                sh 'docker push vnom1985/projecta'
            }
        }
        stage('Remove Docker conatiner') {
            steps {
                echo 'Remove Docker conatiner'
                sh 'docker stop projecta_conatiner || true'
                sh 'docker rm projecta_conatiner || true'
            }
        }        
        stage('Run docker image') {
            steps {
                echo 'Deploy to docker hub'
                sh 'docker run --name projecta_conatiner -d -p 8181:8080 vnom1985/projecta'
            }
        }        
    }
}
