# [INFO] BUILD FAILURE (Spring Boot)
> Error

[`INFO`] BUILD FAILURE
[`INFO`] ------------------------------------------------------------------------
[`INFO`] Total time:  0.839 s
[`INFO`] Finished at: 2024-10-26T12:15:23+06:00
[`INFO`] ------------------------------------------------------------------------
[`ERROR`] Failed to execute goal org.apache.maven.plugins:maven-resources-plugin:3.3.1:resources (default-resources) on project student: Cannot create resource output directory: /opt/docker-student-sync/student-be/target/classes -> [Help 1]
[`ERROR`]
[`ERROR`] To see the full stack trace of the errors, re-run Maven with the -e switch.
[`ERROR`] Re-run Maven using the -X switch to enable full debug logging.
[`ERROR`]
[`ERROR`] For more information about the errors and possible solutions, please read the following articles:
`[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException


> Solved

1. `Check Permissions:` Ensure that the user running Maven has write permissions on the /opt/docker-student-sync/student-be/target directory.
```
sudo chown -R $(whoami) /opt/docker-student-sync/student-be/target
```
2. `lose Open Resources:` Check if any processes are using files within the /target directory. Sometimes, IDEs, editors, or other processes can lock files.
```
lsof +D /opt/docker-student-sync/student-be/target
```
3. `Delete Manually:` Try deleting the /target directory manually and re-run the Maven command.
```
sudo rm -rf /opt/docker-student-sync/student-be/target
```
4. `Run Maven as Root (if necessary):` If running on Linux with restricted permissions, try using `sudo` to run Maven:
```
sudo mvn clean install
```

**If NGINX running first Stop NGINX**
```
sudo systemctl stop NGINX
```