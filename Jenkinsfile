podTemplate(
  yaml: '''
    apiVersion: v1
    kind: Pod
    metadata:
      name: kaniko
    spec:
      containers:
      - name: git
        image: bitnami/git
        command:
        - /bin/cat
        tty: true    
      - name: kaniko
        image: gcr.io/kaniko-project/executor:debug
        command:
        - /busybox/cat
        tty: true
  '''
){

  node(POD_LABEL){

    stage('checkout'){
      container('git'){
        script{
          sh "git clone https://github.com/mihirvijdeshpande/playjenkins.git"
            sh "cd playjenkins && git checkout kaniko"
        }
      }
    }
    stage('Kaniko Build & Push Image') {
      
        container('kaniko') {
          script {
            // sh "sleep 600"
            sh '''
            /kaniko/executor --dockerfile `pwd`/playjenkins/Dockerfile \
                             --context `pwd`/playjenkins \
                             --destination=image --no-push
            '''
            
          }
        }
      
    }

    // stage('Deploy App to Kubernetes') {     
      
    //     container('kubectl') {
    //       withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
    //         sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" myweb.yaml'
    //         sh 'kubectl apply -f myweb.yaml'
    //       }
    //     }
      
    // }

  }
     
  
  }
