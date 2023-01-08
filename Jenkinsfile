pipeline {
    agent { label 'NODE1' }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('vcs') {
            steps {
                git branch: 'develop',
                    url: 'https://github.com/srikanth458/StudentCoursesRestAPI.git'
            }
        }
        stage('build and deploy') {
            steps {
                sh "docker image build -t srikanth458/courses ."
                sh "docker image push srikanth458/courses:develop-$env.BUILD_ID"
            }
        }
        stage('deploy') {
            steps {
                sh "cd deployments/courses/overlays/develop && kustomize edit set image courses=srikanth458/courses:develop-$env.BUILD_ID"
                sh 'kubectl apply -k deployments/courses/overlays/develop'
            }
        }

    }
}
