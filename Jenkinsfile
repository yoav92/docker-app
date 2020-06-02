
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
      docker stop /clair
        docker rm /clair
        docker volume create --name clair-postgres
        docker stop /clair-postgres
        docker rm /clair-postgres
        docker run --detach --name clair-postgres --publish 5432:5432 --net ci --volume clair-postgres:/var/lib/postgresql/data arminc/clair-db:latest
        docker logs --tail 1 clair-postgres 
        curl --silent https://raw.githubusercontent.com/nordri/config-files/master/clair/config-clair.yaml | sed "s/POSTGRES_NAME/clair-postgres/" > config.yaml
        docker run --detach --name clair --net ci --publish 6060:6060 --publish 6061:6061 --volume ${PWD}/config.yaml:/config/config.yaml quay.io/coreos/clair:latest -config /config/config.yaml
        docker run --tty --rm --name clair-scanner --net ci -v /var/run/docker.sock:/var/run/docker.sock nordri/clair-scanner:latest /bin/bash
        # export IP=$(ip r | tail -n1 | awk '{ print $9 }')
        # /clair-scanner --ip ${IP} --clair=http://clair:6060 debian:jessie
      '''
     
   }
}
