title = "Custom SSL certificate for iDrac 7 without CSR"
datetime = "2015-02-04 17:38"
tags = ["Dell", "drac", "cert", "ssl"]
------------

If you want to use the Dell iDrac with a certificate signed by your CA, the web gui allows you to create a CSR, and upload the signed certificate. But if for some reason you want to generate the certificate and key outside of iDrac, the gui doesn't offer a way to upload the key.

There is a way to use fully custom certificates, but it requires the ````racadm```` command line utility. Assuming you have generated the certificate, here are the steps:

1. Upload the CA cert:

        racadm -r 1.2.3.4 -i sslcertupload -f c:\path\to\ca.crt -t 2

2. Upload the cert key:

        racadm -r 1.2.3.4 -i sslkeyupload -f c:\path\to\the_cert.key -t 1

3. Upload the cert:

        racadm -r 1.2.3.4 -i sslcertupload -f c:\path\to\the_cert.crt -t 1


If you are using intermediate CAs, ````ca.crt```` should contain all the CA chain, starting with the root one.
