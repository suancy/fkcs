FROM nginx:stable-alpine

##################################################################
####### The following lines come from the great Project V ########
##----- Check it out ==> https://github.com/v2ray/install/ -----##
##################################################################
ADD https://storage.googleapis.com/v2ray-docker/v2ray /usr/bin/v2ray/
ADD https://storage.googleapis.com/v2ray-docker/v2ctl /usr/bin/v2ray/
ADD https://storage.googleapis.com/v2ray-docker/geoip.dat /usr/bin/v2ray/
ADD https://storage.googleapis.com/v2ray-docker/geosite.dat /usr/bin/v2ray/

RUN set -ex && \
    apk --no-cache add ca-certificates && \
    mkdir /var/log/v2ray/ &&\
    chmod +x /usr/bin/v2ray/v2ctl && \
    chmod +x /usr/bin/v2ray/v2ray
##################################################################
######### The above lines come from the great Project V ##########
##----- Check it out ==> https://github.com/v2ray/install/ -----##
##################################################################

COPY myconf/* /etc/myconf/

CMD nginx -c /etc/myconf/nginx.conf ; v2ray -config /etc/myconf/config.json
