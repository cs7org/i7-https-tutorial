# i7-https-tutorial
Tutorial how to set up a HTTPS webserver and load websites using different HTTPS versions

> [!IMPORTANT]
> Work in progress

## Public server with DNS record
Use the cheapest Debian [Hetzner Cloud Server](https://www.hetzner.com/cloud) (costs little money but allows for easy deletion). Alternatively, you might be able to use your [home Internet access](https://fritz.com/service/wissensdatenbank/dok/FRITZ-Box-7590/30_Dynamic-DNS-in-FRITZ-Box-einrichten/) (use at your own risk, we recommend dedicated hardware for the server, e.g., a Raspberry Pi).

A valid domain and DNS record is needed for the next step, which can be provided by [Dynamic DNS services](https://www.cloudflare.com/learning/dns/glossary/dynamic-dns/), (e.g., https://noip.com).

> Frequently used domain names, which includes the DNS record provided by Hetzner, might cause problems in the next step:
`too many certificates (50) already issued for "your-server.de" in the last 168h0m0s, retry after 2025-01-23 12:35:56 UTC: see https://letsencrypt.org/docs/rate-limits/#new-certificates-per-registered-domain`

- [ ] TODO: Provide instructions how to run server locally


## Let's Encrypt certificates
Certificates are required for HTTP*S* which is mandatory for HTTP/2 and HTTP/3.
This is done using Let's encrypt, e.g., using the [Certbot](https://certbot.eff.org/instructions?ws=other&os=pip).

The outcome should be a public and private key, typically available as\
`/etc/letsencrypt/live/your-domain-name.ddns.net/fullchain.pem`\
`/etc/letsencrypt/live/your-domain-name.ddns.net/privkey.pem`


## Install OpenLiteSpeed webserver
We use the OpenLiteSpeed webserver, because it supports HTTP/3 QUIC out of the box. Of course, there are many alternatives.
Install OpenLiteSpeed as described [here](https://docs.openlitespeed.org/installation). In case you opt for the binary, check the latest available version for [download](https://openlitespeed.org/downloads/).


## Configure OpenLiteSpeed
TODO


## Add NetEm delay for *geostationary satellite* latency
As shown in the lecture notes
