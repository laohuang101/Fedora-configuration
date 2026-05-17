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
