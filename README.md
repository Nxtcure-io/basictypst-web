# Simple Typst Web Framework

This will render the typst in index.typ

For more info:
https://github.com/Myriad-Dreamin/typst.ts/tree/main/projects/vite-plugin-typst

Prereqs:

```bash
#install yarn
npm install -g corepack

#use yarn to add plugins
yarn add -D vite

#to start a new project
#the . here uses current directory eg $PROJECT_DIR
#yarn create vite . --template react 
yarn add -D @myriaddreamin/vite-plugin-typst
yarn add -D @myriaddreamin/typst-ts-node-compiler
```

To run the website on localhost:
```bash
npm run dev
```
Notes
* dev is set in package.json to use --host flag
	* --host flag makes machines beyond localhost work
	* e.g http://0.0.0.0:5173 becomes valid
	* exposing vite like this an attack surface and is vulnerable
	* this is not suitable for production(cf nginx, apache) 

In order to skip tls/ssl setup for https, consider tunnelmole
```bash
npm install -g tunnelmole
tmole 5173
```

Ideally maybe do something like this
```bash
LOG_DIR=$HOME/.logs/screen
mkdir -p $LOG_DIR
LOG_FILE=simpletypst-web.log
rm "$LOG_DIR/$LOG_FILE" #assuming you are running again
WEB_PORT=5173
TUNNEL_NAME=tunnel
screen -L -Logfile "$LOG_DIR/$LOG_FILE" -dmS $TUNNEL_NAME tmole $WEB_PORT
sleep 10s #let the tunnel load up
grep "https://" "$LOG_DIR/$LOG_FILE" | grep "localhost" | awk '{print $1}'
```
Notes
* What this does is run the tunnel and then return the https address for the service.
* The vite config has been set to accept .tunnelmole.net as a host
