FROM tomcat:9
EXPOSE 8080
ADD target/*.war /usr/local/tomcat/webapps