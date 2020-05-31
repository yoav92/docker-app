
node{
   def app


         stage('Clone'){
            checkout scm
      }

        stage('Build image'){
          app = docker.build("docker-app","./simple_api")
      }
  
  stage('Run image'){
     docker.image('docker-app')
   }
      
   stage("docker_scan"){
      sh 'docker stop d3e1d4789c08f1ff0b401a156f8650751f312a878f817be60223addd16ede823'
      sh 'docker rm d3e1d4789c08f1ff0b401a156f8650751f312a878f817be60223addd16ede823'
      sh '''
        docker run -d --name db arminc/clair-db
        sleep 15 # wait for db to come up
        docker run -p 6060:6060 --link db:postgres -d --name clair arminc/clair-local-scan
        sleep 1
        DOCKER_GATEWAY=$(docker network inspect bridge --format "{{range .IPAM.Config}}{{.Gateway}}{{end}}")
        wget -qO clair-scanner https://github.com/arminc/clair-scanner/releases/download/v8/clair-scanner_linux_amd64 && chmod +x clair-scanner
        ./clair-scanner --ip="$DOCKER_GATEWAY" docker-app || exit 0
      '''
    }
}
