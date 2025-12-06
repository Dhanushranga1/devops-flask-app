pipeline{
    agent any
    environment{
        GITHUB_REPO="https://github.com/Dhanushranga1/devops-flask-app.git"
        DOCKER_IMAGE="devops-flask:${BUILD_NUMBER}
        "
    }
    stages{
        stage('Checkout'){
            steps{
                echo"Cloning Repository..."
                git branch: 'main', url:env.GITHUB_REPO
            }
        }
        stage('Unit Tests'){
            steps {
                echo "Running Unit Tests..."
                sh '''
                cd app
                pip install -r requirements.txt
                python -m pytest tests/ || python tests/test_app.py
                
                '''
                        }
        }
        stage('Build Docker Image'){
            steps{
                echo "Building Docker Image..."
                sh'''
                cd app
                docker build -t ${DOCKER_IMAGE} .
                docker tag ${DOCKER_IMAGE} devops-flask:latest
                '''
            }
        }
        stage('Run Container Locally'){
            steps{
                echo "Running container..."
                sh '''
                docker stop flask-app||true
                docker rm flask-app||true
                docker run -d --name flask-app
                -p 5000:5000 devops-flask:latest
                '''
            }
        }
        stage('Health Check'){
            steps{
                echo "Checking health endpoint..."
                sh '''
                sleep 5
                curl -f http://localhost:5000/health
                '''
            }
        }

    }
    post {
        success{
            echo "Pipeline finished successfully"
        }
        failure{
            echo "Pipeline failed - check logs."
        }
        
    }
}