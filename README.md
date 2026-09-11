# springboot-dependencies

给多个 `io.github.tcq1007` 项目共用的 Maven BOM。版本号只在本仓库的 `gradle.properties` 里改。

坐标：`io.github.tcq1007:springboot-dependencies:<springbootDependenciesVersion>`

会再导出 Spring Boot / Spring Cloud / Spring Cloud Alibaba / QueryDSL 的上游 BOM，并锁定一批第三方依赖版本。

Gradle 插件版本（如 Spring Boot 插件）不由本 BOM 管理，消费方在自己的 `gradle.properties` 里声明，并与本仓库的 `springBootVersion` 对齐。

## 本地发布

```bash
./gradlew publishToMavenLocal
```

## 发布到 GitHub Packages

```bash
./gradlew publish
```

需要 `GITHUB_ACTOR` / `GITHUB_TOKEN`（或 `gpr.user` / `gpr.token`）。

## 消费方

```properties
springbootDependenciesVersion=boot-4.1.1-1
springBootVersion=4.1.1
springDependencyManagementVersion=1.1.7
```

```gradle
repositories {
    mavenLocal()
    maven {
        url = uri('https://maven.pkg.github.com/TCQ1007/springboot-dependencies')
        credentials {
            username = project.findProperty('gpr.user') ?: System.getenv('GITHUB_ACTOR')
            password = project.findProperty('gpr.token') ?: System.getenv('GITHUB_TOKEN')
        }
    }
    mavenCentral()
}

dependencyManagement {
    imports {
        mavenBom "io.github.tcq1007:springboot-dependencies:${springbootDependenciesVersion}"
    }
}
```

依赖写无版本坐标即可。不要对本 BOM 使用 `includeBuild`。

## 升级

1. 改本仓库 `gradle.properties`
2. 调高 `springbootDependenciesVersion`
3. `./gradlew publishToMavenLocal` 或 `./gradlew publish`
4. 消费方改 `springbootDependenciesVersion`，并按需同步插件相关版本
