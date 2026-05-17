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

flatpak run com.wps.Office
```

## Try linux 7.1
```
# 1. Enable the mainline vanilla kernel repository
sudo dnf copr enable @kernel-vanilla/mainline

# 2. Upgrade your kernel packages from the new repo
sudo dnf upgrade 'kernel*'
```

## rEFInd
```
sudo dnf install rEFInd
git clone https://github.com/gutlessCGH/RONBM.git
sudo refind-install
sudo mkrlconf
sudo mkdir -p /boot/efi/EFI/refind/themes
sudo cp -r ./RONBM /boot/efi/EFI/refind/themes/RONBM
sudo nano /boot/efi/EFI/refind/refind.conf
```

## WinApps
### Install KVM
```
sudo dnf install -y qemu-kvm libvirt virt-manager virt-viewer \

dnsmasq bridge-utils netcat libguestfs-tools \
swtpm dialog freerdp

sudo usermod -aG libvirt $(whoami)
sudo systemctl enable --now libvirtd
mkdir -p ~/.config/libvirt
echo 'uri_default = "qemu:///system"' > ~/.config/libvirt/libvirt.conf
sudo usermod -a -G libvirt <username>
sudo usermod -a -G kvm <username>
```
### Get Win 11 Pro (if upgrade fail using free key)
#### key: 
```
VK7JG-NPHTM-C97JM-9MPGT-3V66T
```
#### If Fail:
Disconnect internet connection then:
```
mkdir ~/win11_patch && cd ~/win11_patch
nano ei.cfg
```
```
[EditionID]
Professional
[Channel]
Retail
```
```
dd if=/dev/zero of=patch.img bs=1M count=10
mkfs.vfat patch.img
sudo mkdir /mnt/patch
sudo mount patch.img /mnt/patch
sudo mkdir /mnt/patch/sources
sudo cp ei.cfg /mnt/patch/sources/
sudo umount /mnt/patch
```
### In windows set allow list
```
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Terminal Server\TSAppAllowList" /v fDisabledAllowList /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Terminal Server\TSAppAllowList" /v fApplicationsSpecified /t REG_DWORD /d 0 /f
```

### Setup windows login info
```
mkdir -p ~/.config/winapps
nano ~/.config/winapps/winapps.conf
```
```
RDP_USER="<username>"
RDP_PASS="<password>"
WAFLAVOR="libvirt"
GUEST_NAME="win11"
LIBVIRT_URI="qemu:///system"
```
```
rm -rf ~/.local/share/winapps
git clone https://github.com/winapps-org/winapps.git ~/.local/share/winapps
cd ~/.local/share/winapps
./setup.sh --user
```
### Brightness
#### Linux 6.19
```
sudo nano /boot/refind_linux.conf
```
```
Boot with standard options"  "root=UUID=c6e94e97-8b6e-4c08-86a2-47d70e9539e2 rw rootflags=subvol=root zswap.enabled=0 rootfstype=btrfs loglevel=3 quiet nvidia-drm.modeset=1 ipv6.disable=1 acpi_backlight=native"
"Boot to single-user mode"    "root=UUID=c6e94e97-8b6e-4c08-86a2-47d70e9539e2 rw rootflags=subvol=root zswap.enabled=0 rootfstype=btrfs loglevel=3 quiet nvidia-drm.modeset=1 single ipv6.disable=1 acpi_backlight=native"
```

#### Linux 7.0 and above alt way
##### Install
```
sudo dnf install gammastep
```

##### Key Bind
```
// === Brightness Controls ===
XF86MonBrightnessUp allow-when-locked=true {
    spawn "sh" "-c" "BR=$(cat /tmp/b 2>/dev/null || echo 1.0); BR=$(echo \"$BR + 0.1\" | bc); if [ $(echo \"$BR > 1.0\" | bc) -eq 1 ]; then BR=1.0; fi; echo $BR > /tmp/b; pkill gammastep; gammastep -O 6500 -b $BR"
}
XF86MonBrightnessDown allow-when-locked=true {
    spawn "sh" "-c" "BR=$(cat /tmp/b 2>/dev/null || echo 1.0); BR=$(echo \"$BR - 0.1\" | bc); if [ $(echo \"$BR < 0.2\" | bc) -eq 1 ]; then BR=0.2; fi; echo $BR > /tmp/b; pkill gammastep; gammastep -O 6500 -b $BR"
}
```
