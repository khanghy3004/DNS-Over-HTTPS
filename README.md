# DNS-Over-HTTPS

## Running

### HTTPS-to-DNS proxy

This lets you offer DNS-over-HTTPS for your own DNS server.

First you need to SSL certificate for your server because the broker will only request DNS lookups from a secure server. Normally I'd go 
with Letsencrypt but unfortunately they won't issue a certificate for a raw IP address.

If you don't want to pay for a "real" certificate I've included a script to build a self signed for the IP address you are going to run this on.

`./mkCert.sh 192.168.1.1`

You will be asked for a password 3 times, this will not be needed again but needs to match all 3 times.

To run on the default port 443 and query the Google 8.8.8.8 DNS server then use the following:

`node https-to-dns-proxy.js`

You can change the DNS server to query by passing it on the command line:

`node https-to-dns-proxy.js 192.168.1.1`

Once running point `https://<ip-address>/test` and accept the security exception to import and trust the certificate.

### Configuring Firefox (version 60 or newer)

+ Enter `about:config` in the address bar
+ search for `network.trr`
+ change `network.trr.mode` to either 1, 2 or 3. 
    - 1 Firefox pick the quickest
    - 2 Firefox trys DNS-Over-HTTPS first and falls back to DNS
    - 3 Firefox only uses DNS-Over-HTTPS
+ change `network.trr.uri` to `https://<ip-address>/query`
+ change `network.trr.bootstrapAddress` to `<ip-address>`

### DNS-to-HTTPS proxy

This lets you proxy normal DNS queries to a DNS-over-HTTPS enabled server.

By default this will try to bind to port 53 which will require admin level access. On Linux you can get with with sudo.

`sudo node dns-to-https-proxy`
