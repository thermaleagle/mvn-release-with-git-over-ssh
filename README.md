
# mvn-release-with-git-over-ssh



Call the below command

mvn org.codehaus.mojo:build-helper-maven-plugin:parse-version pl.project13.maven:git-commit-id-plugin:revision versions:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}+git.\${git.commit.id.abbrev}.\${rebuild.identifier}-SNAPSHOT

to change version 
from say, 0.0.20-SNAPSHOT
to say, 0.0.20+git.a508066.20231116220943471-SNAPSHOT


TODO:
1. try to achieve the effect of the above command without defining it in pom.xml
2. if 1 is not possible, try to add the plugins to a bom and use the bom as parent to achieve the same