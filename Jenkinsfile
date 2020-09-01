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
    TOKEN=credentials('61544fac-49a8-491e-8250-b64414e3d65d')
  }

    stages {
      stage("Deploy") {
          steps {
              container('kubectl') {
                  sh '''
			source pro-graf.sh
			kubectl --token=$TOKEN get all -n monitor
		      '''
              }
          }
      }
}
}
