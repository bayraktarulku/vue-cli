# hello-world

## Project setup
```
apt-get update; apt-get upgrade; apt-get dist-upgrade;
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
sudo apt install -y npm
nvm install node
nvm use node
npm -v
npm install -g @vue/cli
```

### Compiles and hot-reloads for development
```
cd vue-cli
npm install
npm install -g json-server
```

### Starting DB Server
```
json-server --watch db.json
```

### Starting Web Server
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```
