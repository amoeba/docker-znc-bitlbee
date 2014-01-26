FROM        ubuntu:12.04
MAINTAINER  Jeff Smick <jeff@squishtech.com>

RUN apt-get update
RUN apt-get install -y libssl-dev
RUN apt-get install -y automake
RUN apt-get install -y gettext
RUN apt-get install -y g++
RUN apt-get install -y make

ADD http://znc.in/releases/znc-1.2.tar.gz /znc-1.2.tar.gz
RUN tar zxf znc-1.2.tar.gz
RUN cd znc-1.2 && ./configure
RUN cd znc-1.2 && make
RUN cd znc-1.2 && make install

ADD https://raw2.github.com/Palaver/znc-palaver/master/palaver.cpp /palaver.cpp
RUN znc-buildmod palaver.cpp
RUN cp palaver.so /usr/local/lib/znc/palaver.so

ADD https://raw.github.com/kylef/znc-contrib/master/clientaway.cpp /clientaway.cpp
RUN znc-buildmod clientaway.cpp
RUN cp clientaway.so /usr/local/lib/znc/clientaway.so

RUN apt-get remove -y libssl-dev
RUN apt-get remove -y automake
RUN apt-get remove -y gettext
RUN apt-get remove -y g++
RUN apt-get remove -y make
RUN apt-get autoremove -y
RUN apt-get clean

RUN mkdir -p /znc
ADD run /znc/run
RUN chmod +x /znc/run

RUN adduser --group znc
RUN useradd -g znc znc
RUN chown -R znc:znc /znc

USER znc
ENTRYPOINT ["/znc/run"]
CMD ["noop"]
