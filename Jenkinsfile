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
   stage("docker_scan"){
      sh '''
        docker stop d2bc1bea8394
        docker rm d2bc1bea8394
    
        docker run -p 5432:5432 -d --name db arminc/clair-db
        sleep 15 # wait for db to come up
        docker stop 6f8949d2e026
        docker rm 6f8949d2e026
        docker run -p 6060:6060 --link db:postgres -d --name clair arminc/clair-local-scan:v2.0.6
        sleep 1
        DOCKER_GATEWAY=$(docker network inspect bridge --format "{{range .IPAM.Config}}{{.Gateway}}{{end}}")
        wget -qO clair-scanner https://github.com/arminc/clair-scanner/releases/download/v8/clair-scanner_linux_amd64 && chmod +x clair-scanner
        ./clair-scanner --ip="$DOCKER_GATEWAY" app 
      '''
     
   }
}
