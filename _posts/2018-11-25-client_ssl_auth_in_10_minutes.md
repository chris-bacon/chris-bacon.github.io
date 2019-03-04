---
title: client_ssl_auth_in_10_minutes
layout: post
---

### Generate CA root private key

<pre>
openssl genrsa -des3 -out root-ca.key 1024
</pre>

### Use the key to generate and sign its own cert

<pre>
openssl req -new -x509 -days 3650 -key root-ca.key -out root-ca.crt -config openssl.cnf 
</pre>

## Client

### Create private key and certificate

<pre>
openssl req -newkey rsa:1024 -keyout chris.key -config openssl.cnf -out chris.csr
</pre>

### Use the CA root cert to sign the client certificate

<pre>
openssl ca -keyfile root-ca.key -cert root-ca.crt -out chris.crt -infiles chris.csr
</pre>


### Troubleshoting 

Create the index.txt file.

<pre>
touch /etc/pki/CA/index.txt
</pre>

Create a serial file to label the CA and all subsequent certificates.

<pre>
# echo '1000' > /etc/pki/CA/serial
</pre>

You will only need to do this the first time you set up the SSL certificate. Re-run the command:

<pre>
openssl ca -keyfile root-ca.key -cert root-ca.crt -out chris.crt -infiles chris.csr
</pre>


#### References

- http://pages.cs.wisc.edu/~zmiller/ca-howto/
- https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Virtualization/3.2/html/Developer_Guide/Creating_an_SSL_Certificate.html