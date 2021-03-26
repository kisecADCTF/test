pipeline {
    environment {
        registry = "kisec/test-php"
        registryCredential = 'myreg'
    }
    agent any
    stages {
        stage('Build docker image') {
            steps {
	   sh 'git clone https://github.com/raxkson/kisec'
                sh 'docker build -t $registry:latest kisec/PHP/.'
	   sh 'docker tag myreg:30500 $registry:latest'
            }
        }
        stage('Deploy docker image') {
            steps {
                withDockerRegistry([ credentialsId: registryCredential, url: "" ]) {
                    sh 'docker push $registry:latest'
                }
            }
        }
        stage('Clean docker image') {
            steps{
                sh "docker rmi $registry"
            }
        }
     }
}