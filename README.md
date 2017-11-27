# adamantine-fah
Dockerfile for starting a Folding@Home service from a Debian base image.

Change "workernode" to another username if desired (not required). You may also change the POWER environment variable to "low" or "high", this will determine how much of your CPU will be used for folding ("medium" should go no higher than 80% usage). 

### GPU support
It may also be possible to enable your GPU by changing the sed line to:

sed -i -e "s/<team value=\"0\"\/>/<team value=\""$TEAM"\"\/>/;s/<user value=\"Anonymous\"\/>/<user value=\""$USERNAME"\"\/>/;s/<power value=\"medium\"\/>/<power value=\""$POWER"\"\/>/**;s/<gpu value=\"false\"\/>/<power value=\"true\"\/>/**" /etc/fahclient/config.xml

But I have not tested this.

### About the image/config

I tried using a slim image, but the fahclient install errors out when it looks for a sample config.xml file in the /usr directory, but the directory does not exist.

The reason for the long sed command is since the Folding@Home client really only needs a few configuration settings changed in config.xml, it's simpler to edit it as a oneliner instead of having to include a "config.xml" template and 

"ADD config.xml /etc/fahclient/config.xml" 

in the Dockerfile. This should also be better future versions of the client.

*Note: this image does not handle the interrupt signal, so you won't be able to ctrl+c or ctrl+p, ctrl+q out of it. You'll have to do "docker stop imageID" instead.*
