# i7-https-tutorial
Tutorial how to set up a HTTPS webserver and load websites using different HTTPS versions


## Public server with DNS record
Use the cheapest Debian [Hetzner Cloud Server](https://www.hetzner.com/cloud) (costs little money but allows easy experimentation).
Alternatively, you might be able to use your [home Internet access](https://fritz.com/service/wissensdatenbank/dok/FRITZ-Box-7590/30_Dynamic-DNS-in-FRITZ-Box-einrichten/) (use at your own risk, we recommend dedicated hardware for the server, e.g., a Raspberry Pi).

A valid domain and DNS record is needed for the next step, which can be provided by [Dynamic DNS services](https://www.cloudflare.com/learning/dns/glossary/dynamic-dns/), (e.g., https://noip.com).

> Frequently used domain names, which includes the domain provided by Hetzner for the server, might cause problems in the next step:
`too many certificates (50) already issued for "your-server.de" in the last 168h0m0s, retry after 2025-01-23 12:35:56 UTC: see https://letsencrypt.org/docs/rate-limits/#new-certificates-per-registered-domain`

- [ ] TODO: Maybe provide instructions how to run server locally


## Let's Encrypt certificates
Certificates are required for HTTP*S* which is mandatory for HTTP/2 and HTTP/3.
This is done using Let's encrypt, e.g., using the [Certbot](https://certbot.eff.org/instructions?ws=other&os=pip).

The outcome should be a certificate and a private key, typically available as\
`/etc/letsencrypt/live/your-domain-name.ddns.net/fullchain.pem`\
`/etc/letsencrypt/live/your-domain-name.ddns.net/privkey.pem`


## Install OpenLiteSpeed webserver
We use the OpenLiteSpeed webserver, because it supports HTTP/3 QUIC out of the box.
Of course, there are other alternatives.
Install OpenLiteSpeed as described [here](https://docs.openlitespeed.org/installation).
In case you opt for the binary, check the latest available version for [download](https://openlitespeed.org/downloads/).


## Configure OpenLiteSpeed
Configure OpenLiteSpeed as described [here](https://docs.openlitespeed.org/config/).
It is sufficient to change the Default Listener:
 - General: `ANY IPv4`, `Port 443`, `Secure Yes`
 - SSL: Path to private key and certificate, `Chained Certificate Yes` for Let's Encrypt certificates

Copy the `html` files from this repository to the OpenLiteSpeed webserver (e.g., `/usr/local/lsws/Example/html`).


## Test with your Browser
For example with Google Chrome:

- HTTP/1.1\
  `google-chrome --incognito --disable-quic --disable-http2 about:blank`
- HTTP/2\
  `google-chrome --incognito --disable-quic about:blank`
- HTTP/3\
  `google-chrome --incognito --enable-quic --origin-to-force-quic-on=quicsat.de:443 about:blank`

Right-click in window and select `Inspect` (or press `F12`), then select `Network` tab.

Load your website!


## Optional: Add NetEm delay for *geostationary satellite* latency
In the lecture notes, an additional delay of 600ms has been added to mimic the latency of a geostationary satellite path.\
`sudo tc qdisc add dev eth0 root handle 1:0 netem delay 600ms` (replace `eth0` with the interface of your webserver)\
This will add 600ms to the outgoing Ethernet interface of the webserver.

- [ ] TODO: Make this apply to UDP port 443 only

