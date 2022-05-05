# tabby-arm-builds
https://github.com/Eugeny/tabby

Finished builds for the latest Tabby version are on the [Releases](https://github.com/Jai-JAP/tabby-arm-builds/releases) page.

## To compile Tabby on arm linux:
```
sudo apt install -y libsecret-1-dev libfontconfig1-dev libarchive-tools jq
#Install nodejs 16
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -

git clone https://github.com/eugeny/tabby -b $(curl --silent "https://api.github.com/repos/$1/releases/latest" | jq -r '.tag_name') --single-branch
cd tabby

# for armhf downgrade electron to 17.0.0 in package.json
sed -i '/\"electron\":/c\    \"electron\" : \"17.0.0\",' package.json

sudo npm i -g yarn
sed -i "s/['deb', 'tar.gz', 'rpm', 'pacman']/['deb']/" scripts/build-linux.js
cd app && yarn --network-timeout 1000000
rm node_modules/.yarn-integrity
rm -rf node_modules/cpu-features
rm -rf node_modules/ssh2/crypto/build  
cd .. && yarn --network-timeout 1000000
scripts/build-native.js
yarn run build
scripts/prepackage-plugins.js
scripts/build-linux.js

#Build artifacts will be in dist subfolder
```
