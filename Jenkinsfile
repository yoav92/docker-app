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
   
docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /lib64/libdevmapper.so.1.02:/usr/lib/x86_64-linux-gnu/libdevmapper.so.1.02 -v /lib64/libudev.so.0:/usr/lib/x86_64-linux-gnu/libudev.so.0 --privileged=true myorg/clairscan ./clair-scanner --clair="http://$myip:6060" --ip=$myip $animage
      '''
     
   }
}
