
node{
   def app

   stage('Clone'){
      checkout scm
}

  stage('Build image'){
    app = docker.build("docker-app","./simple_api")
}

  stage('Run image'){
     docker.image('docker-app').withRun('-p 5000:80') { c ->

     sh 'docker ps'
   
     sh 'curl 127.0.0.1:5000'

   }
   }
}
