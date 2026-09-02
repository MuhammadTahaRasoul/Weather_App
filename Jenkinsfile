pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    environment {
        DEPLOY_PATH = 'C:\\Deploy\\Weather_App'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Publish') {
            steps {
                bat 'dotnet restore .\\Weather_App.Server\\Weather_App.Server.csproj'
                bat 'dotnet publish .\\Weather_App.Server\\Weather_App.Server.csproj -c Release -o publish_output'
            }
        }

        stage('Stop Site') 
        {
            steps {
                bat 'C:\\Windows\\System32\\inetsrv\\appcmd stop apppool /apppool.name:"Weather_App"'
                bat 'C:\\Windows\\System32\\inetsrv\\appcmd stop site /site.name:"Weather_App"'
    }
}

        stage('Deploy') {
            steps {
                bat "if exist \"${DEPLOY_PATH}\" rmdir /s /q \"${DEPLOY_PATH}\""
                bat "xcopy publish_output \"${DEPLOY_PATH}\" /E /I /Y"
            }
        }

        stage('Start Site') 
        {
            steps {
                bat 'C:\\Windows\\System32\\inetsrv\\appcmd start apppool /apppool.name:"Weather_App"'
                bat 'C:\\Windows\\System32\\inetsrv\\appcmd start site /site.name:"Weather_App"'
    }
}

    post {
        success { echo 'Deployed successfully.' }
        failure { echo 'Build or deploy failed — check console output.' }
    }
}
