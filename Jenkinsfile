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
        sh 'eval "$(ssh-agent -s)"'
        sh 'ssh-add ~/anthos/anthos-aws/acm-demo'
      }
    }

    stage('hydrate-manifests-stg') {
      steps {
          sh 'ls -l'
          sh 'mkdir -p hydrated-manifests/'
          sh 'cd ./k8s/stg'
          sh 'kustomize build . -o ../../hydrated-manifests/stg.yaml'
          sh 'cd ../../'
        }
    }

    stage('hydrate-manifests-prod') {
      steps {
          sh 'cd ./k8s/prod'
          sh 'kustomize build . -o ../../hydrated-manifests/prod.yaml'
          sh 'cd ../../'
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
