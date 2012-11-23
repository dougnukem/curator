# Curator Maven Repository

This branch is used to act as a snapshot maven repository, to make it easier to use forks of this project by including the github page as a maven repository.

For example I forked the https://github.com/Netflix/curator to https://github.com/dougnukem/curator, I made changes to the project committed them. 

I created a maven branch (if it didn't exist already).

```
# Create new clone of repo to make it decoupled from the normal development
git clone git@github.com:dougnukem/curator.git curator-maven
cd curator-maven
# Create orphaned branch because it is not related to the project directly and is used as a maven repository
git checkout --orphan -b maven
mkdir releases snapshots

```

Then in my normal checkout of curator I modified the gradle.properties to namespace my forked copy of curator.

```
version=1.2.5.dougnukem-SNAPSHOT

```

Then to publish it I run:

```
# Assuming a common directory for git projects
cd curator
gradle uploadMavenLocal

cp -r repo/ ../curator-maven/snapshots
cd ../curator-maven/
git add -A 
git commit -m "publishing new maven artifacts 1.2.5.dougnukem-SNAPSHOT"
git push -u origin maven

```

## TODO
- The process could probably be made simpler but more tightly-coupled if the /repo directory that gradle builds to was a symlink to the maven branch (is this possible in git, like a svn:external?).
- Gradle build could integrate the uploadMavenLocal into a release process so that version numbers are incremented etc (currently I didn't want to impact the normal release process)


## Using Forked Maven Artifacts

Then to reference in an sbt build (or any maven compatible build) to reference a 1.2.5.dougnukem-SNAPSHOT version of curator you'd do the following in the build.sbt:

```scala
resolvers += "dougnukem-curator-mvn-repo" at "http://github.com/dougnukem/curator/raw/maven/releases"

resolvers += "dougnukem-curator-mvn-repo-snapshots" at "http://github.com/dougnukem/curator/raw/maven/snapshots"

libraryDependencies ++= Seq(
	"com.netflix.curator" % "curator-recipes" % "1.2.5.dougnukem-SNAPSHOT",
	"com.netflix.curator" % "curator-test" % "1.2.5.dougnukem-SNAPSHOT",
    "com.netflix.curator" % "curator-x-discovery" % "1.2.5.dougnukem-SNAPSHOT",
    "com.netflix.curator" % "curator-x-discovery-server" % "1.2.5.dougnukem-SNAPSHOT"
)

```