# tabby-arm-builds
https://github.com/Eugeny/tabby

# ***NOTE: [DEPRECATED]***  
- See https://github.com/Eugeny/tabby/pull/6612
- Use builds on Upstream Tabby Releases [page](https://github.com/Eugeny/tabby/releases/tag/latest).  
- This repo will be up for historic purposes.

#  
Finished builds are available on the [Releases](https://github.com/Jai-JAP/tabby-arm-builds/releases) page.

Latest Tabby releases are automatically compiled for armv7l/arm64 using Github workflows.

## To compile Tabby on arm linux:
```
sudo apt install -y libsecret-1-dev libfontconfig1-dev libarchive-tools jq ruby build-essentials git curl
#Install nodejs 16
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install -y nodejs
sudo gem install fpm --no-document

git clone https://github.com/eugeny/tabby -b $(curl --silent "https://api.github.com/repos/eugeny/tabby/releases/latest" | jq -r '.tag_name') --single-branch
cd tabby

# for armhf downgrade electron to 17.0.0 in package.json
sed -i '/"electron":/c\    "electron" : "17.0.0",' package.json

sudo npm i -g yarn
cd app && yarn --network-timeout 1000000
rm node_modules/.yarn-integrity
rm -rf node_modules/cpu-features
rm -rf node_modules/ssh2/crypto/build  
cd .. && yarn --network-timeout 1000000
scripts/build-native.js
yarn run build
scripts/prepackage-plugins.js
USE_SYSTEM_FPM=true scripts/build-linux.js

#Build artifacts will be in dist subfolder
```
