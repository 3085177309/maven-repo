
#�ֿ�����˵��

###1��build.gradle
```groovy
tasks.withType(JavaCompile) {
    options.encoding = 'UTF-8'
}

apply from: "${rootProject.projectDir}/build-maven.gradle"
```

###2��build-maven.gradle
```groovy
def  maven_workerdir = 'f:\\maven-repo'


apply plugin: 'maven-publish'

allprojects{

	repositories {

		mavenLocal()
		mavenCentral()
		maven {
			url "https://raw.githubusercontent.com/3085177309/maven-repo/master"
		}
	}


	configurations.all {
		//ÿ��24Сʱ���Զ�������Ƿ���ڸ���
		//resolutionStrategy.cacheChangingModulesFor 24, 'hours'
		resolutionStrategy.cacheChangingModulesFor 0, 'seconds'
	}

}


publishing {
	publications {
		maven(MavenPublication) {
			groupId project.group
			artifactId project.name
			version project.version
			//����war������дcomponents.web,����jar������дcomponents.java
			from components.java
		}
	}
	repositories {
		maven {
			url = maven_workerdir
		}
	}
}


task gitCloneInit(type:Exec){

	commandLine 'cmd', '/c', ' IF NOT EXIST ' + maven_workerdir + ' mkdir ' + maven_workerdir
	//println getCommandLine()

}

gitCloneInit.doFirst {
	workingDir maven_workerdir
	commandLine   'cmd', '/c', ' IF NOT EXIST ' + maven_workerdir+'\\.git ' + 'git clone https://github.com/3085177309/maven-repo.git . '
	println getCommandLine()
	exec()
}

gitCloneInit.doLast{
	workingDir maven_workerdir
	commandLine   'cmd', '/c', ' IF  EXIST ' + maven_workerdir+'\\.git ' + 'git config user.name SunDeny '
	println getCommandLine()
	exec()
	commandLine   'cmd', '/c', ' IF  EXIST ' + maven_workerdir+'\\.git ' + 'git config user.email 3085177309@qq.com  '
	println getCommandLine()
	exec()
}


task gitPushToGithubMaven(type:Exec){

	dependsOn(gitCloneInit)

	workingDir maven_workerdir
	commandLine 'cmd', '/c', 'IF EXIST  gitPush.bat  call  gitPush.bat'
	//println getCommandLine()

}
```
