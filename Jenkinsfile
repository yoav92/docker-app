
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
   
     sh 'curl -u toto:python -X GET http://user:python@192.168.232.140/job/GitTrigger/5/pozos/api/v1.0/get_student_ages'

   }
   }
}
