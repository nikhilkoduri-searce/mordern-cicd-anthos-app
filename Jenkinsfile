pipeline {
  agent any
  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('set PATH env') {
      steps {
        sh 'export KUSTOMIZATION_PATH_STG=./k8s/stg'
        sh 'export KUSTOMIZATION_PATH_PROD=./k8s/prod'
      }
    }

    stage('hydrate-manifests-stg') {
      steps {
          sh 'mkdir -p hydrated-manifests/'
          sh 'sudo kustomize build k8s/stg/ -o hydrated-manifests/stg.yaml'
        }
    }

    stage('hydrate-manifests-prod') {
      steps {
          sh 'sudo kustomize build k8s/prod/ -o hydrated-manifests/prod.yaml'
        }
    }

    stage('push hydrated manifests') {
      steps {
          sh 'git clone git@github.com:nikhilkoduri-searce/modern-cicd-anthos-env.git'
          sh 'cd modern-cicd-anthos-env'
          sh 'cp ../hydrated-manifests/stg.yaml stg.yaml'
          sh 'cp ../hydrated-manifests/prod.yaml prod.yaml'
          sh 'git add stg.yaml prod.yaml'
          sh 'git commit -m "updates"'
          sh 'git push origin staging'
        }
    }

  }
}
