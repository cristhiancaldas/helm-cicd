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
                            sudo wget -qO /usr/local/bin/yq https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64
                            sudo chmod a+x /usr/local/bin/yq
                            yq --version
                            git config --global user.name "ccargocd"
                            git config --global user.email "c.caldas.m@gmail.com"
                           
                            cat values.yaml
                            yq eval '.containers.tag = ${DOCKERTAG}' -i values.yaml
                            cat values.yaml
                            git add .
                            git commit -m 'By Jenkins Job changemanifest: ${DOCKERTAG}'
                            git push https://${GIT_PASSWORD}@github.com/cristhiancaldas/helm-cicd.git HEAD:main
                       """
      }
    }
  }
}

}
