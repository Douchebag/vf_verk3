## Verkefni 3

[Tutorial](https://learn.adafruit.com/matrix-led-sand/overview) sem var fylgt.

### Operating System

Náðum í Raspbian [hér](https://www.raspberrypi.org/downloads) og notuðum [balenaEtcher](https://www.balena.io/etcher/) til að skrifa á SD kortið. Ekki hægt að gera í skólatölvunum. Þarf að skrifa .iso ekki virkar að nota .zip þótt þeir segja að það virkar.

### SSH Uppsetning

[Þessar leiðbeiningar](https://github.com/reyniraron/vf-verkefni-3/blob/master/SSH-uppsetning.md) miða við að notast sé við Linux eða annað stýrikerfi sem getur lesið og skrifað í Linux partitions (`ext4`). Þessar breytingar fara fram á SD-kortinu sem notað er með Raspberry Pi-tölvunni, eftir að stýrikerfið hefur verið brennt á það. Öll skref eiga við um `rootfs` partition-ið nema annað sé tekið fram.

SSH

Búa til tóma skrá í rót á `boot` partition sem heitir `ssh` (`touch ssh`).

Hostname

Breyta innihaldi `/etc/hostname` (á `rootfs`) í það hostname sem er valið.

Opna `/etc/hosts` og breyta línunni `127.0.0.1 raspberrypi` í `127.0.0.1 [valið hostname]`.

Wi-Fi

Opna `/etc/wpa_supplicant/wpa_supplicant.conf` og bæta eftirfarandi línum við:

```conf
country=IS

network={
    ssid="Taekniskolinn"
    key_mgmt=NONE
}
```

Þegar þessu er lokið er hægt að setja SD-kortið í Raspberry Pi, tengja það við rafmagn og tengjast því í gegn um SSH með hostname-inu sem var valið og notandanum `pi`. Sjálfgefið lykilorð er `raspberry` en ráðlagt er að breyta því.

`ssh pi@hostname`

### Installa Matrix Driver

Eftir að SSH-a inn á raspberry-inn þarf að installa matrix driver-inum. Tekur circa 15 mín.

```conf
curl https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/rgb-matrix.sh >rgb-matrix.sh
sudo bash rgb-matrix.sh
```

### Installa Git og clone

```conf
sudo apt-get install -y git
git clone https://github.com/adafruit/Adafruit_PixelDust.git
```

### Enable-a I2C via raspi-config

Fórum eftir þessum [tutorial](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c) 

```conf
sudo apt-get install -y python-smbus
sudo apt-get install -y i2c-tools
sudo raspi-config
```

Ferð í **Interfacing Options** svo í **I2C** og ýtir á **Yes** tvisvar. 
```conf
sudo reboot
```

### Make PixelDust

```conf
cd Adafruit_PixelDust/raspberry_pi
make
```

### Running the code

Til að keyra kóðann:

```conf
cd Adafruit_PixelDust/raspberry_pi/
sudo python buttons.py
```
Ctrl+C til að stoppa skriptu

### Brightness og RGB

Lækkum birtuna og breyta rgb sequence, villa á Adafruit rbg=rgb

```conf
nano buttons.py
```

Lína 15 ætti að líta svona út eftir breytingar.

```conf
FLAGS        = ["--led-rgb-sequence=rgb", "--led-brightness=33"]
```

### Automatic Startup

```conf
sudo nano /etc/rc.local
```

Rétt fyrir exit 0 línuna bættum við þessum tveimur línum.

```conf
cd /home/pi/Adafruit_PixelDust/raspberry_pi
python buttons.py &
```

Svo save-uðum við og reboot-uðum.

### Samsetning og lóðun

Lóðun gekk vel þangað til það þurfti að lóða headers á Zero-inn. Útaf hversu slæm lóðunin var hjá öllum nema einum hóp þurftu allir að skipta í Raspberry pi 3.

<img src="https://github.com/Douchebag/vf_verk3/blob/master/myndir/IMG_2222.JPG?raw=true"  width="250" height="250">
<img src="https://github.com/Douchebag/vf_verk3/blob/master/myndir/IMG_2242.JPG?raw=true"  width="250" height="250">
<img src="https://github.com/Douchebag/vf_verk3/blob/master/myndir/IMG_2241.JPG?raw=true"  width="250" height="250">

