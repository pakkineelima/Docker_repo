#This is the base image from which we want our image to be built 
FROM ubuntu:18.04
#This is the package manager for ubuntu to install the predefined packages
like we use yum for linux
# Install dependencies
RUN apt-get update && \
 apt-get -y install apache2
#this command will execute both the arguments like updating the apt-get and installing apache
# Install apache and write custom text
RUN echo 'Hello World!' > /var/www/html/index.html
#In this command we are giving a custom text and we are redirecting the text to the index.html file which is in /var/www/html
RUN echo 'Hello World!' > /var/www/html/index.html

#this is the layer of configuring the env.variables and redirecting them to the run_apache.sh which is in the default apache root folder

# Configure apache
RUN echo '. /etc/apache2/envvars' > /root/run_apache.sh && \
 echo 'mkdir -p /var/run/apache2' >> /root/run_apache.sh && \
 echo 'mkdir -p /var/lock/apache2' >> /root/run_apache.sh && \ 
 echo '/usr/sbin/apache2 -D FOREGROUND' >> /root/run_apache.sh && \ 
#-D FOREGROUND will bring the application to the user interface
 chmod 755 /root/run_apache.sh
#  read write execute permissons to the user, read and execute permissions to the usergroup and others are given with this for this file

EXPOSE 80
#this will expose the port where we want our application to run on and at the time of running the container this port should me mapped 
CMD /root/run_apache.sh
#this command will execute the file 
