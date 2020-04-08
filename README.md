# gwt-ace

Generated JsInterop types for J2CL projects to use when interacting with the Ace Editor library. This project
uses the same mechanism as [elemental2](https://github.com/google/elemental2/) to generate its output: 
[jsinterop-generator](https://github.com/google/jsinterop-generator/). 

### Building
To build this project for maven, make sure you have the correct version of [Bazel](https://bazel.build) (see the 
`.bazelversion` file for the expected version), and invoke `build.sh`, then run your desired maven goal in the
`maven/` directory - probably `mvn clean install`.

## Including this in your project

### Bazel
As with elemental2, you can add this project as a `http_archive`, and depend on a target to use this in your
J2CL project. However, this project modifies how [closure-compiler](https://github.com/google/closure-compiler/)
is built (in a way that should remain compatible with elemental2), so please use this project's modified 
`load_elemental2_repo_deps` def instead of the one in elemental2. Roughly:

```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "com_google_elemental2",
    strip_prefix = "elemental2-master",
    url = "https://github.com/google/elemental2/archive/master.zip",
)
http_archive(
    name = "com_vertispan_gwt_ace_editor",
    strip_prefix = "gwt-ace-master",
    url = "https://github.com/Vertispan/gwt-ace/archive/master.zip",
)

load("@com_vertispan_gwt_ace_editor//build_defs:repository.bzl", "load_elemental2_repo_deps")
load_elemental2_repo_deps()

load("@com_google_elemental2//build_defs:workspace.bzl", "setup_elemental2_workspace")
setup_elemental2_workspace()
```

Then, you can depend on target `@com_vertispan_gwt_ace_editor//:ace-editor`.

### Maven
The version string is based on the version of Ace Editor that this jar is built to be compatible with - 1.2.3 matches
the version of the externs available in closure-compiler, the `-1` suffix exists to allow the generated Java to be
updated and a new release made. The `-SNAPSHOT` suffix then indicates that this is not yet ready for production as we 
incorporate feedback from users, tweak things in how the jsinterop generator is used, etc.

Snapshots are presently available in either the Sonatype or Vertispan snapshot repository.
```xml
<dependency>
    <groupId>com.vertispan.gwt.jsinterop.wrapper</groupId>
    <artifactId>gwt-ace-editor</artifactId>
    <version>1.2.3-1-SNAPSHOT</version>
</dependency>
```