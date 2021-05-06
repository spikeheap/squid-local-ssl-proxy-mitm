# Local Squid SSL Proxy

This project is an example of a local Squid proxy configuration to test path-based SSL restrictions.

It was built as a proof of concept to test the user experience of websites in very restricted environments (e.g. in-cell devices in prisons).

## Getting set up

0. Install Squid. This guide assumes you're using Homebrew on OSX:

```
brew install squid
```

1. Set up a new root CA to create certificates for proxied sites

```
openssl req -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509  -keyout squid-ca-key.pem -out squid-ca-cert.pem
cat squid-ca-cert.pem squid-ca-key.pem >> squid-ca-cert-key.pem
```

2. Initialise the SSL cert DB

> Note the Squid version in the path will likely be different

```
/usr/local/Cellar/squid/4.14/libexec/security_file_certgen -c -s /var/lib/ssl_db -M 16MB
```

3. Run Squid

```
/usr/local/sbin/squid -f squid.conf --foreground
```

4. Set your HTTP/HTTPS proxy to point to 127.0.0.1:3128
