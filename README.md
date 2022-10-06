# [vinogradle](https://github.com/rtmigo/vinogradle_kt)

Personal reusable Gradle build script for Kotlin/JVM. Included as Git Subtree
in my other projects.


## Add "buildSrc" as Git Subtree

```
cd /path/to/my_project_kt

git subtree add --prefix buildSrc https://github.com/rtmigo/vinogradle_kt dev --squash
```

## Edit project dependencies

```kotlin
plugins {
    // versions should NOT be specified
    kotlin("jvm")

    id("org.jetbrains.dokka")  // creates JavaDocs
    id("signing")  // adds GPG signatures needed by Maven Central
    id("io.codearte.nexus-staging")  // for "closeAndReleaseRepository"
    id("maven-publish")  // for "publish"
}
```

## Update "buildSrc" from Git Subtree

```
cd /path/to/my_project_kt

git add .
git commit -m "Before pulling vinogradle" --allow-empty
git subtree pull --prefix buildSrc https://github.com/rtmigo/vinogradle_kt dev --squash -m "Pulling vinogradle from GitHub"
```

## GitHub Actions

```yaml
  to-github-package:
    # needs: [ to-master ]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup JDK 8
        uses: actions/setup-java@v2
        with:
          java-version: '8'
          distribution: 'adopt'
          cache: gradle
      - name: Publish as Maven to GitHub
        run:
          ./gradlew publish
        env:
          # this token is predefined
          GITHUB_PKGPUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  to-maven-central-package:
    # needs: [ to-master ]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup JDK 8
        uses: actions/setup-java@v2
        with:
          java-version: '8'
          distribution: 'adopt'
          cache: gradle
      - name: Publish as Maven to Sonatype
        run:
          ./gradlew publish closeAndReleaseRepository
        env:
          MAVEN_GPG_KEY: ${{ secrets.MAVEN_GPG_KEY }}
          MAVEN_GPG_PASSWORD: ${{ secrets.MAVEN_GPG_PASSWORD }}
          SONATYPE_USERNAME: ${{ secrets.SONATYPE_USERNAME }}
          SONATYPE_PASSWORD: ${{ secrets.SONATYPE_PASSWORD }}
```