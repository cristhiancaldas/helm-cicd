node {
    def app

  environment {
      GIT_CREDS = credentials('github-token')
  }

    stage('Clone repository') {
      
        checkout scm
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                      
                    sh """
                            wget https://github.com/mikefarah/yq/releases/download/v4.9.6/yq_linux_amd64.tar.gz
                            tar xvf yq_linux_amd64.tar.gz
                            mv yq_linux_amd64 /usr/bin/yq
                            git config --global user.name "ccargocd"
                            git config --global user.email "c.caldas.m@gmail.com"
                           
                            cat values.yml
                            yq eval '.containers.tag = ${DOCKERTAG}' -i values.yaml
                            cat values.yml
                            git add .
                            git commit -m 'By Jenkins Job changemanifest: ${DOCKERTAG}'
                            git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/cristhiancaldas/helm-cicd.git HEAD:main
                       """
      }
    }
  }
}

}
