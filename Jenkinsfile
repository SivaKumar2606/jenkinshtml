// git rep: jenkinshtml Branch: dev

pipeline {
    agent any
    stages {
         stage("Clone Repo") {
            steps {
                sh "rm -rf jenkinshtml || true"
                sh "git clone https://github.com/SivaKumar2606/jenkinshtml.git"
                sh "cp /var/lib/jenkins/workspace/multibranchpipe/jenkinshtml/* /var/lib/jenkins/workspace/multibranchpipe/"
            }
         }
         stage("Removing Existing Image & Container") {
            steps {
                sh "docker -H tcp://10.1.1.5:2375 stop jk-devweb || true"
                sh "docker -H tcp://10.1.1.5:2375 rmi -f sivakumar2606/multibranchpipedev:v1 || true"
            }
         }
         stage("Building New DockerImage") {
            steps {
                sh "cd /var/lib/jenkins/workspace/multibranchpipe/"
                sh "docker rmi -f sivakumar2606/multibranchpipedev:v1 || true"
                sh "sleep 3s"
                sh "docker build -t sivakumar2606/multibranchpipedev:v1 ."
            }
         }
         stage("Pushing the New Image to hub Registry") {
            steps {
               sh "docker push sivakumar2606/multibranchpipedev:v1"
            }
        }
         stage("Deploy Container in Remote Node") {
            steps {
              sh "docker -H tcp://10.1.1.5:2375 run --rm -dit --name jk-devweb --hostname jk-devweb -p 9002:80 sivakumar2606/multibranchpipedev:v1"
            }
        }
        stage("Check Reachability") {
            steps {
              sh "sleep 5s"
              sh "curl http://20.127.142.166:9002"
            }
       }
    }
}

