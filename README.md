From: http://www.faqforge.com/windows/use-openssl-on-windows/

And: http://techbrahmana.blogspot.com/2013/10/creating-wildcard-self-signed.html

# create private key (KEY)
openssl genrsa -des3 -out your.domain.key 4096

# generate certificate request (CSR)
openssl req -config openssl.cfg -new -key your.domain.key -out your.domain.csr 
- for FQDN use your.domain, everything else is whatever

# sign certificate (CRT)
openssl x509 -req -extensions v3_req -days 365 -in your.domain.csr -signkey your.domain.key -out your.domain.crt -extfile altNames.txt

# export to pfx for windows (PFX)
openssl pkcs12 -export -inkey your.domain.key -in your.domain.crt -out your.domain.pfx -name "*.your.domain"
- the -name "*.your.domain" part is important to make sure IIS allows you to bind the same certificate to different sites on different subdomains

This will create the wildcard self signed cert, with two alt names defined in altNames.txt, just import into IIS!
