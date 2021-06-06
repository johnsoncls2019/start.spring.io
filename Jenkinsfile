node {
stage ('SCM Checkout' ) {
git credentialsId: 'git_creds2', url: 'https://github.com/johnsoncls2019/new_spring_project.git'
}
stage ('Mvn package') {
def mvnHome = tool name: 'localMaven', type: 'maven' 
def mvnCMD = "${mvnHome}/usr/bin/mvn"
sh " mvn clean package"
}
stage ('Build Docker image') {
sh "docker build -t springboot1 ."
}
stage ('Push docker image') {
withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
sh "docker login -u johnsoncls2019 -p ${dockerHubPwd}"
}
sh 'docker push johnsoncls2019/springboot1'
}
stage ('Run Container on AWS Server') {
def dockerRemove = 'docker rm --force AchiStarTechnologies'
def dockerRun = 'docker run -p 5000:5000 -d -t --name AchistarTecnologies2 johnsoncls2019/springboot1'
withCredentials([aws(accessKeyVariable: 'AKIAZEX4M52X46CTAZUA', credentialsId: 'aws_server', secretKeyVariable: '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')]) {
"${dockerRemove}"
"${dockerRun}"
}
}
stage ('Run container on Dev server') {
def dockerRemove = 'docker rm --force AchiStarTechnologies1'
def dockerRun = 'docker run -p 5000:5000 -d  -t --name AchiStarTechnologies1 johnsoncls2019/springboot1'
sshagent(['dev-server']) {
sh "ssh -o StrictHostKeyChecking=no root@192.168.44.129 ${dockerRemove}"
sh "ssh -o StrictHostKeyChecking=no root@192.168.44.129 ${dockerRun}"
}
}
}
