node{
   def app
   def registryProjet='hub.docker.com/repository/docker/yoav92/docker-app'
   def image="${registryProjet}:version-${env.BUILD_ID}"

         stage('Clone'){
            checkout scm
      }

        stage('Build image'){
          app = docker.build("yoav92/docker-app:latest","./simple_api")
      }
  
  stage('Run image'){
     docker.image('yoav92/docker-app:latest').withRun('-p 8080:80') { c ->

     sh 'docker ps'
   
     sh 'curl -u toto:<token> -X GET http://0.0.0.0:8080/pozos/api/v1.0/get_student_ages'
      
  }

  
}
   stage("docker_scan"){
      sh '''
      docker stop /db
        docker rm /db
      docker run -d --name db arminc/clair-db:latest
       docker stop /clair
        docker rm /clair
      docker run -d --link db:postgres --name clair arminc/clair-local-scan:v2.0.6
      docker run --rm  -v /var/run/docker.sock:/var/run/docker.sock --network=container:clair ovotech/clair-scanner clair-scanner app || exit 0

      '''
     
   }
   
   stage('Push image') {
    docker.withRegistry('', 'reg1') {
      app.push()
    }
   }
   
   stage('Ansible'){
      sh 'ansible-playbook -i hosts.yml playbook.yml'
   }
}
