node {
    //This is a groovy scripted language
    def app
    
    env.IMAGE = 'pumejlab/amazon'

    stage('Clone repository') {
             git branch: 'pumejbranch', url: 'https://github.com/Mexxy-lab/argocd-amazon-manifest.git'
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'pumej-git_token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //script {def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')}
                        //script  {def IMAGE='pumejlab/amazon'}
                        sh "git config user.email pumej1985@gmail.com"
                        sh "git config user.name Mexxy-lab"
                        //sh "git switch master"
                        sh "cat deployment.yml"
                        //This command used to substitute the image name to new one that was pushed. From latest to Jenkins build number.
                        sh "sed -i 's+${IMAGE}.*+${IMAGE}:${DOCKERTAG}+g' deployment.yml"
                        sh "cat deployment.yml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/argocd-amazon-manifest.git HEAD:pumejbranch"
             }
         }
     }
  }
}