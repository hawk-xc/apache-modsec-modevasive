# apache-modsec
Apache2 mod security and mod evasive installation, configuration, and implementation

```bash
sudo su
```

```bash
apt-get install apache2 apache2-utils libapache2-mod-security2
```

check module

```bash
a2enmod security2
```

restart service

```bash
systemctl restart apache2
```

config-sec.conf

```bash
/etc/apache2/mods-enabled/security2.conf
```

```bash
vi /etc/modsecurity/modsecurity.conf
```

```
SecRuleEngine On
SecAuditLogParts ABCEFGHJKZ
```

```bash
systemctl restart apache2
```

make modsec directory for modsec rule

```bash
mkdir /etc/apache2/modsecurity-crs
cd /etc/apache2/modsecurity-crs
```

clone OWASP modsec rule

```bash
git clone https://github.com/coreruleset/coreruleset.git
```

change name

```bash
cd coreruleset
mv crs-setup.conf.example crs-setup.conf
```

edit rule run

```bash
vi /etc/apache2/mods-enabled/security2.conf
```

```
IncludeOptional /etc/apache2/modsecurity-crs/coreruleset/crs-setup.conf
IncludeOptional /etc/apache2/modsecurity-crs/coreruleset/rules/*.conf
```

restart service

```bash
apache2ctl -t
systemctl restart apache2
```
# apache-modevasive Debian family

```bash
apt-get install libapache2-mod-evasive
```

config mod evasive config file

```bash
vi /etc/apache/mods-enabled/evasive.conf
```
```
<IfModule mod_evasive20.c>
    DOSHashTableSize    3097
    DOSPageCount        2
    DOSSiteCount        50
    DOSPageInterval     1
    DOSSiteInterval     1
    DOSBlockingPeriod   10

    DOSEmailNotify      yourmail@mail.com
    DOSSystemCommand    "su - someuser -c '/sbin/... %s ...'"
    DOSLogDir           "/var/log/mod_evasive"
</IfModule>
```

buat direktory dan perizinannya

```bash
mkdir /var/log/mod_evasive && chown -R www-data:www-data /var/log/mod_evasive
```

restart service

```bash
systemctl restart apache2
```

# httpd-modevasive red-hat family

```bash
yum install httpd-devel -y
yum groupinstall 'Development tools'
```

Downloads and compile modevasive

```bash
cd /opt
wget https://codeload.github.com/shivaas/mod_evasive/zip/master
unzip master
```

```bash
cd mod_evasive-master
apxs -i -c -a mod_evasive24.o 
```

restart httpd service

```bash
systemctl restart httpd
systemctl status httpd -l
```

# Test configuration

### modsec test
akses server ip dengan sql i

_https://_address_/?id=3 or 'a'='a'

### modevasive test
```bash
apt-get install perl -y
```

```
vi /usr/share/doc/libapache2-mod-evasive/examples/test.pl
```

```
#!/usr/bin/perl

# test.pl: small script to test mod_dosevasive's effectiveness

use IO::Socket;
use strict;

for(0..100) {
  my($response);
  my($SOCKET) = new IO::Socket::INET( Proto   => "tcp",
                                      PeerAddr=> "127.0.0.1:80");
  if (! defined $SOCKET) { die $!; }
#  print $SOCKET "GET /?$_ HTTP/1.0\n\n";
  print $SOCKET "GET /?$_ HTTP/1.0\r\nHost: 127.0.0.1\r\n\r\n\r\n";
  $response = <$SOCKET>;
  print $response;
  close($SOCKET);
}
```
```bash
perl text.pl
HTTP/1.1 200 OK
HTTP/1.1 200 OK
HTTP/1.1 403 Forbidden
HTTP/1.1 200 OK
HTTP/1.1 403 Forbidden
HTTP/1.1 200 OK
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
```
