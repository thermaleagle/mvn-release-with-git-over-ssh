
# mvn-release-with-git-over-ssh



Call the below command

mvn -B org.codehaus.mojo:build-helper-maven-plugin:parse-version pl.project13.maven:git-commit-id-plugin:revision versions:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}+git.\${git.commit.id.abbrev}.\${rebuild.identifier}-SNAPSHOT release:clean release:prepare release:perform 

to change version 
from say, 0.0.20-SNAPSHOT
to say, 0.0.20+git.a508066.20231116220943471-SNAPSHOT

and then perform a clean release:prepare and release:perform
to release the verison 0.0.20+git.a508066.20231116220943471

and then run
org.codehaus.mojo:build-helper-maven-plugin:parse-version pl.project13.maven:git-commit-id-plugin:revision versions:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}-SNAPSHOT

to change latest version back to
to say, 0.0.20-SNAPSHOT

TODO:
1. try to achieve the effect of the above command without defining it in pom.xml
2. if 1 is not possible, try to add the plugins to a bom and use the bom as parent to achieve the same
3. Remove the need to ignore local modifications within the pom.xml during release:prepare
4. Make sure the tag added matches (scm.tagNameFormat) matches the version number released including the above build metadata