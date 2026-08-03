# Nexus Repository Manager Installation on Ubuntu

An example of how to install and configure Nexus Repository Manager on an Ubuntu server.

## Install Java
- Install Java 17 using `apt install openjdk-17-jre-headless`

## Install Nexus Repository Manager
- Navigate to the `/opt` directory
- Download the Nexus archive using `wget <download link>`
- Extract the archive using `tar -zxvf <archive-name>`

## Configure Nexus User
- Create a dedicated service account using `adduser nexus`
- Change ownership of the Nexus directories using `chown -R nexus:nexus <directory>`
- Configure Nexus to run as the `nexus` user by editing `nexus-3.94.1-06/bin/nexus.rc` (create the file if it does not exist)

## Start Nexus
- Switch to the Nexus user using `su - nexus`
- Start Nexus using `/opt/nexus-3.94.1-06/bin/nexus start`
- Verify that Nexus is running using `ps aux | grep nexus` or `netstat -lnpt`

## Configure Network Access
- Open TCP port `8081` on the server firewall to allow inbound connections

## Access Nexus
- Open the server in a web browser using `http://<server-ip>:8081`
- Log in with the default `admin` account
- Retrieve the initial admin password from the directory displayed during startup
- Change the default admin password immediately after logging in

## Publish an Artifact
- Create a Nexus user with permission to upload artifacts:
  - Navigate to **Security → Users** and create a new user
  - Navigate to **Security → Roles** and create a new role
  - Add all privileges for the `maven-snapshots` repository views
  - Assign the new role to the user
- Configure the Gradle `maven-publish` plugin (see [commit](https://github.com/JonathanBaqDev/TWN-Nexus-Gradle/commit/33f170c4906c0dd4d0b7021e3cc76d5fb122ad60))
- Build the project using `gradle build`
- Publish the artifact using `gradle publish`
