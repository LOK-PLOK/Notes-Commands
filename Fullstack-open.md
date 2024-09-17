## Creating a REACT project
navitgate to your desired directory
```bash
# creates the react project
npm create vite@latest <filename> -- --template react

# change directory to the project folder
cd <filename>

# install node modules folder
npm install
```
running the project
```bash
# type this command to the terminal inside project folder
npm run dev
```
for JSON server
```bash
# after creating your .json file, execute this command from the root directory of the project
npx json-server --port 3001 --watch db.json

# after running it you can go to the browser and open the server
 http://localhost:3001/<data_name>
```
installing axios
```bash
# execute this command from the root directory of the project
npm install axios
```
installing json-servers as a development dependency
```bash
# only used during development
npm install json-server --save-dev
```
running the server
```bash
# execute command
npm run server
```
>[!important]
>running the react project and the server should be on different terminals
