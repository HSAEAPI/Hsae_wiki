
# 1	Introduction

### &nbsp;&nbsp; 1.1 Purpose
This document describes the elements of the HSAE API, such as VehicleManager classes, VehicleManager, and HsaeAudioManager, that a developer is likely to use or extend. While the goal of HSAE API is to provide a complete solution for test Android applications and provide API for android developer.

### &nbsp;&nbsp; 1.2 how to use this jar for android source build apk

**if development by android source, you need add this jar like this.**

  if you use Android.mk to make your apk ,you should add config like this.
```
LOCAL_JAVA_LIBRARIES := \
		hsae.framework
```

### &nbsp;&nbsp; 1.3 how to use this jar for android studio build apk.
**if development by android studio, you need add this jar like this.**

**First :**  copy hsae.framework.jar to app\libs\ path.

**Second :** add "compileOnly files('libs/hsae.framework.jar')" in your build.gradle(module:app),path dependencies.

***Example:***

```
dependencies {
//implementation fileTree(include: ['*.jar'], dir: 'libs')
implementation 'com.android.support:appcompat-v7:28.0.0'
implementation 'com.android.support.constraint:constraint-layout:1.1.3'
implementation 'com.android.support:design:28.0.0'
testImplementation'junit:junit:4.12'
androidTestImplementation'com.android.support.test:runner:1.0.2'
androidTestImplementation'com.android.support.test.espresso:espresso-core:3.0.2'
compileOnly files('libs/hsae.framework.jar')
}
```







