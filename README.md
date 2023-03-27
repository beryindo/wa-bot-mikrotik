***wa-bot-mikrotik***

DEBIAN 11
=
***cek posisi direktori, ini akan menentukan folder service***
```
pwd
```
```
apt update && apt upgrade -y
```
```
apt install curl sudo git -y
```
```
curl -sL https://deb.nodesource.com/setup_17.x -o /tmp/nodesource_setup.sh
```
```
sudo bash /tmp/nodesource_setup.sh
```
```
sudo apt install nodejs -y
```
```
sudo apt install libnss3-dev libatk-bridge2.0-0 libxkbcommon-x11-0 libgtk-3-0 libgbm-dev -y
```
```
sudo apt install chromium -y
```
```
sudo git clone https://github.com/ngekoding/whatsapp-api-tutorial
```
```
mv whatsapp-api-tutorial whatsapp
```
```
cd whatsapp
```


***RUBAH PORT (opsional)***

```
sed -i 's/const port = process.env.PORT || 8000;/const port = process.env.PORT || 8069;/g' app.js
```
```
npm install
```
```
wget https://raw.githubusercontent.com/beryindo/wa-bot-mikrotik/main/script.sh
```
```
chmod +x script.sh
```
```
wget https://raw.githubusercontent.com/beryindo/wa-bot-mikrotik/main/wa.service -O /etc/systemd/system/wa.service
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl enable wa
```
```
sudo systemctl restart wa
```

***Buka pake browser, dan scan bardcode WA nya***
```
http://ipaddress:8069
```

***Tambahkan Script di Profile PPPOE***
***rubah nomor hp penerima WA dan ipaddress***

***On UP***

```
:local nama "$user";
:local ips [/ppp active get [find name=$nama] address];
:local up [/ppp active get [find name=$nama] uptime];
:local comment [/ppp secret get [find name=$nama] comment];
:local caller [/ppp active get [find name=$nama] caller-id];
:local service [/ppp active get [find name=$nama] service];
:local active [/ppp active print count];
:local datetime "Tanggal: $[/system clock get date] %0AJam: $[/system clock get time]";
:local lastdisc [/ppp secret get [find name=$user] last-disconnect-reason];
:local lastlogout [/ppp secret get [find name=$user] last-logged-out];
:local lastcall [/ppp secret get [find name=$user] last-caller-id];
/tool fetch http-header-field="content-type: application/x-www-form-urlencoded" http-method=post http-data="number=08123456789&message=\E2\9C\85 PPPoE TERHUBUNG%0A$datetime%0AUser: $user%0ANama Pelanggan: $comment%0AIP Client: $ips%0ACaller ID: $caller%0AUptime: $up%0ATotal Active: $active Client%0AService: $service%0ALast Disconnect Reason: $lastdisc %0ALast Logout: $lastlogout %0ALast Caller ID: $lastcall" url="http://ipaddress:8069/send-message" keep-result=no
```


***On DOWN***
```
:local nama "$user";
:local service [/ppp secret get [find name=$nama] service];
:local comment [/ppp secret get [find name=$nama] comment];
:local local [/ppp secret get [find name=$nama] local];
:local remote [/ppp secret get [find name=$nama] remote];
:local profile [/ppp secret get [find name=$nama] profile];
:local last [/ppp secret get [find name=$nama] last-logged-out];
:local lastcall [/ppp secret get [find name=$nama] last-caller-id];
:local lastdic [/ppp secret get [find name=$nama] last-disconnect-reason];
/tool fetch http-header-field="content-type: application/x-www-form-urlencoded" http-method=post http-data="number=08123456789&message=\E2\9D\8C PPPoE TERPUTUS !!!%0AUser: $user%0ANama Pelanggan: $comment%0AService: $service %0ALocal Address: $local%0ARemote Address: $remote%0AProfile: $profile%0ALast Logout: $last%0ALast Caller ID: $lastcall %0ALast Disconnect Reason: $lastdic" url="http://ipaddress:8069/send-message" keep-result=no
```
