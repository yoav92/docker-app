
node{
   def app
   docker.image('docker').inside('-v /var/run/docker.sock:/var/run/docker.sock') {
         stage('Clone'){
            checkout scm
      }

        stage('Build image'){
          app = docker.build("docker-app","./simple_api")
      }
   }
  


  
}
   docker.image('nordri/clair-scanner').inside('--net ci') {

       stage ('Security scanner') {
           sh '''
             IP=$(ip r | tail -n1 | awk '{ print $9 }')
             /clair-scanner --ip ${IP} --clair=http://clair:6060 --threshold="Critical" app
           '''
       }
}
}
