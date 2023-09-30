


## Install Ubuntu

for snap version :

on site `https://snapcraft.io/node` 

to install version 20 :
```
sudo snap install node --channel=20/stable --classic
```

for nmv version :

on site `https://github.com/nvm-sh/nvm` after all close `terminal` !
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash

```
what your version
```
nmv ls 
```

see all version on 
```
nmv ls-remote
```

for install 

```
nvm install v18.18.0

```

switch between system snap or nvm 

```
nvm use system
```




Then install Node.js:
```bash
sudo apt install nodejs
```
Check that the install was successful by querying `node` for its version number:
```
node -v
```
## To install yarn on Ubuntu

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```
```
sudo apt update && sudo apt install yarn
```
Note: Ubuntu 17.04 comes with cmdtest installed by default. If you’re getting errors from installing yarn, you may want to run sudo apt remove cmdtest first. Refer to this for more information.



if not working
```
sudo apt remove cmdtest
sudo apt remove yarn
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn -y
```



## Install for Windows 

Download and install: 

https://nodejs.org/en/download/current

after it see if all good: 

```cmd
note -version 
```

## Install yarn

```
npm install global yarn
```

## create new project with yarn

```
cd desktop && cd yarn create react-app test-project-app 
```

after 
```
cd test-prject-app 
```
and `yarn start`


