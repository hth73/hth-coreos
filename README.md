# CoreOS Test Environment

<img src="https://img.shields.io/badge/VirtualBox-2F61B4?style=flat&logo=virtualbox&labelColor=ffffff&logoColor=2F61B4" /> <img src="https://img.shields.io/badge/Fedora%20CoreOS-51A2DA?style=flat&logo=fedora&labelColor=ffffff&logoColor=5277C3" /> <img src="https://img.shields.io/badge/Butane-51A2DA?style=flat&logo=fedora&labelColor=ffffff&logoColor=5277C3" />

---

## Beschreibung

Fedora CoreOS wird in diesem Repository vollständig automatisiert bereitgestellt. Die Grundkonfiguration wird deklarativ mit Butane beschrieben und in eine Ignition-Datei übersetzt. Diese wird während der Installation von Fedora CoreOS über `coreos-installer` eingebunden. Auf dieser Basis erstellen Packer, Ansible und Vagrant eine reproduzierbare `coreos-base.box`, die Docker CE, Neovim sowie weitere Standardwerkzeuge bereits vorkonfiguriert enthält.

## Vorbereitung für erste CoreOS Installation
```bash
## CoreOS Butane lokal installieren
##
cd /tmp
wget https://github.com/coreos/butane/releases/download/v0.27.0/butane-x86_64-unknown-linux-gnu

mv butane-x86_64-unknown-linux-gnu ~/bin/butane
chmod +x ~/bin/butane

butane --version
# Butane 0.27.0

## Butane Konfiguration vorbereiten
##
vi ./config.bu

# --- config.bu
# Fedora CoreOS Butane configuration
# Date: 2026-05-16
variant: fcos
version: 1.6.0

passwd:
  users:
    # Default User core
    - name: core
    # ...

## ./config.bu in Ignition-Datei konvertieren und in Git Repo hochladen
butane --pretty --strict config.bu > config.ign
```

## Erste Installation durchführen

```bash
## VirtualBox VM anlegen und mit Fedora CoreOS Live DVD starten
## CoreOS Installation in der VM starten
sudo loadkeys de

sudo coreos-installer install /dev/sda --ignition-url https://raw.githubusercontent.com/hth73/hth-coreos/refs/heads/main/config.ign

# oder

curl -LO https://raw.githubusercontent.com/hth73/hth-coreos/refs/heads/main/config.ign
sudo coreos-installer install /dev/sda --ignition-file config.ign
```
