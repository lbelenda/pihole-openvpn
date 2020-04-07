1. Install OpenVPN

```$docker volume create --name ovpn-data

server ip=52.66.18.251

$docker run -v ovpn-data:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://52.66.18.251

Certificate Generation take 2-5 minutes.

$docker run -v ovpn-data:/etc/openvpn --rm -it kylemanna/openvpn ovpn_initpki
```

Openvpn running in port 1194

```$docker run -v ovpn-data:/etc/openvpn --name openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn```

Create User "example" without Password, use the keypharse used in certificate generation

```$docker run -v ovpn-data:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full example nopass```

Download the "example.ovpn" from docker volume to local volume

```$docker run -v ovpn-data:/etc/openvpn --rm kylemanna/openvpn ovpn_getclient example > example.ovpn```

2. Get Pihole IP

```$docker inspect pihole```

3. OpenVPN + PiHole


```
$docker exec -it openvpn /bin/bash

$export TERM=xterm

$apk update 

$apk add nano 

$nano /etc/openvpn/openvpn.conf

Push Configurations Below

push block-outside-dns

push dhcp-option DNS 172.17.0.3

#push dhcp-option DNS 8.8.4.4
```


finally restart openvpn

`$docker restart openvpn`

Note: Set WEBPASSWORD for Pihole in docker-compose file ot ir will be random
