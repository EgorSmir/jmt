def setGitHubCommitStatus(context, state, message, commitSha = null) {
    def params = [
        $class: 'GitHubCommitStatusSetter',
        contextSource: [$class: 'ManuallyEnteredCommitContextSource', context: context],
        statusResultSource: [$class: 'ConditionalStatusResultSource', results: [
            [$class: 'AnyBuildResult', state: state, message: message]
        ]]
    ]

    if (commitSha) {
        params['commitShaSource'] = [$class: 'ManuallyEnteredShaSource', sha: commitSha]
    }

    step(params)
}


node {
    try {
        stage('Cleanup') {
            // Clean the workspace
            deleteDir()
        }

        stage('Checkout') {
            checkout scm
        }

        def commitSha = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()

        // Set initial commit status
        setGitHubCommitStatus('Unique-Context-Job2', 'PENDING', 'Build started', commitSha)

        stage('Build') {
            // Your build steps
            sh 'echo Building...'
        }

        // Set success status
        setGitHubCommitStatus('Unique-Context-Job2', 'SUCCESS', 'Build succeeded', commitSha)

    } catch (err) {
        // Set failure status
        setGitHubCommitStatus('Unique-Context-Job2', 'FAILURE', 'Build failed', commitSha)
        throw err
    }
}
