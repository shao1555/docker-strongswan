strongswan
==========

[strongSwan][1] is an Open Source IPsec-based VPN solution for Linux and other
UNIX based operating systems implementing both the IKEv1 and IKEv2 key exchange
protocols.

this docker image based on [vimagick/strongswan](https://github.com/vimagick/dockerfiles/tree/master/strongswan) with some modifications such as:
- add AWS CLI
- use latest strongSwan package provided by latest Alpine Linux.

> :warning: This docker image only support IKEv2!

### docker-compose.yml

```yaml
version: '2'
services:
  strongswan:
    image: shao1555/strongswan
    ports:
      - 500:500/udp
      - 4500:4500/udp
    volumes:
      - /lib/modules:/lib/modules
      - /etc/localtime:/etc/localtime
    environment:
      - VPN_DOMAIN=vpn.example.com
      - VPN_NETWORK=10.20.30.0/24
      - LAN_NETWORK=192.168.0.0/16
      - VPN_P12_PASSWORD=secret
    tmpfs: /run
    privileged: yes
    restart: always
```

### up and running

```bash
docker-compose up -d
docker cp strongswan_strongswan_1:/etc/ipsec.d/client.mobileconfig .
docker cp strongswan_strongswan_1:/etc/ipsec.d/client.cert.p12 .
docker-compose logs -f
```

- Mac/IOS: `client.mobileconfig`
- Android: `client.cert.p12`

[1]: https://strongswan.org/
