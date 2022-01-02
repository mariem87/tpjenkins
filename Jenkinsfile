def commit_id
pipeline {
    agent any
    stages {
        
        stage('preparation') {
            steps {
                checkout scm
                sh "git rev-parse --short HEAD > .git/commit-id"
                script {
                    commit_id = readFile('.git/commit-id').trim()
                }
            }
        }
    
        stage ('build') {
            steps {
                echo 'building maven workload'
                sh "mvn clean install"
                echo 'build complete'
            }
        }
        //stage ('test') {
            //steps {
                //sh "mvn clean verify sonar:sonar   -Dsonar.projectKey=tracker   -Dsonar.host.url=http://localhost:9000   -Dsonar.login=e794c72e1ab136a7c0f603ff6746b807eecece9c"

            //}
        //}

        //stage ("image build") {
            //steps {
                //echo 'building docker image'
                //sh "docker build -t 2alinfo7/position-simulator:${commit_id} ."
                //echo 'docker image built'
            //}
        //}
stage ("image build") {
            steps {
                echo 'building docker image'
                //sh "docker build -t veteron90/tracker:${commit_id} ." 
                sh " docker build -t  192.168.222.157:8082/repository/mariemreponexus:${commit_id} ."
                //sh "docker push veteron90/tracker:${commit_id} "
                sh " docker push  192.168.222.157:8082/repository/mariemreponexus:${commit_id} "
                echo 'docker image built'
            }
        }
        stage ("Deploy") {
            steps {
                echo 'Deploying in k8s'
                sh "sed -i -r 's|richardchesterwood/k8s-fleetman-position-simulator:release2|position-simulator:${commit_id}|' workloads.yaml"
                sh "kubectl apply -f workloads.yaml"
                sh "kubectl apply -f services.yaml"
                sh "kubectl get all"
                echo 'Deployment complete'
            }
        }
    }
}
