FROM	registry.redhat.io/rhel8/nodejs-12:1
ARG NEXUS_BASE_URL
MAINTAINER Md Mostarijullah Siddiquee <mossiddi@in.ibm.com>

COPY run.sh build ${HOME}/
#RUN npm install formidable@v3 && \
#  npm install mysql2 && \
#    npm install uuid && \
#    npm install
RUN npm install --registry=http://$NEXUS_BASE_URL/repository/nodejs/ && \
    npm install mysql2   
EXPOSE	30080

CMD	["./run.sh"]

