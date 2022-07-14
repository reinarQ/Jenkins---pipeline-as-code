pipeline {
    agent any

    stages {
        stage('Preparing gradlew') {
            steps {
                sh 'chmod +x gradlew'
            }
        }
        stage('test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('build') {
            steps {
                sh './gradlew build'
            }
        }
        stage('Release') {
            steps {
                sh 'token="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e"'
                sh 'tag=$(git describe --tags)'
                sh 'message="$(git for-each-ref refs/tags/$tag --format=\'%(contents)\')"'
                sh 'name=$(echo "$message" | head -n1)'
                sh 'description=$(echo "$message" | tail -n +3)'
                sh 'release=$(curl -XPOST -H "Authorization:token $token" --data \'{"tag_name": "$tag", "target_commitish": "main", "name": "$name", "body": "$description", "draft": false, "prerelease": false}\' "https://api.github.com/repos/reinarQ/Jenkins---pipeline-as-code/releases"'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}