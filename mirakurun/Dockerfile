FROM node:10-stretch-slim
LABEL maintainer="h-mineta <h-mineta@0nyx.net>, Mirai Kim <me@euc-kr.net>"
ENV DOCKER="YES"
RUN set -x \
	&& apt-get update \
	&& apt-get install -y \
		bash \
		coreutils \
		libusb-1.0-0-dev \
		libpcsclite1 \
		libpcsclite-dev \
		pcscd \
		curl \
		wget \
		unzip \
		ca-certificates \
		util-linux \
	&& apt-get install --no-install-recommends -y \
		git \
		make \
		cmake \
		gcc \
		g++ \
		gzip \
		autoconf \
		automake \
		pkg-config \
		libc-dev \
		musl-dev \
		libevent-dev \
		libusb-dev \
		libtool\
		flex\
	&& npm install pm2 -g \
	&& npm install arib-b25-stream-test -g --unsafe \
	# mirakurun
	&& npm install mirakurun@latest -g --unsafe --production \
	# ccid
	&& cd /tmp \
	&& git clone --recursive https://salsa.debian.org/rousseau/CCID.git \
	&& cd CCID \
	&& ./bootstrap \
	&& ./configure \
	&& make \
	&& make install \
	# libarib25
	&& wget https://github.com/stz2012/libarib25/archive/master.zip -O /tmp/libarib25.zip \
	&& unzip /tmp/libarib25.zip -d /tmp/ \
	&& cd /tmp/libarib25-master \
	&& cmake . \
	&& make \
	&& make install \
	# recdvb
	&& wget http://www13.plala.or.jp/sat/recdvb/recdvb-1.3.2.tgz -O /tmp/recdvb.tgz \
	&& mkdir /tmp/recdvb \
	&& tar xvzf /tmp/recdvb.tgz --strip-components=1 -C /tmp/recdvb \
	&& cd /tmp/recdvb \
	&& ./autogen.sh \
	&& ./configure --enable-b25 \
	&& make \
	&& EXTRA_SID=1 make \
	&& make install \
	# cleaning
	&& cd / \
	&& npm cache verify \
	&& apt-get clean \
 	&& rm -rf /var/lib/apt/lists/* \
	#&& rm -rf /tmp/* \
	&& rm -rf /tmp/recdvb* \
	&& rm -rf /tmp/ccid-*
	# forward request and error logs to docker log collector
	#&& ln -sf /dev/stdout /usr/local/var/log/mirakurun.stdout-0.log \
	#&& ln -sf /dev/stderr /usr/local/var/log/mirakurun.stderr-0.log
COPY services.sh /usr/local/bin
WORKDIR /usr/local/lib/node_modules/mirakurun
CMD ["/usr/local/bin/services.sh"]
EXPOSE 40772
