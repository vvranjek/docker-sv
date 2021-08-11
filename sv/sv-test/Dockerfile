#FROM debian:buster-slim
FROM ubuntu:18.04

RUN groupadd -r bitcoin && useradd -r -m -g bitcoin bitcoin

RUN set -ex \
	&& apt-get update \
	&& apt-get install -qq --no-install-recommends ca-certificates dirmngr gosu gpg wget libatomic1 \
	&& apt-get install libboost-system1.65.1 libboost-filesystem1.65.1 libboost-chrono1.65.1 libboost-program-options1.65.1 libboost-test1.65.1 libboost-thread1.65.1 \
	&& apt-get install libdb5.3++ libminiupnpc10 libminiupnpc-dev libssl1.1 libtool libgoogle-perftools4 libevent-pthreads-2.1-6 libevent-2.1-6 libzmq3-dev -y \
#	&& ln -s libminiupnpc.so.10 libminiupnpc.so.8 \
#	&& ldconfig -v \
	&& rm -rf /var/lib/apt/lists/*

ENV BITCOIN_VERSION 1.0.8


# install bitcoin binaries
RUN set -ex \
	&& cd /tmp \
	&& wget -O bitcoind "https://www.dropbox.com/s/qxk10xf55zmb39g/bitcoind1"  \
	&& chmod +x bitcoind \
	&& ls -a \
	&& cp bitcoind /usr/local/bin/ \
	&& rm -rf /tmp/*

#tar -xzvf bitcoin.tar.gz -C /usr/local --strip-components=1 --exclude=*-qt \


# create data directory
ENV BITCOIN_DATA /data
RUN mkdir "$BITCOIN_DATA" \
	&& chown -R bitcoin:bitcoin "$BITCOIN_DATA" \
	&& ln -sfn "$BITCOIN_DATA" /home/bitcoin/.bitcoin \
	&& chown -h bitcoin:bitcoin /home/bitcoin/.bitcoin
VOLUME /data

COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

EXPOSE 8332 8333 9332 9333 18332 18333
CMD ["bitcoind"]
