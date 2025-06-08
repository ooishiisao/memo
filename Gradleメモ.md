# Gradleメモ

## ディレクトリ構造
- ルートディレクトリ
    - gradleディレクトリ
    - gradlew
    - gradlew.bat
    - settings.gradle
    サブプロジェクトの定義
        - サブディレクトリ
            - srcディレクトリ
                - mainディレクトリ
                    - javaディレクトリ
                    - resourceディレクトリ
                - testディレクトリ
                    - javaディレクトリ
                    - resourceディレクトリ
            - binディレクトリ
            - buildディレクトリ
            - build.gradle
        - サブディレクトリ
            - build.gradle


## build.gradle構造
### plugins
プラグインの定義
```
plugins {
    id 'application'
}
```
​
### repositories
ダウンロード先
```
repositories {
    mavenCentral()
}
```

### dependencies
ライブラリの依存関係
```
dependencies {
    testImplementation libs.junit.jupiter
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
    implementation libs.guava
}
```

​
プラグインの設定
```
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

application {
    // Define the main class for the application.
    mainClass = 'gp.App'
}
```

```
tasks.named('test') {
    // Use JUnit Platform for unit tests.
    useJUnitPlatform()
}
```

​
