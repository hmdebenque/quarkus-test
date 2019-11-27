# quarkus-test
Test project for quarkus, because this is a very cool tool.

# Release process

It is released using the [unleash maven plugin](https://github.com/shillner/unleash-maven-plugin) that allows a lot of customization and safe rollback on error.

To release, one should execute:
```bash
mvn --batch-mode unleash:perform
```
This will execute the default release workflow with default release version: patch version incrementation.

Once release is successful, we should get:
1. a commit in current branch to update version to next development version. For instance `1.2.3-SNAPSHOT` to `1.2.4-SNAPSHOT`
1. a commit outside any branch tagged `1.2.3` with the original HEAD commit (last `1.2.3-SNAPSHOT`) as parent
1. a new release appearing on github: [releases](https://github.com/hmdebenque/quarkus-test/releases)
1. the released artifact on the package manager: [packages](https://github.com/hmdebenque/quarkus-test/packages)

# Release setup

To make it work I had to fill the `<scm>` and `<distributionManagement>` parts in the `pom.xml`. I also had to configure the server accordingly in my `settings.xml` to give it the correct access rights.

:warning: using a mirror might prevent unleash to upload the artifact on the repository

The customizable [release workflow](https://github.com/shillner/unleash-maven-plugin/wiki/unleash%3Aperform#default-workflow):
```
storeScmRevision
checkProjectVersions
checkParentVersions
checkDependencies
checkPlugins
checkPluginDependencies
prepareVersions
checkAether
setReleaseVersions
addSpyPlugin
buildReleaseArtifacts
removeSpyPlugin
checkForScmChanges
tagScm
detectReleaseArtifacts
setDevVersion
serializeMetadata
installArtifacts
deployArtifacts
```
