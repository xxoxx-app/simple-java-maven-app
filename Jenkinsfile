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
        rtSetProps (
            serverId: "af",
            props: 'build_url='+env.BUILD_URL,
            spec: """{
            "files": [{
                "pattern": "target/demo-app-*.jar",
                "props": "filter-by-this-prop=yes"
            }]}"""
        )
        server.setProps spec: setPropsSpec, props: “p1=v1;p2=v2”
        rtMavenRun(
            tool: "maven_3.6.1",
            goals: 'clean package -Dmaven.test.skip=true -Dmaven.repo.local=.repository',
            deployerId: "MAVEN_DEPLOYER",
            resolverId: "MAVEN_RESOLVER"
        )
    }
    
}
