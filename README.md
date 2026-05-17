# Fedora-configuration
## Install niri
```
sudo dnf copr enable yalter/niri
sudo dnf install niri
```

## Insatll DMS
```
sudo dnf copr enable avengemedia/dms
sudo dnf install dms
```

## Setup DMS
```
dms srtup
```

## Allow DMS start with niri
```
systemctl --user add-wants niri.service dms
```

## Disable waybar
```
killall waybar
systemctl --user disable --now waybar.service
```

## Fixing video cant ply issue
```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```
## Install cloudflare-warp
```
sudo rpm -e 'gpg-pubkey(4fa1c3ba-61abda35)' && sudo rpm --import https://pkg.cloudflareclient.com/pubkey.gpg
sudo tee /etc/yum.repos.d/cloudflare-warp.repo <<EOF
[cloudflare-warp]
name=Cloudflare WARP
baseurl=https://pkg.cloudflareclient.com/rpm
enabled=1
gpgcheck=1
gpgkey=https://pkg.cloudflareclient.com/pubkey.gpg
EOF
sudo dnf update
```
```
sudo rpm -ivh --nodeps cloudflare-warp-*.rpm
rm cloudflare-warp-*.rpm
dnf download cloudflare-warp.x86_64
sudo rpm -ivh --nodeps cloudflare-warp-*.rpm
rm cloudflare-warp-*.rpm
sudo systemctl enable --now warp-svc.service
warp-cli registration new

warp-cli connect
warp-cli mode warp
```

## Changing default shell to fish
```
sudo usermod --shell /usr/bin/fish $USER
```

## Install autopsy
```
# 1. Install the snap daemon package
sudo dnf install snapd

# 2. Enable classic snap support (required by Fedora)
sudo ln -s /var/lib/snapd/snap /snap

# 3. Install Autopsy from the Snap store
sudo snap install autopsy
```

## Install Burp
```
# 1. Download the official Linux 64-bit installer script
curl -Lo burpsuite_installer.sh "https://portswigger.net/burp/releases/download?product=community&type=Linux"

# 2. Grant the script executable permissions
chmod +x burpsuite_installer.sh

# 3. Run the installation script wizard
./burpsuite_installer.sh

rm burpsuite_installer.sh
```

## Remove Libre Office
```
sudo dnf remove "libreoffice*"
```

## install WPS office
```
# 1. Ensure the Flathub repository is enabled on your system
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

# 2. Install WPS Office from Flathub
flatpak install flathub com.wps.Office
```
