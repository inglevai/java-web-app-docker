node{
    def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git', branch: 'master'
        
    }
    
    stage("Maven Clean Package"){
        def mavenHome= tool name: "Maven",type: "maven"
         sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage("Build Docker image"){
        sh "docker build -t vdockerprctice/java-web-app-docker:${BUILD_NUMBER} ."
    }
    
    stage("Docker login and push"){
        withCredentials([string(credentialsId: 'DockerHubPasswd', variable: 'DockerHubPasswd')]) {
             sh "docker login -u vdockerprctice -p ${DockerHubPasswd}"
         }
        
        sh "docker push vdockerprctice/java-web-app-docker:${BUILD_NUMBER}" 
    }
    
    stage("Deploy Application As docker container in Docker Deployment Server"){
        sshagent(['Docker_Dev_Server_SSH']) {
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.164 docker rm -f javawebappcontainer || true"
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.164 docker run -d -p 8080:8080 --name javawebappcontainer vdockerprctice/java-web-app-docker:${BUILD_NUMBER}"
      }
    }
}
