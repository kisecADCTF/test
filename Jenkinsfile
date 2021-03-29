podTemplate(
    label: 'mypod',
    volumes : [
        emptyDirVolume(mountPath:'/etc/gitrepo', memory : false),
            hostPathVolume(mountPath:'/var/run/docker.sock', hostPath : '/var/run/docker.sock')
    ] ,
    containers :
            [
                containerTemplate(name:'git', image : 'alpine/git', ttyEnabled : true, command : 'cat'),
                containerTemplate(name:'docker', image : 'docker', ttyEnabled : true, command : 'cat')
            ]
        ) {
    node('mypod') {
        stage('Clone repository') {
            container('git') {
	   sh "echo 'nameserver 8.8.8.8' >> /etc/resolv.conf"
                sh 'git clone http://github.com/raxkson/kisec.git /etc/gitrepo'
            }
        }
        stage('Build and push docker image and remove image') {
            container('docker') {
                    sh 'docker build /etc/gitrepo/ -t test-php kisec/PHP/. --no-cache'
                    sh 'docker tag test-php myreg:30500'
                    sh 'docker push myreg:30500/test-php'
                    sh 'docker rmi test-php myreg:30500/test-php'
            }
        }
    }
}
