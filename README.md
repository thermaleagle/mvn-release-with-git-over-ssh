
# mvn-release-with-git-over-ssh



Call mvn org.codehaus.mojo:build-helper-maven-plugin:parse-version pl.project13.maven:git-commit-id-plugin:revision versions:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}+git.\${git.commit.id.abbrev}-SNAPSHOT
to change version 
from say, 0.0.20-SNAPSHOT
to say, 0.0.20+git.b667be4-SNAPSHOT


TODO:
1. try to achieve the effect of the above command without defining it in pom.xml
2. if 1 is not possible, try to add the plugins to a bom and use the bom as parent to achieve the same