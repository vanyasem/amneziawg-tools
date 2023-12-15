FROM golang:1.20 as build

RUN \
apt-get -y update && \
apt-get -y install iproute2 iptables iputils-ping iperf3

RUN \
cd /root && \
git clone -b master https://github.com/amnezia-vpn/amnezia-wg.git && \
cd amnezia-wg && \
go mod download && \
go mod verify && \
go build -ldflags '-linkmode external -extldflags "-fno-PIC -static"' -v -o /usr/bin

RUN \
cd /root && \
git clone -b new_fields_to_config https://github.com/amnezia-vpn/amnezia-wg-tools.git && \
cd /root/amnezia-wg-tools/src && \
make -e LDFLAGS=-static && \
make install

FROM alpine:3.15

RUN apk add iproute2 bash

COPY --from=build /usr/bin/amnezia-wg /usr/bin/wireguard-go
COPY --from=build /usr/bin/wg /usr/bin/wg-quick /usr/bin/
