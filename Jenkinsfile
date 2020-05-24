
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
   
     sh 'curl -u -X GET http://0.0.0.0:8080/pozos/api/v1.0/get_student_age'

   }
   }
}
