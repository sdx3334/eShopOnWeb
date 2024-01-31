pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'dotnet build eShopOnWeb.sln '
      }
    }

    stage('Test') {
      parallel {
        stage('Unit') {
          steps {
            sh 'dotnet test tests/UnitTests'
          }
        }

        stage('Integration') {
          steps {
            sh 'dotnet test tests/IntegrationTests'
          }
        }

        stage('Functional') {
          steps {
            warnError(message: 'Functional problem ')
            sh 'dotnet test Unit/FunctionalTests'
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        sh '    dotnet publish eShopOnWeb.sln -o /var/aspnet  -p:ErrorOnDuplicatePublishOutputFiles=false'
        dir(path: '/var/aspnet')
        archiveArtifacts 'OnlyIfSuccessful '
      }
    }

  }
}