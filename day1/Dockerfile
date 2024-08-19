FROM oraclelinux:8.7 
# from will pull the image from docker hub to docker server
LABEL name="ashutoshh"
LABEL email="ashutoshh@linux.com"
# label is optional field but you write to share image creator info 
RUN dnf install httpd -y 
# installing httpd server in docker image 
COPY .  /var/www/html/
# copy all webcode data to /var/www/html/ of container image 
CMD ["httpd","-D","FOREGROUND"]
# starting httpd service here we can't write systemctl start httpd 
# because docker doest not support 

