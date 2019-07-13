## Setting the MISP Server and Creating Threat Events using PyMISP

Malware Information Sharing Platform (MISP) is an open source threat intelligence platform that develops utilities and documentation for more effective threat intelligence, by sharing indicators of compromise. 

The goal of this PoC to setup the MISP Docker instance and manually create threat events with tags. Later we will automate the threat information sharing using PyMISP. 

Let's breakdown the PoC into two steps.
*    MISP Setup
*    Event Generation from PyMISP

Before continuing, please make sure Python3 and Docker are installed on your system. Also ensure ports 443,80 and 3306 are not used. If used please change the port mapping while starting the MISP container. 
 


### MISP Setup
    
*   Pull the MISP docker image from Dockerhub.
```
docker pull harvarditsecurity/misp
```

*  Make sure the MISP image is listed on available docker images.
```
docker images
```

*  Create a directory for MISP and  export a variable `dockerroot`  that specifies the path of created directory.
```
mkdir ~/misp
export dockerroot=~/misp
```

*  Create a Database directory and initialize Database.
```
mkdir -p $dockerroot/misp-db
docker run -it --rm  -v $dockerroot/misp-db:/var/lib/mysql harvarditsecurity/misp /init-db
```

* Start the MISP container
```
docker run -it -d -p 443:443 -p 80:80  -p 3306:3306 -v $dockerroot/misp-db:/var/lib/mysql  harvarditsecurity/misp
```

* Access Web URL and change the password.
```
Go to: https://localhost 

Login: admin@admin.test
Password: admin
```

* Go to MISP homepage  
![Alt text](./screenshots/misp-home.png?raw=true "MISP Homepage")

* Add Threat Event 
![Alt text](./screenshots/misp-add-event.png?raw=true "Add Threat Event")

* Add new tag  
![Alt text](./screenshots/misp-add-tag-menu.png?raw=true "Add Tag Menu")
![Alt text](./screenshots/misp-add-tag.png?raw=true "Add Tag")                  

* List available tags
![Alt text](./screenshots/misp-list-tags.png?raw=true "List Tags")

* Link tag to the created event
![Alt text](./screenshots/misp-link-tag-event.png?raw=true "Link tag to an event")

* List of events with tag(s)
![Alt text](./screenshots/misp-list-events-with-tag.png?raw=true "List Events+Tags")




### Event Creation using PyMISP



* **References**
    * https://github.com/harvard-itsecurity/docker-misp
