+++
showonlyimage = true
draft = false
image = "/img/bortzmeyer.jpg"
date = "2013-09-09T18:25:22+05:30"
title = " DNS"
writer = "ieufi"
categories = [ "DNS"]
weight = 1
+++

Stéphane Bortzmeyer

9 septembre 2013 @ La Cantine / Paris

<!--more-->

* Ingénieur R&D
* site : <a href="https://www.bortzmeyer.org/">bortzmeyer.org</a>

**Note** : Cette conférence est aussi visionnable sur [youtube](https://www.youtube.com/channel/UCAIt2HaVASaV5f8eQSt1HZQ/videos).

# Conférence


<video "controls": true, id="bortzmeyer_conference" class="video-js vjs-default-skin" controls width="100%" height="100%" 
<source src="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_conference_360p.mp4" type='video/mp4' /> 
<source src="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_conference_360p.webm" type='video/webm' /> 
</video>

# partie technique

<video "controls": true, id="bortzmeyer_technique" class="video-js vjs-default-skin" controls width="100%" height="100%" 
<source src="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_tech_360p.mp4" type='video/mp4' /> 
<source src="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_tech_360p.webm" type='video/webm' /> 
</video>

# les questions

<video "controls": true, id="bortzmeyer_questions" class="video-js vjs-default-skin" controls width="100%" height="100%" 
<source src="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_questions_360p.mp4" type='video/mp4' /> 
<source src="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_questions_360p.webm" type='video/webm' /> 
</video>

# Téléchargements

## Les slides

Les slides sont <a href="https://www.bortzmeyer.org/static/DNS@Cantine/#/">visionnables en ligne</a> sur le site de Stéphane Bortzmeyer, il existe aussi une <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/dns_bortzmeyer_slides.pdf">version pdf</a>.

## Les vidéos

Les vidéos de la conférence sont disponibles en basse et en haute définition, en .mp4 et en .webm :


### Basse définition

* conférence : <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/webm/IEUFI_DNS_bortzmeyer_conference_360p.webm">webm</a> (82M) / <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_conference_360p.mp4">mp4</a> (183M)
* questions : <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/webm/IEUFI_DNS_bortzmeyer_questions_360p.webm">webm</a> (95M) / <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/360p/mp4/IEUFI_DNS_bortzmeyer_questions_360p.mp4">mp4</a> (184M)

### Haute définition

* conférence : <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/720p/webm/IEUFI_DNS_bortzmeyer_conference_720p.webm">webm</a> (243M) / <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/720p/mp4/IEUFI_DNS_bortzmeyer_conference_720p.mp4">mp4</a> (694M)
* questions : <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/720p/webm/IEUFI_DNS_bortzmeyer_questions_720p.webm">webm</a> (360M) / <a href="https://data.iletaitunefoisinternet.fr/dns-bortzmeyer/720p/mp4/IEUFI_DNS_bortzmeyer_questions_720p.mp4">mp4</a> (410M)


# Configurations

<h4 id="bindserveurfaisantautorit">Bind - serveur faisant autorité</h4>

```bash
# Le plus simple serveur faisant autorité 
# pour le TLD "example"

options {
        recursion no;
};
zone "example" {
        type master;
        file "example";
};

```

<h4 id="bindrsolveur">Bind - résolveur</h4>

```bash
# Le plus simple résolveur
acl me {
                2001:db8:43::/48;
};
options {
        recursion yes;
        allow-recursion   { mynetwork; };
        allow-query-cache { mynetwork; };
        allow-query { mynetwork; };
};
```

<h4 id="binddonnes">Bind - données</h4>

```bash
@       IN      SOA     ns1.nic root@nic (
                2013071800              ; Serial
                7200            ; Refresh
                1800            ; Retry
                2419200         ; Expire
                600 )   ; Negative Cache TTL

@       IN      NS      ns1.nic.example.
        IN      NS      ns1.pch.net.

www     IN    AAAA   2001:db8::bad:dcaf
```

<h4 id="nsd">NSD</h4>

```bash
zone:         
        name: "example"
        zonefile: "example"
```

<h4 id="unbound">Unbound</h4>

```bash
server:
  interface: ::0
  interface: 0.0.0.0
  access-control: 2001:db8:43::/48 allow
```