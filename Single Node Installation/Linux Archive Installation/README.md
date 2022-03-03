# Single Node Installation
## Linux Archive Installation
1. Set the JFrog Home environment variable
    > echo "export JFROG_HOME=/opt/jfrog" >> /etc/profile<br>
    source /etc/profile
2. Create a JFrog Home directory and move the downloaded installer archive into that directory, for example:
    > mkdir -p $JFROG_HOME<br>
    mv jfrog-artifactory-pro-\<version>-linux.tar.gz $JFROG_HOME/<br>
    cd $JFROG_HOME/
3. Extract the contents of the compressed archive and move it into the artifactory directory.
    > tar -xvf jfrog-artifactory-pro-\<version>-linux.tar.gz<br>
    mv artifactory-pro-\<version> artifactory
4. Customize the product configuration (optional) including database, Java Opts, and filestore.
    > vim $JFROG_HOME/artifactory/var/etc/system.yaml<br>
    shared:<br>
    ....<br>
    &nbsp;&nbsp;&nbsp;&nbsp;node:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: "art1"<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ip: "192.168.xx.xx”<br>
    ....
5. Run Artifactory as a foreground, background process, or as a service.
    >
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/artifactory/app/bin/artifactoryctl start |
6. Check Artifactory Log.
    >tail -f $JFROG_HOME/artifactory/var/log/console.log
7. Access Artifactory from your browser at: http://SERVER_HOSTNAME:8082/ui/. For example, on your local machine: http://localhost:8082/ui/.


