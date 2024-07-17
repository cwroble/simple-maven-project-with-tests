pipeline {
    agent none
    stages {
        stage('maven') {
            agent {
                kubernetes { 
                    yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.9.4-eclipse-temurin-11
    command:
    - sleep
    args:
    - infinity
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop:
        - ALL
      runAsNonRoot: true
      runAsUser: 1000
      seccompProfile:
        type: RuntimeDefault
'''
                    retries 2
                }
            }
            steps {
                container('maven') {
                    sh 'mvn -B -ntp -Dmaven.test.failure.ignore verify'
                }
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }
    }
}
