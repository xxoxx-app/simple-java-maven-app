node('master'){
    stage('Clean workspace'){
        //cleanWs()
    }
    stage('fetch code'){
        checkout(
            [
                $class: 'GitSCM', 
                branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, 
                userRemoteConfigs: [
                    [
                        credentialsId: 'e63d883e-2987-4556-b83d-5ed2670853d1', 
                        url: 'git@github.com:xxoxx-app/simple-java-maven-app.git'
                    ]
                ]
            ]
        )
    }
    stage('compile'){
        step{
        rtMavenResolver(
            id: "MAVEN_RESOLVER",
            serverId: "af",
            releaseRepo: "libs-release",
            snapshotRepo: "libs-snapshot"
        )
        rtMavenDeployer(
            id: "MAVEN_DEPLOYER",
            serverId: "af",
            releaseRepo: "nxl_test-release-local",
            snapshotRepo: "nxl_test-snapshot-local"
        )
        rtBuildInfo (
            captureEnv: true
        )
        }    
        rtMavenRun(
            tool: "maven_3.6.1",
            goals: 'clean package',
            opts: '-Dmaven.test.skip=true -Dmaven.repo.local=.repository',
            deployerId: "MAVEN_DEPLOYER",
            resolverId: "MAVEN_RESOLVER"
        )
        rtSetProps (
            serverId: "af",
            props: 'build__url='+env.BUILD_URL,     
            spec: """{
            "files": [{
                "pattern": "nxl_test-snapshot-local/**/*.jar",
                "props":"build.number=${BUILD_NUMBER}"
            }]}"""
        )
        rtPublishBuildInfo (
            serverId: "af"
        )
    }
}
