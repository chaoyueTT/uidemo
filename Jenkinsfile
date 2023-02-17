pipeline{
    agent any
    environment {
      // 镜像名称
      IMAGE_NAME = "ruoyi-ui"
      // 工作目录
      WS = "${WORKSPACE}"
      // 后台项目镜像名称
      API_IMAGE_NAME = "ruoyi-admin"
      // 自定义的构建参数
      PROFILE = "prod"
    }
 
    //定义流水线的加工流程
    stages {
        //流水线的所有阶段
        stage('1.环境检查'){
            steps {
               sh 'pwd && ls -alh'
               sh 'printenv'
               sh 'docker version'
               sh 'git --version'
            }
        }
 
        stage('2.编译'){
            agent {
                docker {
                    image 'node:12-alpine'
                 }
            }
            steps {
               sh 'pwd && ls -alh'
               sh 'node -v'
               sh 'cd "/var/lib/jenkins/workspace/uidemo_main@2" && npm install --registry=https://registry.npmmirror.com --no-fund && npm run build:prod'
            }
        }
 
        stage('3.打包'){
            steps {
               sh 'pwd && ls -alh'
               sh 'docker build --build-arg PROFILE=${PROFILE} -t ${IMAGE_NAME} .'
            }
        }
 
        stage('4.部署'){
            // 删除容器和虚悬镜像
            steps {
               sh 'pwd && ls -alh'
               sh 'docker rm -f ${IMAGE_NAME} || true && docker rmi $(docker images -q -f dangling=true) || true'
               sh 'docker run -d -p 8889:80 --name ${IMAGE_NAME} --link ruoyi-admin:ruoyi-admin ${IMAGE_NAME}'
            }
        }
    }
}