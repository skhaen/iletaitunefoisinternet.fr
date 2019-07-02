+++
showonlyimage = true
draft = false
image = "/img/sonntag1.jpg"
date = "2014-02-07T18:25:22+05:30"
title = "L'email"
writer = "ieufi"
categories = [ "Email"]
weight = 1
+++

Benjamin Sonntag

7 février 2014 @ Institut Mines-Télécom / Paris
<!--more-->

* Site : <a href="https://benjamin.sonntag.fr/fr">benjamin.sonntag.fr</a>,
* associé gérant et directeur technique d'<a href="https://www.octopuce.fr">Octopuce</a>.

**Note** : Cette conférence est aussi visionnable sur [youtube](https://www.youtube.com/channel/UCAIt2HaVASaV5f8eQSt1HZQ/videos).

# Conférence

<video "controls": true, id="email_sonntag_conference" class="video-js vjs-default-skin" controls width="100%" height="100%" 
<source src="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_conference-360p.mp4" type='video/mp4' /> 
<source src="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_conference-360p.webm" type='video/webm' /> 
</video>

# Questions

<video "controls": true, id="email_sonntag_questions" class="video-js vjs-default-skin" controls width="100%" height="100%" 
<source src="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_questions-360p.mp4" type='video/mp4' /> 
<source src="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_questions-360p.webm" type='video/webm' /> 
</video>

# Téléchargements

## Les slides

Les slides sont disponibles : « <em><a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/IEUFI_sonntag_mail.pdf">L'email, vaste sujet</a></em> » (pdf).

## Les vidéos

Les vidéos de la conférence sont disponibles en basse et en haute définition, en .mp4 et en .webm :

### Basse définition

* conférence : <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_conference-360p.webm">webm</a> (214M) / <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_conference-360p.mp4">mp4</a> (557M)
* questions : <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_questions-360p.webm">webm</a> (45M) / <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/360p/IEUFI_mail_sonntag_questions-360p.mp4">mp4</a> (151M)

### Haute définition

* conférence : <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/720p/IEUFI_mail_sonntag_conference-720p.webm">webm</a> (664M) / <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/720p/IEUFI_mail_sonntag_conference-720p.mp4">mp4</a> (1.3G)
* questions : <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/720p/IEUFI_mail_sonntag_questions-720p.webm">webm</a> (155M) / <a href="https://data.iletaitunefoisinternet.fr/mail_sonntag/720p/IEUFI_mail_sonntag_questions-720p.mp4">mp4</a> (362)


# Exemples de configurations

<h4 id="exempledeconfigurationdnsavecopendkimspfautodiscover">Exemple de configuration DNS avec OpenDKIM, SPF &amp; autodiscover</h4>

```bash
zone sonntag.fr.
@ IN TXT   "v=spf1 a mx a:brassens.heberge.info a:z1.sonntag.fr ?all"
@ IN TXT   "mailconf=https://autodiscover.sonntag.fr/mail/mailautoconfig.xml"
@ IN MX 5  alice.sonntag.fr.
@ IN MX 10 secondary.sonntag.fr.
alternc._domainkey  IN  TXT "v=DKIM1\; k=rsa\; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC7lmzOaaojx4ppHCIoaqX5CP92tVCij36eFh+FscPyoTQ5CimLSFAyDhDEp0hDYA/8EZoqWvF/z3rZKp+JrypKoqpPSI3QpaJGp+ZuqJabcKjE5rgk7bUUfm9gVUnOehIM185n7xpbWkQFxmCufpJu3wu4eqNc2YPJ5A9H9AldyQIDAQAB"   
mail IN CNAME alice
webmail IN CNAME alice
pop IN CNAME alice
imap IN CNAME alice 
smtp IN CNAME alice

alice IN A    91.194.60.6
alice IN AAAA 2001:67c:288::6

zone 60.194.91.in-addr.arpa.
6     IN PTR alice.sonntag.fr.

zone 0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.8.2.0.C.7.6.0.1.0.0.2.ip6.arpa.
6.0.0.0   IN PTR alice.sonntag.fr.
```

<h4 id="exempledextraitdeconfigurationpourpostfixetcpostfixmaincf">Exemple d'extrait de configuration pour Postfix (/etc/postfix/main.cf)</h4>

```bash
# TLS parameters
smtpd_tls_cert_file = /etc/ssl/certs/alice.crt+chain
smtpd_tls_key_file = /etc/ssl/private/alice.key
smtpd_use_tls = yes  # idem avec smtp

# Generic settings
myhostname = alice.sonntag.fr
alias_maps = hash:/etc/aliases
mydestination = alice.sonntag.Fr, localhost.sonntag.fr, localhost
# relayhost = 10.2.0.1
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128
recipient_delimiter = +
inet_interfaces = all

# Filtres DKIM
milter_default_action = accept
milter_protocol = 6
smtpd_milters = inet:127.0.0.1:8891
non_smtpd_milters = inet:127.0.0.1:8891

# Filtres maison
header_checks = regexp:/etc/postfix/header_checks
body_checks = regexp:/etc/postfix/body_checks
message_size_limit = 502400000
mailbox_size_limit = 1024000000

# Secondary MX configuration
relay_domains = $mydestination hash:/etc/postfix/secondary
disable_vrfy_command = yes
smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_invalid_hostname, 
  reject_non_fqdn_hostname, reject_non_fqdn_sender, reject_rbl_client zen.spamhaus.org,
  reject_non_fqdn_recipient, reject_unknown_sender_domain, reject_unknown_recipient_domain, 
  reject_unauth_pipelining, reject_unlisted_recipient, reject_unauth_destination
```

<h4 id="exempledextraitdeconfigurationpourpostfixetcpostfixmastercf">Exemple d'extrait de configuration pour Postfix (/etc/postfix/master.cf)</h4>

```bash
smtp      inet  n       -       y       -       20       smtpd 
  -o content_filter=smtp-amavis:[127.0.0.1]:10024

smtps      inet  n       -       y       -       20       smtpd 
  -o smtpd_tls_wrappermode=yes -o smtpd_sasl_auth_enable=yes 
  -o smtpd_client_restrictions=permit_mynetworks,permit_sasl_authenticated,reject

submission      inet  n       -       y       -       20       smtpd 
  -o header_checks=regexp:/etc/postfix/receivedanonymous 
  -o smtpd_recipient_restrictions=permit_mynetworks,permit_sasl_authenticated,reject
```

/etc/postfix/receivedanonymous

```bash
/^received: /    IGNORE
/^X-Sender: /    IGNORE
/^User-agent: /    IGNORE
```