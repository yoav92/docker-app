
node{
   def app
  

         stage('Clone'){
            checkout scm
      }

        stage('Build image'){
          app = docker.build("docker-app","./simple_api")
      }
  
  stage('Run image'){
     docker.image('docker-app').withRun('-p 8080:80') { c ->

     sh 'docker ps'
   
     sh 'curl -u toto:python -X GET http://localhost:8080/pozos/api/v1.0/get_student_ages'

   }
   }
      
      stage('docker_scan'){
      sh 'docker ps'
      sh 'docker stop 4b652ad831b4b1b9f8b304f238b48173a17bc1bb40922561484e06ce5ba77d55'
      sh 'docker rm 4b652ad831b4b1b9f8b304f238b48173a17bc1bb40922561484e06ce5ba77d55'
      sh 'docker stop 73e354267ac9553110663d4d67fa37cdf6ca40c379beac3f64307d4c88d99c93'
      sh 'docker rm 73e354267ac9553110663d4d67fa37cdf6ca40c379beac3f64307d4c88d99c93'
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
