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
        def server = Artifactory.server 'af'
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
        rtMavenRun(
            tool: "maven_3.6.1",
            goals: 'clean package',
            opts: '-Dmaven.test.skip=true -Dmaven.repo.local=.repository',
            deployerId: "MAVEN_DEPLOYER",
            resolverId: "MAVEN_RESOLVER"
        )
        def buildInfo = Artifactory.newBuildInfo()
        rtBuildInfo (
            captureEnv: true
        )
        rtPublishBuildInfo (
            serverId: "af"
        )
        server.publishBuildInfo buildInfo
        rtSetProps (
            serverId: "af",
            props: 'p1=v1;p2=v2',     
            spec: """{
            "files": [{
                "pattern": "nxl_test-snapshot-local",
                "props": "filter-by-this-prop=yes"
            }]}"""
        )
    }
    
}
