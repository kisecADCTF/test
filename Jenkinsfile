node {
    def git_repo = 'kisec'
    def git_repo_php = 'kisec/php'
    def docker_regi = 'myreg:30500'
    def docker_img = 'test-php01'
    def helm_repo = 'test-php'
    
    
    
    stage('Clone repository') {
        try {
        sh 'git clone http://github.com/raxkson/' + git_repo
        }
        catch (e) {
            echo 'Already exits'
            sh 'git pull ' + git_repo
        }
    }

    stage('Build image') {
        sh 'docker build '+ git_repo_php + ' -t ' + docker_img + ' --no-cache'
    }
     
    stage('Tag image') {
		sh 'docker tag ' + docker_img + ' ' + docker_regi + '/'+ docker_img +':${BUILD_NUMBER}'
    }

    stage('Push image') {
		sh 'docker logout'
		sh 'docker login '+ docker_regi +' -u kisec -p kisec1234'
		sh 'docker push ' + docker_regi + '/' + docker_img + ':${BUILD_NUMBER}'
    }
    
    stage('Remove docker image') {
        try {
            sh 'sudo docker rmi ' + docker_regi + '/' + docker_img ':${BUILD_NUMBER}'
            sh 'docker rmi $(docker images --filter "dangling=true" -q --no-trunc)'
        }
        catch (e) {
            echo 'No images'
        }
    }
    
    stage('Helm install') {
        sh'kubectl get pods'
        try {
            sh 'helm uninstall ' + helm_repo 
        }
        catch (e) {
            echo 'Helm not exists'
        }
        sh 'helm create ' + helm_repo
        sh 'echo "appVersion: latest" >> ' + helm_repo + '/Chart.yaml'
        sh 'helm install --set image.repository=' + docker_regi + '/' + docker_img +',image.pullPolicy=Always,service.type=NodePort ' + helm_repo + ' ' + helm_repo
    }
    
    stage('Helm get port') {
        try {
            sh 'kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services ' + helm_repo
        }
        catch (e) {
            echo 'Check helm exits'
            sh 'helm list'
        }
    }
	
	
}