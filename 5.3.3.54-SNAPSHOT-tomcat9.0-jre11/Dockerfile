FROM tomcat:9.0.30-jdk11-openjdk

# Replace the Trusted SSL Certificates packaged with Lucee with those from Java
RUN mkdir -p /opt/lucee/server/lucee-server/context/security \
	&& test -e $JAVA_HOME/jre/lib/security/cacerts && cp -f $JAVA_HOME/jre/lib/security/cacerts /opt/lucee/server/lucee-server/context/security/cacerts || true \
	&& test -e $JAVA_HOME/lib/security/cacerts && cp -f $JAVA_HOME/lib/security/cacerts /opt/lucee/server/lucee-server/context/security/cacerts || true

# Delete the default Tomcat webapps so they aren't deployed at startup
RUN rm -rf /usr/local/tomcat/webapps/*

# Custom setenv.sh to load Lucee
# Tomcat memory settings
# -Xms<size> set initial Java heap size
# -Xmx<size> set maximum Java heap size
ENV LUCEE_JAVA_OPTS "-Xms1024m -Xmx1024m"
ENV LUCEE_EXTENSIONS "6A03159F-C6DC-4FCC-95A068B1E1A903FA;name=OrtusMongoDB;version=1.0.0+2213,ortuspdf-extension;name=OrtusPDF;version=2.0.0.728,37C61C0A-5D7E-4256-8572639BE0CF5838;name=OWASP;version=2.1.0.17,CED6227E-0F49-6367-A68D21AACA6B07E8;Name=LuceeAdmin;version=1.0.0.3,7E673D15-D87C-41A6-8B5F1956528C605F;Name=MySQL;version=8.0.16,99A4EF8D-F2FD-40C8-8FB8C2E67A4EEEB6;Name=MSSQL;version=7.2.2.jre8,16FF9B13-C595-4FA7-B87DED467B7E61A0;Name=Memcached;version=3.0.2.29,D4EDFDBD-A9A3-E9AF-597322D767E0C949;Name=Oracle;version=12.1.0.2,17AB52DE-B300-A94B-E058BD978511E39E;Name=s3;version=0.9.4.119,EFDEB172-F52E-4D84-9CD1A1F561B3DFC8;Name=Lucene;version=2.4.1.32,3F9DFF32-B555-449D-B0EB5DB723044045;Name=LuceeWebsockets;version=2.0.3,037A27FF-0B80-4CBA-B954BEBD790B460E;Name=CFSpreadsheet;version=3.0.3,5C558CC6-1E67-4776-96A60F9726D580F1;Name=OrtusRedisCache;version=1.4.0+2303"

# Download Lucee JAR
RUN mkdir -p /usr/local/tomcat/lucee
ADD https://release.lucee.org/rest/update/provider/loader/5.3.3.54-SNAPSHOT /usr/local/tomcat/lucee/lucee.jar

# Delete the default Tomcat webapps so they aren't deployed at startup
RUN rm -rf /usr/local/tomcat/webapps/*

# Set Tomcat config to load Lucee
COPY catalina.properties \
	server.xml \
	web.xml \
	/usr/local/tomcat/conf/

COPY rewrite.config /usr/local/tomcat/conf/Catalina/127.0.0.1/

# Custom setenv.sh to load Lucee
COPY setenv.sh /usr/local/tomcat/bin/
RUN chmod a+x /usr/local/tomcat/bin/setenv.sh

# Create Lucee configs
COPY lucee-server.xml /opt/lucee/server/lucee-server/context/lucee-server.xml
COPY lucee-web.xml.cfm /opt/lucee/web/lucee-web.xml.cfm

# Provide test page
RUN mkdir -p /var/www
COPY index.cfm /var/www/
ONBUILD RUN rm -rf /var/www/*

# lucee first time startup; explodes lucee and installs bundles/extensions (prewarms twice due to additional bundle downloads)
COPY prewarm.sh /usr/local/tomcat/bin/
RUN chmod +x /usr/local/tomcat/bin/prewarm.sh
RUN /usr/local/tomcat/bin/prewarm.sh && /usr/local/tomcat/bin/prewarm.sh