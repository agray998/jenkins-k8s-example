pipeline {
  agent any
  stages {
/*
    stage('Install CLIs') {
      steps {
        sh "sudo chmod +x *.sh"
        sh "./install-cli.sh"
        sh "./install-eksctl.sh"
        sh "./install-kubectl.sh"
      }
    }
*/
    stage('Configure kubectl') {
      steps {
        sh '''
        if [ ${env.BRANCH_NAME} == main ]; then
          export NAMESPACE=production
        else
          export NAMESPACE=staging
        fi
        '''
        sh "aws eks --region eu-west-2 update-kubeconfig --name exerciseCluster"
        sh "kubectl config set-context --current --namespace=${env.NAMESPACE}"
      }
    }
    stage('Deploy manifests') {
      steps {
        sh "kubectl apply -f back.yml"
        sh "kubectl apply -f front.yml"
        sh "kubectl get services"
      }
    }
  }
}
