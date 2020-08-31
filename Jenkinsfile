pipeline {
      agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kubectl
    image: bryandollery/terraform-packer-aws-alpine
    command:
    - cat
    tty: true
  - name: helm
    image: alpine/helm
    command:
    - cat
    tty: true
"""
    }
  }
  environment {
    TOKEN=credentials('4b5c4b58-267c-4cf6-ab2a-4190bae53da7')
  }

    stages {
      stage("Deploy") {
          steps {
              container('kubectl') {
                  sh '''
		      kubectl --token=$TOKEN create namespace monitor       
		      '''
              }
          }
      }
        stage('helm-deploy') {
	  steps {
                  container('helm') {
                      sh '''
  			helm repo add stable https://kubernetes-charts.storage.googleapis.com
			helm repo update
			helm install prometheus-operator stable/prometheus-operator --namespace monitor --set grafana.service.type=NodePort
			kubectl apply -f ingress.yaml -n monitor
		    '''
                    }
            }
        }
    }
}
