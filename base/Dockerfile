FROM openjdk:8

# use chinese apt mirror
COPY sources.list /etc/apt/sources.list

# delete net-tools
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \ 
	curl \
	netcat \
&& rm -rf /var/lib/apt/lists/*

ENV HBASE_VERSION 2.2.5
ENV HBASE_URL http://mirror.bit.edu.cn/apache/hbase/$HBASE_VERSION/hbase-$HBASE_VERSION-bin.tar.gz
ENV HBASE_KEY_URL http://www.apache.org/dist/hbase/$HBASE_VERSION/hbase-$HBASE_VERSION-bin.tar.gz.asc
RUN set -x \
	&& curl -fSL "$HBASE_KEY_URL" -o /tmp/hbase.tar.gz.asc \
	&& curl -fSL "$HBASE_URL" -o /tmp/hbase.tar.gz \
	&& tar -xvf /tmp/hbase.tar.gz -C /opt/ \
	&& rm /tmp/hbase.tar.gz* \
	&& ln -s /opt/hbase-$HBASE_VERSION/conf /etc/hbase \
	&& mkdir /opt/hbase-$HBASE_VERSION/logs \
	&& mkdir /hadoop-data

ENV HBASE_PREFIX=/opt/hbase-$HBASE_VERSION
ENV HBASE_CONF_DIR=/etc/hbase

ENV USER=root
ENV PATH $HBASE_PREFIX/bin/:$PATH

COPY entrypoint.sh /entrypoint.sh
RUN chmod a+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
