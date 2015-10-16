FROM tomcat:8-jre8

MAINTAINER "Aitor Magán <amagan@conwet.com>"

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
ENV WMARKET_VERSION v4.4.4
ENV WMARKET_FILE WMarket.war

WORKDIR /wmarket

# Needed packages
RUN apt-get update && apt-get install -y zip

# Get the WAR and uncompress it
RUN wget https://github.com/conwetlab/WMarket/releases/download/$WMARKET_VERSION/$WMARKET_FILE
RUN unzip $WMARKET_FILE -d WMarket
RUN rm $WMARKET_FILE

# Configure database, media folder and index folder
RUN sed -i "s|^jdbc.url=.*$|jdbc.url=jdbc:mysql://wmarket_db:3306/marketplace|" WMarket/WEB-INF/classes/properties/database.properties
RUN sed -i "s|^jdbc.password=.*$|jdbc.password=my-secret-pw|" WMarket/WEB-INF/classes/properties/database.properties
RUN sed -i "s|^media.folder.*$|media.folder=/WMarket/static|" WMarket/WEB-INF/classes/properties/marketplace.properties
RUN sed -i "s|^lucene.IndexPath.*$|lucene.IndexPath=/WMarket/index|" WMarket/WEB-INF/classes/properties/marketplace.properties

# Recompress the WAR and upload the application to Tomcat
RUN cd WMarket && zip -r ../$WMARKET_FILE *
RUN cp $WMARKET_FILE /usr/local/tomcat/webapps