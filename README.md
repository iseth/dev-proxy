# DNSMasq details
#### 1. find `/etc/dnsmasq.conf` and make sure that the line which contains the following is uncommented like the following:
```
# Include all files in a directory which end in .conf
conf-dir=/usr/local/etc/dnsmasq.d/,*.conf
```

the above code will load all config files in `/usr/local/etc/dnsmasq.d/*.conf`

#### 2. make a new file `/usr/local/etc/dnsmasq.d/dnsmasq-new-config.conf` with the following contents:
```
# Valet
# Include all *.conf files in user's valet directory
conf-dir=/Users/sendhaggling/.config/valet/dnsmasq.d/,*.conf
```

#### 2. make a new file `/Users/sendhaggling/.config/valet/dnsmasq.d/tld-testss.conf` with the following contents:
```
address=/.testss/127.0.0.1
listen-address=127.0.0.1
```

