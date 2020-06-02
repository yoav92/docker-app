
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
     git clone https://github.com/Chathuru/clair-scanner.git
     helm install <RELEASE_NAME> clair
     wget https://github.com/arminc/clair-scanner/releases/download/v12/clair-scanner_linux_amd64
 
mv clair-scanner_linux_amd64 clair-scanner
 chmod +x clair-scanner
      '''
     
   }
}
