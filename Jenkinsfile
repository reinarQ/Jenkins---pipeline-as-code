pipeline {
    agent any

    triggers {
        githubPush()
    }
    stages {
        stage('Preparing gradlew') {
            steps {
                sh 'chmod +x gradlew'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }
        stage('Release') {
            steps {
                sh 'token="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e"'
                sh 'tag=$(git describe --tags)'
                sh 'message="$(git for-each-ref refs/tags/$tag --format='%(contents)')"'
                sh 'name=$(echo "$message" | head -n1)'
                sh 'description=$(echo "$message" | tail -n +3)'
                sh 'description=$(echo "$description" | sed -z s/\n/\\n/g)'
                sh 'release=$(curl -XPOST -H "Authorization:token $token" --data \'{"tag_name": "$tag", "target_commitish": "main", "name": "$name", "body": "$description", "draft": false, "prerelease": true}\' "https://api.github.com/repos/reinarQ/Jenkins---pipeline-as-code/releases/$tag")'
                sh 'id=$(echo "$release" | sed -n -e 's/"id":\ \([0-9]\+\),/\1/p' | head -n 1 | sed 's/[[:blank:]])'
                sh 'https://api.github.com/repos/reinarQ/Jenkins---pipeline-as-code/releases/$id/assets?name=artifact.zip'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}