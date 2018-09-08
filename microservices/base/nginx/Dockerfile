FROM nginx:latest

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        bc \
        curl \
        unzip \
    && rm -rf /var/lib/apt/lists/*

#consul
ENV CONSUL_TEMPLATE_VERSION 0.19.5

#RUN curl --retry 7 -Lso /tmp/consul-template.zip \ "https://releases.hashicorp.com/consul-template/${CONSUL_TEMPLATE_VERSION}/ consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip" \
COPY file/consul-template_0.19.5_linux_amd64.zip /tmp/consul-template.zip
RUN unzip /tmp/consul-template.zip -d /usr/local/bin \
    && rm /tmp/consul-template.zip

#container pilot
ENV CONTAINERPILOT_VERSION 3.0.0
ENV CONTAINERPILOT /etc/containerpilot.json

#RUN curl --retry 7 -LsoS /tmp/containerpilot.tar.gz "https://github.com/joyent/containerpilot/releases/download/${CONTAINERPILOT_VERSION}/containerpilot-${CONTAINERPILOT_VERSION}.tar.gz" \
COPY file/containerpilot-3.0.0.tar.gz /tmp/containerpilot.tar.gz
RUN tar zxf /tmp/containerpilot.tar.gz -C /usr/local/bin \
    && rm /tmp/containerpilot.tar.gz

COPY config/ /etc
COPY scripts/ /usr/local/bin

COPY file/index.html /var/www/html/index.html

RUN chmod +x /usr/local/bin/reload.sh
RUN chmod +x /usr/local/bin/sensor.sh

COPY config/nginx/nginx.conf.ctmpl /etc/containerpilot/

CMD [ "/usr/local/bin/containerpilot", \
    "nginx", \
        "-g", \
        "daemon off;"]