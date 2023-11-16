
# mvn-release-with-git-over-ssh



Call the below command

mvn -B release:clean org.codehaus.mojo:build-helper-maven-plugin:parse-version pl.project13.maven:git-commit-id-plugin:revision release:prepare release:perform -DreleaseVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}+\${rebuild.identifier}.git.\${git.commit.id.abbrev} -DdevelopmentVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}-SNAPSHOT

to release the version 
from say, 0.0.20-SNAPSHOT
as 0.0.20+2023-11-16.22-09-43.471.git.a508066
and set the version back 
to say, 0.0.20-SNAPSHOT

TODO:
1. try to achieve the effect of the above command without defining the plugins in pom.xml
2. if 1 is not possible, try to add the plugins to a bom and use the bom as parent to achieve the same
