
pipeline {
    agent any
    stages {
         stage("Clone Repo") {
            steps {
                sh "rm -rf jenkinshtml || true"
                sh "git clone https://github.com/SivaKumar2606/jenkinshtml.git"
                //sh "cp /var/lib/jenkins/workspace/multibranchpipe/jenkinshtml/* /var/lib/jenkins/workspace/multibranchpipe/"
            }
         }
         stage("Removing Existing Image & Container") {
            steps {
                sh "docker -H tcp://10.1.1.5:2275 stop jk-masterweb || true"
                sh "docker -H tcp://10.1.1.5:2275 rmi -f sivakumar2606/multibranchpipemaster:v1 || true"
            }
         }
         stage("Building New DockerImage") {
            steps {
                //sh "cd /var/lib/jenkins/workspace/multibranchpipe/"
                sh "docker rmi -f sivakumar2606/multibranchpipemaster:v1 || true"
                sh "sleep 3s"
                sh "docker build -t sivakumar2606/multibranchpipemaster:v1 ."
            }
         }
         stage("Pushing the New Image to hub Registry") {
            steps {
               sh "docker push sivakumar2606/multibranchpipemaster:v1"
            }
        }
         stage("Deploy Container in Remote Node") {
            steps {
              sh "docker -H tcp://10.1.1.5:2275 run --rm -dit --name jk-masterweb --hostname jk-masterweb -p 9001:80 sivakumar2606/multibranchpipemaster:v1"
            }
        }
        stage("Check Reachability") {
            steps {
              sh "sleep 5s"
              sh "curl http://20.127.142.166:9001"
            }
       }
    }
}

