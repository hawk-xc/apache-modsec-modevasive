# apache-modsec-modevasive
Apache2 mod security and mod evasive installation, configuration, and implementation

```bash
sudo su
```

```bash
apt-get install libapache2-mod-security2
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

# Test configuration

akses server ip dengan sql i

_https://_address_/?id=3 or 'a'='a'
