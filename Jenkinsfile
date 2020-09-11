pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                    label 'docker'
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
                stash name: "jar", includes: "target/**/*.jar"
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/**/*.jar', fingerprint: true
                    archiveArtifacts artifacts: 'deployment.yml', fingerprint: true
                    archiveArtifacts artifacts: 'service.yml', fingerprint: true
                    archiveArtifacts artifacts: 'ingress.yml', fingerprint: true
                }
            }
        }
        stage('Dockerfile') {
            agent { label 'docker' }
            steps {
                script {
                    unstash 'jar'
                    docker.withRegistry('https://registry:5043') {
                        def customImage = docker.build("greeting:${env.BUILD_ID}")
                        customImage.push()
                        def customImageLatest = docker.build("greeting:latest")
                        customImageLatest.push()
                    }
                }
            }
        }
        stage('Deployment') {
            agent { label 'docker' }
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'ls -a'
                    sh 'kubectl apply -f deployment.yml'
                    sh 'kubectl apply -f service.ytml'
                    sh 'kubectl apply -f ingress.ytml'
                }
            }
        }
    }
}
