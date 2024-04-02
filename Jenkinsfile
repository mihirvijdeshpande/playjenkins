podTemplate(
  yaml: '''
    apiVersion: v1
    kind: Pod
    metadata:
      name: kaniko
    spec:
      containers:
      - name: kubectl
        image: joshendriks/alpine-k8s
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
    stage('Kaniko Build & Push Image') {
      
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile \
                             --context `pwd` \
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
