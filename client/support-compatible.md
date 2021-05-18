# 兼容仍在使用support库的过时应用

CUGE Android SDK的主线版本基于AndroidX + Kotlin Coroutine Library开发，这导致其与传统的基于support架构的应用程序存在冲突。考虑到市面上仍然有相当数量的应用采用support架构，因此CUGE Android SDK新拉出了一条support兼容分支，将主线的功能backport到了support架构上。

如果您希望在仍使用support架构的应用上集成CUGE Android SDK，您应采用以下写法来引入依赖:

```groovy
// CUGE
implementation('com.zzm.android_basic_library:cuge_android_sdk:1.0.16-support-SNAPSHOT') {
   exclude group: 'androidx.core', module: 'core'
   exclude group: 'androidx.appcompat', module: 'appcompat'
   exclude group: 'androidx.test.ext', module: 'junit'
   exclude group: 'androidx.test.espresso', module: 'espresso-core'
}
```

> 最新版本请以 [Maven仓库](http://159.75.8.205:8090/#browse/search=keyword%3Dcuge_android_sdk) 为准，注意版本号应带有support字样。
>
> 如果您的项目中使用了Kotlin标准库，或者是Kotlin协程，请确保它们的版本不高于如下所示:
>
> ```groovy
> implementation "org.jetbrains.kotlin:kotlin-stdlib:1.3.72"
> implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.7"
> implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.7"
> ```
>
> 同时，也请确保`kotlin-gradle-plugin`的版本不高于:
>
> ```groovy
> classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.3.72"
> ```
>
> 否则，您可能需要手动排掉SDK中的Kotlin依赖，且承担SDK可能存在的不稳定风险。
