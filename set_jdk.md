### Run JDK 1.7 and 1.8 on OSX

To run multiple JDK installations on OSX, add this to your `~/.bash_profile` file:

```
function setjdk() {
  if [ $# -ne 0 ]; then
   removeFromPath '/System/Library/Frameworks/JavaVM.framework/Home/bin'
   if [ -n "${JAVA_HOME+x}" ]; then
    removeFromPath $JAVA_HOME/bin
   fi
   export JAVA_HOME=`/usr/libexec/java_home -v $@`
   export PATH=$JAVA_HOME/bin:$PATH
  fi
 }
 function removeFromPath() {
  export PATH=$(echo $PATH | sed -E -e "s;:$1;;" -e "s;$1:?;;")
 }
setjdk 1.7
```

This will set your default install to 1.7:

```
dalvizu:~$ java -version
java version "1.7.0_60"
Java(TM) SE Runtime Environment (build 1.7.0_60-b19)
Java HotSpot(TM) 64-Bit Server VM (build 24.60-b09, mixed mode)
```

To set it to 1.8 for a window, run `setjdk 1.8`:

```
dalvizu:~$ java -version
java version "1.8.0_45"
Java(TM) SE Runtime Environment (build 1.8.0_45-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.45-b02, mixed mode)
```

To set it to 1.8 by default, change the line in `bash_profile` from `setjdk 1.7`
 to `setjdk 1.8`

### About:

Java installs by default to `/System/Library/Frameworks/JavaVM.framework/` and adds the binary directory there to the system PATH. This works for one JVM installs, but not several.

There's also a utility at `/usr/libexec/java_home` to tell you JAVA_HOME for all installed JDKs.

So - the way this works is to remove any set JAVA_HOME from your path
