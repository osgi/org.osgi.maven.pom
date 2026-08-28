# org.osgi.maven.pom

[![build](https://github.com/osgi/org.osgi.maven.pom/actions/workflows/build.yml/badge.svg)](https://github.com/osgi/org.osgi.maven.pom/actions/workflows/build.yml)

Maven parent POM for all repositories of the
[OSGi Specification Project](https://projects.eclipse.org/projects/technology.osgi).

Every `org.osgi.*` spec repo references it as parent:

```xml
<parent>
	<groupId>org.osgi</groupId>
	<artifactId>org.osgi.maven.pom.parent</artifactId>
	<version>0.0.1-SNAPSHOT</version>
</parent>
```

It centralizes what all spec repos share: plugin management (bnd-maven-plugin
with the common bundle headers, bnd baseline against the last release on
Maven Central, bnd resolver/testing for the TCK runs, flatten for CI-friendly
versions, sources/javadoc/gpg and Central publishing for deploys) and
dependency management for the test infrastructure (JUnit, AssertJ, Mockito).

SNAPSHOTs are deployed to
[Sonatype Central snapshots](https://central.sonatype.com/repository/maven-snapshots/).
Maven Central does not serve snapshots, so as long as the parent is consumed
as a SNAPSHOT, the referencing pom must declare that repository itself — a
`<repositories>` entry in the parent cannot be used to resolve the parent:

```xml
<repositories>
	<repository>
		<id>central-snapshots</id>
		<url>https://central.sonatype.com/repository/maven-snapshots/</url>
		<releases>
			<enabled>false</enabled>
		</releases>
		<snapshots>
			<enabled>true</enabled>
		</snapshots>
	</repository>
</repositories>
```

Alternatively, install it into the local repository:

```
mvn install
```

## Releases

The version is the literal `<version>` in `pom.xml`. A release is cut by
pushing a git tag that equals that version, without any prefix (e.g.
`0.0.1`); the release workflow refuses a mismatching tag or a SNAPSHOT
version. SNAPSHOTs are deployed weekly by the build workflow. Unlike the
spec repos, this repository has its own workflows — the shared spec
workflows in [osgi/.github](https://github.com/osgi/.github) assume the
spec repo layout.

License: [Apache-2.0](LICENSE)
