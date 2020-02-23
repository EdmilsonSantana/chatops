FROM netdata/netdata

# Install hubot dependencies
RUN apk update && apk upgrade \
 && apk add autoconf automake bash build-base \
          nodejs=10.14.2-r0 nodejs-npm=10.14.2-r0 python=2.7.16-r2 \
          libmnl libmnl-dev libuuid util-linux-dev \
          zlib zlib-dev \
 && npm install -g coffeescript \
 && npm install -g yo generator-hubot \
 && rm -rf /var/cache/apk/*

# Create hubot workdir
ENV HUBOT_HOME /home/hubot
ENV NETDATA_HOME /etc/netdata
RUN mkdir $HUBOT_HOME && chown -R netdata:netdata $HUBOT_HOME && \
    chown -R netdata:netdata $NETDATA_HOME
USER netdata
WORKDIR $HUBOT_HOME

# Install hubot
ENV HUBOT_NAME "isac"
ENV HUBOT_OWNER "Edmilson Santana <edmilsonmgsantana@gmail.com>"
ENV HUBOT_ADAPTER "mattermost"
ENV HUBOT_DESCRIPTION "A robot may not harm humanity, or, by inaction, allow humanity to come to harm"

RUN yo hubot\
 --adapter=$HUBOT_ADAPTER \
 --owner="$HUBOT_OWNER" \
 --name="$HUBOT_NAME" \
 --description="$HUBOT_DESCRIPTION" \
 --defaults

COPY package.json package.json
ADD external-scripts.json $HUBOT_HOME
RUN rm hubot-scripts.json
COPY scripts/* $HUBOT_HOME/scripts/
COPY netdata-stream.slave.conf /etc/netdata/stream.conf

RUN npm install

ENV EXPRESS_PORT 8086
EXPOSE $EXPRESS_PORT

ENTRYPOINT ["/bin/sh", "-c", "netdata && ./bin/hubot -a $HUBOT_ADAPTER"]