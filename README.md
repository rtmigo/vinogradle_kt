# [vinogradle](https://github.com/rtmigo/vinogradle_kt)

Personal reusable Gradle build script for Kotlin/JVM. Included as Git Subtree
in my other projects.


### Add "buildSrc" as Git Subtree

```
cd /path/to/my_project_kt

git subtree add --prefix buildSrc https://github.com/rtmigo/vinogradle_kt dev --squash
```

### Edit project dependencies

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

### Update "buildSrc" from Git Subtree

```
cd /path/to/my_project_kt

git add .
git commit -m "Before pulling vinogradle" --allow-empty
git subtree pull --prefix buildSrc https://github.com/rtmigo/vinogradle_kt dev --squash -m "Pulling vinogradle from GitHub"
```
