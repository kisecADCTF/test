pipeline {
    environment {
        registry = "test-php"
        registryCredential = 'myreg'
    }
    agent any
    stages {
        stage('Build docker image') {
            steps {
	   sh 'git clone https://github.com/raxkson/kisec'
                sh 'docker build -t $registry kisec/PHP/.'
	   sh 'docker tag $registry myreg:30500'
            }
        }
        stage('Deploy docker image') {
            steps {
                withDockerRegistry([ credentialsId: registryCredential, url: "" ]) {
                    sh 'docker push myreg:30500 $registry'
                }
            }
        }
        stage('Clean docker image') {
            steps{
                sh "docker rmi myreg:30500/$registry"
            }
        }
     }
}