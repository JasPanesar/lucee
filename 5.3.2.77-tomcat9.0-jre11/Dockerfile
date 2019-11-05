FROM lucee/lucee:5.3.2.77-tomcat9.0-jre11

COPY server.xml /usr/local/tomcat/conf/
COPY rewrite.config /usr/local/tomcat/conf/Catalina/127.0.0.1/

RUN mkdir -p /var/www
COPY index.cfm /var/www/
ONBUILD RUN rm -rf /var/www/*