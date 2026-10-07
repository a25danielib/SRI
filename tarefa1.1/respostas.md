## 1.Saída deste comando dig @localhost xunta.gal

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> xunta.gal
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45049
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;xunta.gal.                     IN      A

;; ANSWER SECTION:
xunta.gal.              28800   IN      A       85.91.64.109

;; Query time: 28 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Tue Oct 06 15:27:39 UTC 2026
;; MSG SIZE  rcvd: 43

## 2.Configura o servidor BIND9 no equipo mandalorian para que empregue como reenviador a darthvader pegando no documento de entrega contido do ficheiro /etc/bind/named.conf.options e a saída deste comando: dig @localhost santiagodecompostela.gal. Contido do ficheiro named.conf.options

options {
    directory "/var/cache/bind";

    forwarders {
        192.168.20.10;
    };

    forward only;

    dnssec-validation auto;

    auth-nxdomain no;
    listen-on-v6 { any; };
};

## comando

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> santiagodecompostela.gal
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 20480
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;santiagodecompostela.gal.      IN      A

;; ANSWER SECTION:
santiagodecompostela.gal. 300   IN      A       195.57.25.148

;; Query time: 56 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Wed Oct 07 15:48:17 UTC 2026
;; MSG SIZE  rcvd: 58

## 3.Instala unha zona primaria de resolución directa chamada "starwars.lan" e engade os seguintes rexistros de recursos (a maiores dos rexistros NS e SOA imprescindibles): Pega no documento de entrega o contido do arquivo de zona, e do arquivo /etc/bind/named.conf.local

zone "starwars.lan" {
    type master;
    file "/var/cache/bind/db.starwars.lan";
};

zone "20.168.192.in-addr.arpa" {
    type master;
    file "/var/cache/bind/db.192.168.20";
};

## 4.Instala unha zona de resolución inversa que teña que ver co enderezo do equipo darthvader, e engade rexistros PTR para os rexistros tipo A do exercicio anterior. Pega no documento de entrega o contido do arquivo de zona, e do arquivo /etc/bind/named.conf.local

zone "starwars.lan" {
    type master;
    file "/var/cache/bind/db.starwars.lan";
};

zone "20.168.192.in-addr.arpa" {
    type master;
    file "/var/cache/bind/db.192.168.20";
};

## 5.Comproba que podes resolver os distintos rexistros de recursos. Pega no documento de entrega a saída dos comandos:

### nslookup darthvader.starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

Name:   darthvader.starwars.lan
Address: 192.168.20.10

### nslookup skywalker.starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

Name:   skywalker.starwars.lan
Address: 192.168.20.111
Name:   skywalker.starwars.lan
Address: 192.168.20.101

### nslookup starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

Name:   starwars.lan
Address: 192.168.20.10

### nslookup -q=mx starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

starwars.lan    mail exchanger = 10 c3p0.starwars.lan.

### nslookup -q=ns starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

starwars.lan    nameserver = darthsidious.starwars.lan.
starwars.lan    nameserver = darthvader.starwars.lan.

### nslookup -q=soa starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

starwars.lan
        origin = darthvader.starwars.lan
        mail addr = admin.starwars.lan
        serial = 2
        refresh = 604800
        retry = 86400
        expire = 2419200
        minimum = 604800

### nslookup -q=txt lenda.starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

lenda.starwars.lan      text = "Que a forza te acompanhe"

### nslookup 192.168.20.11 localhost

Server:         localhost
Address:        127.0.0.1#53

lenda.starwars.lan      text = "Que a forza te acompanhe"

root@darthvader:/var/cache/bind# nslookup 192.168.20.11 localhost
11.20.168.192.in-addr.arpa      name = darthsidious.starwars.lan.


