+++
showonlyimage = true
draft = false
image = "/img/sonntag_2-1.jpg"
date = "2013-09-09T18:25:22+05:30"
title = "SSL / TLS"
writer = "ieufi"
categories = [ "SSL"]
weight = 1
+++

Benjamin Sonntag

20 septembre 2013 @ La Cantine / Paris

<!--more-->

* Site : <a href="https://benjamin.sonntag.fr/fr">benjamin.sonntag.fr</a>,
* associé gérant et directeur technique d'<a href="https://www.octopuce.fr">Octopuce</a>.


# Conférence

<video "controls": true, id="ssltls_sonntag_conference" class="video-js vjs-default-skin"controls preload="auto" width="100%" height="100%" 
<source src="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/360p/IEUFI_ssl-tls_Sonntag_360.mp4" type='video/mp4' /> 
<source src="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/360p/IEUFI_ssl-tls_Sonntag_360.webm" type='video/webm' /> 
</video>

# Téléchargements

## Les slides

Les slides peuvent être téléchargées ici : <a href="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/IEUFI_SSL-TLS_Sonntag.pdf">SSL-TLS_Sonntag.pdf</a>.

Le document « <em>Analysis of the HTTPS Certificate Ecosystem</em> » peut être téléchargé ici : <a href="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/docs/Analysis_of_the_HTTPS%C3%A8_certificate_ecosystem.pdf">Analysis of the certificate ecosystem (pdf)</a>.

## Les vidéos

Les vidéos de la conférence sont disponibles en basse et en haute définition, en .mp4 et en .webm :

### Basse définition

* conférence : <a href="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/360p/IEUFI_ssl-tls_Sonntag_360.webm">webm</a> (321M) / <a href="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/360p/IEUFI_ssl-tls_Sonntag_360.mp4">mp4</a> (378M)


### Haute définition

* conférence : <a href="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/720p/IEUFI_ssl-tls_Sonntag_720.webm">webm</a> (1,2G) / <a href="https://data.iletaitunefoisinternet.fr/ssl-tls-sonntag/720p/IEUFI_ssl-tls_Sonntag_720.mp4">mp4</a> (894M)


# Exemples de configurations</h3>

<h4 id="apache">Apache</h4>

```bash
ServerAdmin webmaster@octopuce.fr
DocumentRoot /var/www

SSLCertificateFile        /etc/ssl/private/octopuce.fr.crt
SSLCertificateChainFile   /etc/ssl/private/octopuce.fr.chain
SSLCertificateKeyFile     /etc/ssl/private/octopuce.fr.key

SSLEngine on
SSLProtocol +TLSv1.2 +TLSv1.1 +TLSv1
SSLCompression off
SSLHonorCipherOrder on
SSLCipherSuite ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:+HIGH:+MEDIUM 
Header set Strict-Transport-Security "max-age=2678400"
```

<h4 id="nginx">Nginx</h4>

```bash
server {
  listen 443;
  ssl on;
  ssl_certificate /etc/ssl/private/octopuce.fr.crt+chain;
  ssl_certificate_key /etc/ssl/private/octopuce.fr.key;
  ssl_session_timeout 5m;
  ssl_prefer_server_ciphers on;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:+HIGH:+MEDIUM;

  ssl_dhparam /etc/ssl/private/dh2048.pem;
  add_header Strict-Transport-Security max-age=2678400;
}
```

Et 

```bash
openssl dhparam -out /etc/ssl/private/dh2048.pem -outform PEM -2 2048
```

Cf. <a href="https://nginx.org/en/docs/http/configuring_https_servers.html">nginx.org</a> pour plus de détails

<h4 id="lighttpd">lighttpd</h4>

```bash
$SERVER["socket"] == "0.0.0.0:443" {
  ssl.engine  = "enable"
  ssl.pemfile = "/etc/ssl/private/octopuce.fr.key+crt"
  ssl.ca-file = "/etc/ssl/private/octopuce.fr.chain"

  ssl.cipher-list = "ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:+HIGH:+MEDIUM"
  ssl.honor-cipher-order = "enable"
  setenv.add-response-header  = ( "Strict-Transport-Security" =&gt; "max-age=2678400")
}
```

<h4 id="postfix">Postfix</h4>

```bash
# TLS parameters
smtpd_tls_cert_file = /etc/ssl/private/octopuce.fr.crt+chain
smtpd_tls_dcert_file = $smtpd_tls_cert_file
smtpd_tls_key_file = /etc/ssl/private/octopuce.fr.key
smtpd_tls_dkey_file = $smtpd_tls_key_file
smtpd_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
smtpd_tls_mandatory_ciphers = high
smtpd_tls_mandatory_exclude_ciphers = aNULL, MD5
smtpd_tls_dh1024_param_file = /etc/ssl/private/dh2048.pem

smtpd_use_tls = yes
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtpd_tls_protocols = !SSLv2,!SSLv3
smtpd_tls_received_header = yes

smtp_tls_cert_file = $smtpd_tls_cert_file
smtp_tls_dcert_file = $smtpd_tls_dcert_file
smtp_tls_key_file = $smtpd_tls_key_file
smtp_tls_dkey_file = $smtpd_tls_dkey_file
smtp_tls_CAfile = $smtpd_tls_CAfile
smtp_tls_mandatory_ciphers = $smtpd_tls_mandatory_ciphers
smtp_tls_mandatory_exclude_ciphers = $smtpd_tls_mandatory_exclude_ciphers

smtp_use_tls = yes
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
smtp_tls_protocols = !SSLv2,!SSLv3
smtp_tls_secure_cert_match = nexthop, dot-nexthop
```

Voir <a href="https://www.postfix.org/TLS_README.html">postfix.org</a> pour plus de détails.

<h4 id="proftpd">Proftpd</h4>

```bash
TLSEngine                on
TLSLog                    /var/log/proftpd/tls.log
TLSProtocol             TLSv1
TLSCipherSuite  ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:+HIGH:+MEDIUM
TLSRSACertificateFile     /etc/ssl/private/octopuce.fr.crt+chain
TLSRSACertificateKeyFile     /etc/ssl/private/octopuce.fr.key
TLSRenegotiate           required off
TLSOptions               NoCertRequest EnableDiags NoSessionReuseRequired
TLSVerifyClient          off
TLSRequired                on # facultatif
```

<h4 id="prosody">Prosody</h4>

```bash
ssl = {
    key = "/etc/ssl/private/octopuce.fr.key";
    certificate = "/etc/ssl/private/octopuce.fr.crt+chain";
}

c2s_require_encryption = true
s2s_require_encryption = true
```

* <strong>IMPORTANT</strong> : copier le bloc SSL = { } dans tout VirtualHost. 

Vous pouvez voir les sites suivants plus aller plus loin :
 
* <a href="https://www.jeveuxhttps.fr/Accueil">Jeveuxhttps.fr</a> pour mettre en place SSL/TLS sur différentes plateformes,
* <a href="https://www.libwalk.so/2014/02/14/installer-un-serveur-xmppjabber-avec-prosody.html">comment configurer un serveur jabber</a> avec Prosody sous Debian.

