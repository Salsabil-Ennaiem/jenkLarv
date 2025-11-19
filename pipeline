pipeline {
 agent any
 stages {
 stage('Docker') {
 steps {
 bat 'whoami'
 bat 'docker version'
 bat 'docker run --rm nginx:alpine nginx -v'
 }
 }
 }
}