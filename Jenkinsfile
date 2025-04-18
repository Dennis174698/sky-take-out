pipeline {
    agent any

    tools {
        maven 'maven'      // 在 Jenkins -> Global Tool Configuration 中配置
        jdk 'jdk11'        // 同上，确保 ID 是 'jdk11'
    }

    environment {
        IMAGE_NAME = 'sky-server:latest'  // Docker Hub 上的镜像名称
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
                DOCKER_USER = credentials('dockerhub-username')   // 确保此凭证配置正确
                DOCKER_PASS = credentials('dockerhub-password')   // 确保此凭证配置正确
            }
            steps {
                script {
                    echo "🔨 开始构建 Docker 镜像..."

                    // 输出凭证变量确认是否正确
                    echo "DOCKER_USER=${DOCKER_USER}"
                    echo "DOCKER_PASS=${DOCKER_PASS}"

                    sh '''
                        docker build -t $DOCKER_USER/sky-server:latest -f sky-server/Dockerfile .
                        docker tag sky-server:latest dennis9218/sky-server:first
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKER_USER/sky-server:latest
                    '''

                    echo "✅ Docker 镜像已成功推送！"
                }
            }
        }
    }
}
