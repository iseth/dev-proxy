# DNSMasq install details
### Ubuntu / Debian (linux)
On ubuntu 20 `lsb_release -r |cut -f 2 |cut -d '.' -f 1`
#### 1. We need to first stop the default dns manager which is usually `systemd-resolved`
```bash
# this needs to be done as sudo because systemd starts and owns the process
sudo service systemd-resolved stop
```
if you cannot figure out what is the current dns manager try looking up what is using port `53` and stop it
```
sudo lsof -i -P -n | grep LISTEN
sudo netstat -tulpn | grep LISTEN
sudo ss -tulpn | grep LISTEN
sudo lsof -i:22 ## see a specific port such as 22 ##
sudo nmap -sTU -O IP-address-Here
```
#### 2. We then need to install dnsmasq
```bash
sudo apt install -y dnsmasq
```

### MacOS (darwin)
#### 1. I believe we can directly install dnsmasq because mac doesnt have a dns manager running defautly
```
brew install dnsmasq
```
#### 2. Check if the brew etc dir exists or if it doesn't make it: `$(brew --prefix)/etc/`

#### 3. We need to setup the *.test tld
```bash
echo 'address=/.test/127.0.0.1' >> $(brew --prefix)/etc/dnsmasq.conf
```
#### 4. Do we need to change port?
```
echo 'port=53' >> $(brew --prefix)/etc/dnsmasq.conf
```
#### 5. Autostart - now and after reboot
```
sudo brew services start dnsmasq
```
#### 6. Add to resolvers and 
```bash
sudo mkdir -p /etc/resolver && sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/test'
```

# DNSMasq config details
#### 1. find `/etc/dnsmasq.conf` and make sure that the line which contains the following is uncommented like the following:
```
# add google dns server so that we still have internet
server=8.8.8.8

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

### Using caddy:
https://medium.com/@devahmedshendy/traditional-setup-run-local-development-over-https-using-caddy-964884e75232
https://github.com/FiloSottile/mkcert
