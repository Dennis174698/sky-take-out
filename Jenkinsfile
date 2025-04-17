pipeline {
    agent any

    tools {
        maven 'maven'      // 在 Jenkins -> Global Tool Configuration 中配置
        jdk 'jdk11'        // 同上，确保 ID 是 'jdk11'
    }

    environment {
        IMAGE_NAME = 'dennis9218/sky-server:latest'
        PATH = "/usr/local/bin:$PATH"
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
            environment {
                DOCKER_USER = credentials('dockerhub-username')   // 这两项必须在这个 stage 内 environment 块里引用
                DOCKER_PASS = credentials('dockerhub-password')
            }
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
