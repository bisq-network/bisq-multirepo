# Bisq

## Clone this and other Bisq repositories

This repository contains a [`.mrconfig`](.mrconfig) file that makes it simple to clone all `bisq-network` repositories. Here's how to use it:

    brew install mr                                 # or equivalent
    git clone https://github.com/bisq-network/bisq  # clone this repositories
    cd bisq
    mr checkout                                     # clone all bisq-* repositories

See the [myrepos](https://myrepos.branchable.com/) homepage for more details on `mr`.

If you do not wish to use `mr`, then run `grep checkout .mrconfig | cut -d'=' -f2` and copy and paste the `git clone` commands for the repositories you want to work with.

## Build and test everything

Once you've checkout out the repositories you want to work with, run a full Gradle build from the root directory

    ./gradlew build

## Import into IDEA

This repository also contains a [`settings.gradle`](settings.gradle) file that defines a [composite build](https://docs.gradle.org/current/userguide/composite_builds.html) consisting of all `bisq-*` that have been checked out above. This makes it easy to import all `bisq-*` components into a single IDEA project as follows:

 1. Go to `Import Project`, select `settings.gradle` and click `Open`
 1. In the `Import Project from Gradle` screen, check the `Use auto-import` option and click `OK`
 1. When prompted whether to overwrite the existing `.idea` directory, click `Yes`
 1. Go to `Preferences->Build, Execution, Deployment->Compiler->Annotation Processors` and check the `Enable annotation processing` option (to enable processing of Lombok annotations)
 1. In the `Project` tool window, right click on the root-level `.idea` folder, select `Git->Revert...` and click OK in the dialog that appears (to restore source-controlled `.idea` configuration files that get overwritten during project import)
 1. Go to `Help->Edit Custom Properties...`, add a line to the file that reads `idea.max.intellisense.filesize=12500`, then quit and restart IDEA (to handle Bisq's very large generated `PB.java` Protobuf source file)
 1. Go to `Build->Build project`. Everything should build cleanly. You should be able to run tests, run `main` methods in any component, etc.

> TIP: If you encounter compilation errors related to the `io.bisq.generated.protobuffer.PB` class, it is probably because you didn't run the full Gradle build above. You need to run the `generateProto` task in the `common` project. You can do this via the Gradle tool window in IDEA, or you can do it the command line with `cd common; ./gradlew generateProto`. Once you've done that, run `Build->Build project` again and you should have no errors.
