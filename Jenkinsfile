pipeline {
    agent {
        agent { label 'jenkins-slave-label' } // Specify the label of your EC2 agent here
    }
    stages {
        stage('Docker Build and Run') {
            steps {
                script {
                    def dockerImage = 'public.ecr.aws/docker/library/maven:3.9-sapmachine'
                    
                    // Pull the Docker image on the EC2 agent
                    sh "docker pull $dockerImage"
                    
                    // Run your pipeline inside a Docker container on the EC2 agent
                    sh "docker run --rm -v ${env.WORKSPACE}:/workspace -w /workspace $dockerImage mvn --version"
                    sh "docker run --rm -v ${env.WORKSPACE}:/workspace -w /workspace $dockerImage git --version"
                    git branch: 'main',
                        url: 'https://github.com/LinkedInLearning/essential-jenkins-2468076.git'
                }
            }
        }
        stage('Clean') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn clean'
                }
            }
        }
        stage('Test') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn test'
                }
            }
        }
        stage('Package') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn package -DskipTests'
                }
            }
        }
    }
}
