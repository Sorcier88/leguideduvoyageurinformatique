---
title: "Télécharger Stremio sur Raspberry Pi OS"
---
Voilà de manière assez brut les lignes de commande pour installer Stremio sur Raspberry Pi OS

# Initialisation

```
sudo apt update
```

```
sudo apt upgrade
```

# Première installation

```
sudo apt install snapd
```

# Redémarrage

```
sudo reboot
```

# On continue l’installation

```
sudo snap install core
```

```
sudo snap install cmake --classic
```

```
sudo apt-get install libmpv-dev
```

```
sudo apt-get install libqt5webview5-dev
```

```
sudo apt-get install libkf5webengineviewer-dev
```

```
sudo apt-get install librsvg2-bin
```

```
sudo snap install node --classic
```

```
sudo apt-get install qml-module-qtwebchannel qml-module-qt-labs-platform qml-module-qtwebengine qml-module-qtquick-dialogs qml-module-qtquick-controls qtdeclarative5-dev qml-module-qt-labs-settings qml-module-qt-labs-folderlistmodel
```

# On redémarre encore

```
sudo reboot
```

# On continue l’instalation

```
git clone --recurse-submodules -j8 git://github.com/Stremio/stremio-shell.git
```

```
sudo apt-get install qtcreator qt5-qmake qt5-qmake g++ pkgconf libssl-dev
```

```
cd stremio-shell
```

```
qmake
```

```
make -f release.makefile
```

```
cp ./server.js ./build/ && ln -s "$(which node)" ./build/node
```

# On lance l’app enfin

```
./build/stremio
```

---

Si vous avez toujours des questions n’hésitez pas à m’envoyer un mail à [guidevoyageurinformatique@sorcier.mozmail.com](mailto:guidevoyageurinformatique@sorcier.mozmail.com)