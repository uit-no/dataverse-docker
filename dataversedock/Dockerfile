FROM centos:7.2.1511
# OS dependencies
RUN yum install -y java-1.8.0-openjdk-headless sudo epel-release unzip perl curl
RUN yum install -y wget patch postgresql lynx jq

# copy and unpack dependencies (solr, glassfish)
COPY lang.properties /opt/dv
COPY lang.templates /opt/dv
WORKDIR /opt/dv
COPY dv /opt/dv

#COPY dv/deps/glassfish4 /opt/glassfish4
COPY dv/deps/glassfish-4.1.zip /opt/
RUN unzip /opt/glassfish-4.1.zip -d /opt
RUN rm /opt/glassfish4/glassfish/modules/weld-osgi-bundle.jar
COPY dv/deps/weld-osgi-bundle.jar /opt/glassfish4/glassfish/modules/weld-osgi-bundle.jar

# glassfish port
EXPOSE 8080

COPY dv/deps/dvinstall.zip /opt/dv
COPY docker.patch /opt/dv

# yeah - still not happy if glassfish isn't in /usr/local :<
RUN ln -s /opt/glassfish4 /usr/local/glassfish4
COPY langswitch.sh /opt/dv
#COPY dv/install/ /opt/dv/
COPY install.bash /opt/dv/
COPY entrypoint.bash /opt
COPY testdata /opt/dv/testdata
COPY testscripts/* /opt/dv/testdata/
COPY setupIT.bash /opt/dv
RUN echo "Installing and running Dataverse..."
CMD ["/opt/entrypoint.bash"]
