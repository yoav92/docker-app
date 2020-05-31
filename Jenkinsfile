
node{
   def app


         stage('Clone'){
            checkout scm
      }

        stage('Build image'){
          app = docker.build("docker-app","./simple_api")
      }
  
  stage('Run image'){
     docker.image('docker-app').withRun('-p 8080:80') 
   }
      
    docker.image('nordri/clair-scanner').inside('--net ci') {

       stage ('Security scanner') {
           sh '''
             IP=$(ip r | tail -n1 | awk '{ print $9 }')
             /clair-scanner --ip ${IP} --clair=http://clair:6060 --threshold="Critical" docker-app
           '''
       }
   }
}
