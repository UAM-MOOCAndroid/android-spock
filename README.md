# Spock for Android

[![BuddyBuild](https://dashboard.buddybuild.com/api/statusImage?appID=583c48ac3f7e8e0100bc315b&branch=master&build=latest)](https://dashboard.buddybuild.com/apps/583c48ac3f7e8e0100bc315b/build/latest)
[![Build Status](https://travis-ci.org/pieces029/android-spock.svg?branch=master)](https://travis-ci.org/pieces029/android-spock)


This library allows for the [Spock Framework](//spockframework.org) mocks to be used on Android. As
well as add some helpful features to make Android testing easier.

## Usage

### Setup Groovy For Android

```groovy
buildscript {
  repositories {
    jcenter()
  }

  dependencies {
     classpath 'com.android.tools.build:gradle:2.2.2'
     classpath 'org.codehaus.groovy:groovy-android-gradle-plugin:1.1.0'
  }
}

apply plugin: 'com.android.application'
apply plugin: 'groovyx.android'
```

See [groovy-android-gradle-plugin](//github.com/groovy/groovy-android-gradle-plugin) for more
details.

### Setup Dependencies

```groovy
dependencies {
  ...
  androidTestCompile 'org.codehaus.groovy:groovy:2.4.7:grooid'
  androidTestCompile "com.andrewreitz:spock-android:${androidSpockVersion}"
  ...
}
```

### Setup Android Plugin

```groovy
android {
  ...

  defaultConfig {
    ...
    testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
  }

  packagingOptions {
    exclude 'META-INF/services/org.codehaus.groovy.transform.ASTTransformation'
    exclude 'LICENSE.txt'
  }
}
```

### Write your tests

Tests must be placed in the `./src/androidTest/groovy` directory.

Write your tests like you would regular spock tests. See the spock-android-sample project and
[Spock Framework](//spockframework.org) for more details.

### Mocking

Objenesis and cglib do not work with Android. But that's okay. Using dexmaker we can still create
mock objects in spock fashion. The only difference is instead of your test classes inheriting from
`Specification`, you need to inherit from `AndroidSpecification`.

Note: You can not use mocked automatic getters and setters. Example `mocked.getString()` will work
where as `mocked.string` will not. This is due to limitations of Android not containing certain core
java classes.

### Annotations

`UseActivity` is an field annotation to get access to your Activity during tests. You can even
provide bundle arguments by supplying a BundleCreator.

Ex.
```groovy
@UseActivity(MyActivity) def myActivity
```

`UseApplication` is a field annotation that supplies your Application.

Ex.
```groovy
@UseApplication(MyApplication) def myApplication
```

`WithContext` is a field annotation that supplies you with a context. This is not an implementation of
your application.

Ex.
```groovy
@WithContext def context
```

All field annotations will be set during the setup fixture.

## License

    Copyright 2016 Andrew Reitz

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
