pipeline {
  agent any
  
  stages {
    stage('Check Namespace') {
      steps {
        script {
          def ns = sh(returnStdout: true, script: "kubectl get ns wordp1 -o name || echo 'not found'")
          if (!sh(script: "kubectl get namespaces | grep -q ${namespace}", returnStatus: true)) {
            sh "kubectl create namespace $ns"
          }
        }
      }
    }
    
    stage('Deploy WordPress') {
      steps {
        script {
          def wp = sh(returnStatus: true, script: "helm list -n wordp1 | grep wordpress || echo 'not found'")
          def releaseName = 'final-project-wp-scalefocus'
          def chartPath = 'C:\Users\User\Desktop\Final_project_assesment\bitnami\wordpress'
          
          sh "helm upgrade --install ${releaseName} ${chartPath} -f values.yaml --namespace wp"
        }
      }
    }
  }
}
