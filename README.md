# Facebook Login with OpenID connect

# 0. Create VM for web server
```
# git clone https://github.com/developer-onizuka/OAuth2.0_facebook
# cd OAuth2.0_facebook
# vagrant up --provider=libvirt
# vagrant ssh
```

# 1. Install yarn
```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install -y yarn
yarn --version
```

If you need to update node, then see the URL below: <br>
> https://parashuto.com/rriver/tools/updating-node-js-and-npm
```
# sudo apt install -y npm
# sudo npm install -g n
# n --stable
# sudo n stable
# sudo node -v
```

# 2. Init yarn
```
# mkdir facebook-login
# cd facebook-login/

# yarn init -y
# yarn add static-server --dev
# yarn add next react react-dom --dev
# yarn add local-ssl-proxy --dev
```

# 3. Install mkcert for HTTPS
You should create your webApp with HTTPS because OAuth2.0 enforces HTTPS. <br>
```
sudo apt-get install -y wget libnss3-tools
wget https://github.com/FiloSottile/mkcert/releases/download/v1.4.3/mkcert-v1.4.3-linux-amd64
mv mkcert-v1.4.3-linux-amd64 /usr/bin/mkcert
sudo mv mkcert-v1.4.3-linux-amd64 /usr/bin/mkcert
sudo chmod +x /usr/bin/mkcert
mkcert --version
```

# 4. Create certification
```
# mkcert -install
# mkcert localhost
# ls
localhost-key.pem  localhost.pem  node_modules  package.json  pages  public  yarn.lock
```

# 5. Edit package.json
```
# vi package.json 
{
  "name": "facebook-login",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "devDependencies": {
    "static-server": "^2.2.1"
  },
  "scripts": {
    "dev": "next dev",
    "dev:proxy": "next dev & local-ssl-proxy --key localhost-key.pem --cert localhost.pem --source 3001 --target 3000",
    "start": "node server.js",
    "static": "static-server -i public/index.html -p 3000"
  }
}
```
# 6. Create webApp in [Meta for Developer](https://developers.facebook.com/)
<img src="https://github.com/developer-onizuka/OAuth2.0_facebook/blob/master/facebook-login0.png" width="640"><br>
<img src="https://github.com/developer-onizuka/OAuth2.0_facebook/blob/master/facebook-login4.png" width="640"><br>

# 7. Create index.html
Please refer the url below: <br>
> https://developers.facebook.com/docs/facebook-login/web
```
# mkdir public
# touch public/index.html
# vi public/index.html
```

# 8. Run yarn
```
# mkdir pages
# yarn dev:proxy
yarn run v1.22.19
$ next dev & local-ssl-proxy --key localhost-key.pem --cert localhost.pem --source 3001 --target 3000
Started proxy: https://localhost:3001 → http://localhost:3000
ready - started server on 0.0.0.0:3000, url: http://localhost:3000
event - compiled client and server successfully in 457 ms (159 modules)
wait  - compiling /_error (client and server)...
event - compiled client and server successfully in 104 ms (160 modules)
```

# 8. Go login with facebook account
```
# ssh -X vagrant@192.168.33.111
# google-chrome-stable https://localhost:3001/index.html
```
# (1) Press Login button in the upper left corner, then you will be asked about your authorization request.
<img src="https://github.com/developer-onizuka/OAuth2.0_facebook/blob/master/facebook-login1.png" width="640"><br>

# (2) You can check if your request is granted.
<img src="https://github.com/developer-onizuka/OAuth2.0_facebook/blob/master/facebook-login2.png" width="640"><br>

# (3) If you grant it, you'll see your name as it's registered to your Facebook account. This is the **OpenID Connect**.
<img src="https://github.com/developer-onizuka/OAuth2.0_facebook/blob/master/facebook-login3.png" width="640"><br>
