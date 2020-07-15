podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'git', image: 'alpine/git', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.4.8', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]
  ) {
    node('mypod') {
         
         def registry = "bibincatchme/nodejs"
       def registryCredential = 'dockerhub'
       def remoteImageTag  = "${env.BUILD_ID}"
        def dockerImage = ''

        
        stage('Clone repository') {
            container('git') {
                sh 'whoami'
                sh 'hostname -i'
                sh 'git clone -b master https://github.com/bibincatchme/k8s-nodejs.git'
            }
        }
            
        stage('Docker Build & Push') {
            container('docker') {
                 dir('k8s-nodejs') {
                // example to show you can run docker commands when you mount the socket
                sh 'hostname'
                sh 'hostname -i'
                dockerImage = docker.build registry + ":$BUILD_NUMBER"

                docker.withRegistry( '', registryCredential ) {
                dockerImage.push()
                dockerImage.push('latest')
            
            } }
        }
        }

        stage('Update ImageTag') {
                 dir('k8s-nodejs') {
                sh 'whoami'
                sh 'hostname -i'
                sh 'ls'
                sh "chmod +x changeTag.sh"
                sh "./changeTag.sh"
            }
        } 


        stage('K8s Deploy') {
            container('kubectl') {
                sh 'whoami'
                sh 'hostname -i'
               sh 'kubectl version --client'
                dir('k8s-nodejs/') {
          script {
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubeconfigid")
        }
                } }
            }

    } 
  }  
