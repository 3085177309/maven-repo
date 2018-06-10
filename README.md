
### 1、build.gradle

```groovy
tasks.withType(JavaCompile) {
    options.encoding = 'UTF-8'
}

apply from: "${rootProject.projectDir}/build-maven.gradle"
```

### 2、build-maven.gradle
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
		//每隔24小时检查远程依赖是否存在更�?
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
			//若是war包，就写components.web,若是jar包，就写components.java
			from components.java
		}
	}
	repositories {
		maven {
			url = maven_workerdir
		}
	}
}
```
