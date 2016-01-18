<img src="https://raw.githubusercontent.com/Urucas/gravy/master/icon.png" />

# gravy
A simple gradle task to upload .apk to sauce-labs storage

#Usage
Just add the next code to your main app ```build.gradle``` file

```gradle
apply plugin: 'maven'
ext {
    endpoint = 'http://saucelabs.com/rest/v1/storage'
    user = 'user'
    accessKey = 'accessKey'
}
task gravy(dependsOn: assembleRelease) << {
    def apk_path = "$project.buildDir/outputs/apk/$project.name-release-unsigned.apk"
    def u = "$user:$accessKey"
    def url = "$endpoint/$user/$project.name"
    exec {
        executable "/bin/sh"
        args "-c", "curl -u $u -X POST -H \"Content-Type: application/octet-stream\" $url?overwrite=true --data-binary @$apk_path"
    }
}
```
Run the task from Android Studio Gradle tool window

<img src="https://raw.githubusercontent.com/Urucas/gravy/master/screen.png" />

or via cli
```bash
./gradlew -q gravy
```
