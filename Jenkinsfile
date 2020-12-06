node {
    ansiColor('xterm') {
        withCredentials([string(credentialsId: 'vaultkey', variable: 'vault_pass')]) {
            withEnv(['ANSIBLE_FORCE_COLOR=true', 'GIT_SSL_NO_VERIFY=1']) {
                stage ('Git'){
                    git_project = 'ssh://git@co181:7999/an/node_exporter.git'
					git_branch = 'master'//"$BRANCH"
					cred_id = "gitkey"
					checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: "*/${git_branch}"]], doGenerateSubmoduleConfigurations: false,
					extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: "${env.WORKSPACE}"], [$class: 'SubmoduleOption', disableSubmodules: false,
					parentCredentials: true, recursiveSubmodules: true, reference: '', timeout: 1, trackingSubmodules: false]], submoduleCfg: [],
					userRemoteConfigs: [[credentialsId: cred_id, url: git_project]]]
                }
                stage ('Deploy'){
                    ansiblePlaybook(
                        installation: 'ansible',
                        playbook: 'node_exporter.yaml',
                        inventory: 'inventory',
                        credentialsId: 'gitkey',
                        vaultCredentialsId: 'vaultkey',
                        colorized: true)
                }
                stage ('Clean'){
                    cleanWs()
                }
            }
        }
    }
}
