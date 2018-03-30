pipeline {
  agent any
  environment {
    projectName = 'sample-shiny-analysis-tool'
    remotePath = 'shiny-server/shinyapps'
  }
  stages {
    stage('Build') {
      steps {
        archiveArtifacts(artifacts: '**/*', defaultExcludes: true, excludes: 'Jenkinsfile')
      }
    }
    stage('Development') {
      steps {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'shiny@gcp', transfers: [sshTransfer(excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: true, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: "${remotePath}/${projectName}", remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*'), sshTransfer(excludes: '', execCommand: """
        docker exec --user shiny shiny-server /bin/bash  \
        -c "cd /srv/shiny-server/$projectName &&  \
        pwd &&  \
        Rscript -e 'packrat::restore()' && \
        touch restart.txt"
""", execTimeout: 0, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])


      }
    }
  }
  
}
