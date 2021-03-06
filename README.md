# abuse-docker
Standalone image for [AbuseIO](http://abuse.io) running on NGINX with MySQL, fetchmail and procmail

### to build

    # docker build -t abuseio:latest .

### to run

    # docker run -d -p 8000:8000 -p 3306:3306 -v <host_config_dir>:/config -v <host_data_dir>:/data -v <host_log_dir>:/log abuseio:latest
    
and connect your browser to [http://localhost:8000/](http://localhost:8000/)

### to update

It's recommended to pull the new image and create a new container, however you can update to a newer AbuseIO by running the update script in the container 

    # docker exec -t -i <container_id> /scripts/update_abuseio.sh

After the update (in the container or a new container) you should check if your config still works.

### configuration
During the first boot of the container, AbuseIO will create an admin account and setup a default AbuseIO instance. The credentials, for the admin account, will be shown during this setup.

The  `/config`  volume,  contains the basic settings for AbuseIO, most of them are set to default values.

Mail settings can be set in `fetchmailrc` \( incoming \) and `abuseio.env` \( outgoing \)  for more information about these file see the links below. When you edit fetchmailrc, don't delete or alter the last line.
    
    mda "/usr/bin/procmail -m /etc/procmailrc"
    
This line ensures that the mails are delivered to AbuseIO.


Others setting for  e.g. parsers, collectors and  find-contact modules can be found in the `/config/abuseio` directory.

 - [AbuseIO environment settings](https://docs.abuse.io/en/latest/installation/#environment-settings)
 - [AbuseIO main configuration](https://docs.abuse.io/en/latest/configuration_main/)
 - [Gmail POP3 with fetchmail](https://www.axllent.org/docs/view/gmail-pop3-with-fetchmail/)
 - [Using Fetchmail to Retrieve Email](https://www.linode.com/docs/email/clients/using-fetchmail-to-retrieve-email)

### ports
NGINX is accessible on container port 8000 and MySQL is accessible on port 3306. These ports can be published 
on the host by using the -p option of Docker, see [incoming ports](https://docs.docker.com/engine/reference/run/#expose-incoming-ports)
of the Docker Manual.

### volumes
The container exports three volumes

 - `/config`
   all the necessary files to config AbuseIO e.g. mail credentials 
   
 - `/data`
   persistent data: database and mailarchive

 - `/log`
   logging from AbuseIO, NGINX and procmail
   
 
The volumes can be mapped to local persistent storage, using the -v option of Docker, see [mount volume](https://docs.docker.com/engine/reference/commandline/run/#mount-volume--v---read-only) of the Docker manual for more information
