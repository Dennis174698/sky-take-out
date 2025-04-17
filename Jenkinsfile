pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk11'
    }
    environment {
        IMAGE_NAME = 'dennis9218/sky-server:latest'
        PATH = "/usr/local/bin:$PATH"
        DOCKER_USER = credentials('dockerhub-username')
        DOCKER_PASS = credentials('dockerhub-password')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Dennis174698/sky-take-out.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    echo "🔨 开始构建 Docker 镜像..."
            sh '''
                docker build -t $DOCKER_USER/sky-server:latest -f sky-server/Dockerfile .
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker push $DOCKER_USER/sky-server:latest
            '''
            echo "✅ Docker 镜像已成功推送！"
        }
    }
}

    }
}
