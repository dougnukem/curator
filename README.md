# Curator Maven Repository

This branch is used to act as a snapshot maven repository, to make it easier to use forks of this project by including the github page as a maven repository.

For example in an sbt build to reference a 1.2.6-SNAPSHOT version of curator you'd do the following in the build.sbt:

```scala
resolvers += "dougnukem-curator-mvn-repo" at "http://github.com/dougnukem/curator/raw/maven/releases"

resolvers += "dougnukem-curator-mvn-repo-snapshots" at "http://github.com/dougnukem/curator/raw/maven/snapshots"

libraryDependencies ++= Seq(
	"com.netflix.curator" % "curator-recipes" % "1.2.6-SNAPSHOT",
	"com.netflix.curator" % "curator-test" % "1.2.6-SNAPSHOT",
    "com.netflix.curator" % "curator-x-discovery" % "1.2.6-SNAPSHOT",
    "com.netflix.curator" % "curator-x-discovery-server" % "1.2.6-SNAPSHOT"
)

```