FROM        ubuntu:12.04
MAINTAINER  Jeff Smick <jeff@squishtech.com>

RUN apt-get update
RUN apt-get install -y libglib2.0-dev
RUN apt-get install -y libssl-dev
RUN apt-get install -y g++
RUN apt-get install -y make
RUN apt-get install -y xsltproc
RUN apt-get install -y xmlto

RUN mkdir -p /bitlbee/bin
RUN mkdir -p /bitlbee/state/configs

ADD https://launchpad.net/bitlbee/1.2/3.2.1/+download/bitlbee-3.2.1.tar.gz /bitlbee-3.2.1.tar.gz
RUN tar zxf bitlbee-3.2.1.tar.gz
RUN cd bitlbee-3.2.1 && ./configure --ssl=openssl
RUN cd bitlbee-3.2.1 && make
RUN cd bitlbee-3.2.1 && make install

RUN apt-get remove -y libglib2.0-dev
RUN apt-get remove -y libssl-dev
RUN apt-get remove -y g++
RUN apt-get remove -y make
RUN apt-get remove -y xsltproc
RUN apt-get remove -y xmlto
RUN apt-get autoremove -y
RUN apt-get clean

ADD run /bitlbee/run
RUN chmod +x /bitlbee/run

RUN adduser --group bitlbee
RUN useradd -g bitlbee bitlbee
RUN chown -R bitlbee:bitlbee /bitlbee

USER bitlbee
EXPOSE 6667
ENTRYPOINT ["/bitlbee/run"]
CMD ["noop"]
