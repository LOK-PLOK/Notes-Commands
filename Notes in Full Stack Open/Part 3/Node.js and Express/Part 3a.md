### Creating the backend steps

```bash
npm init
```

#### Follow this:

```JSON
{
  "name": "backend",
  "version": "0.0.1",
  "description": "",
  "main": "index.js",
  "scripts": {
	"start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Matti Luukkainen",
  "license": "MIT"
}
```

#### Running the script:
```bash
node index.js
	or
npm start
```

#### express:
```bash
npm install express
```

#### Nodemon:
```bash
npm instasll --save-dev nodemon
```

#### Package.json file change:
```bash
  // ..
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",   
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // ..
```

#### running the file with nodemon:
```bash
npm run dev
```

#### HTTP verbs:
![[Pasted image 20240920213016.png]]

### Full code:

```javascript
const express = require("express");
const app = express();

let persons = [
  {
    id: "1",
    name: "Arto Hellas",
    number: "040-123456",
  },
  {
    id: "2",
    name: "Ada Lovelace",
    number: "39-44-5323523",
  },
  {
    id: "3",
    name: "Dan Abramov",
    number: "12-43-234345",
  },
  {
    id: "4",
    name: "Mary Poppendieck",
    number: "39-23-6423122",
  },
];

app.use(express.json());

app.get("/", (request, response) => {
  response.send("<h1>Hello World!</h1>");
});

app.get("/api/persons", (request, response) => {
  response.json(persons);
});

  

app.get("/api/info", (request, response) => {
  const size = persons.length;
  const date = new Date();
  response.send(`<p>Phonbook has info for ${size} people</p> <p>${date}</p>`);
});

  

const generateId = () => {
  const maxId = persons.length > 0 ? Math.floor(Math.random() * 3000) : 0;
  return maxId + 1;
};

app.get("/api/persons/:id", (request, response) => {
  const id = String(request.params.id);
  const person = persons.find((person) => person.id === id);
  if (person) {
    response.json(person);
  } else {
    console.log("x");
    response.status(404).end();
  }
});

app.post("/api/persons", (request, response) => {
  const body = request.body;
  console.log("Received body:", body);

  if (!body.name || !body.number) {
    return response.status(400).json({
      error: "name or number missing",
    });
  }

  const duplicateName = persons.find((person) => person.name === body.name);
  const duplicateNumber = persons.find((person) => person.number === body.number);
  
  if (duplicateName && duplicateNumber) {
    return response.status(409).json({
      error: "name and number must be unique",
    });
  }
  
  const person = {
    id: generateId().toString(),
    name: body.name,
    number: body.number,
  };

  persons = persons.concat(person);
  console.log("New person added:", person);
  
  response.json(person);
});

app.delete("/api/persons/:id", (request, response) => {
  const id = Number(request.params.id);
  persons = persons.filter((person) => person.id !== id);
  response.status(204).end();
});

const PORT = 3002;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```


## Imports and Initalization
```javascript
const express = require("express");
const app = express();
```
- **express**: This imports the Express framework, which simplifies the process of creating a web server.
- **app**: This creates an instance of an Express application.

## Sample Data:
```javascript
let persons = [   
	{ id: "1", name: "Arto Hellas", number: "040-123456" },   
	{ id: "2", name: "Ada Lovelace", number: "39-44-5323523" },   
	{ id: "3", name: "Dan Abramov", number: "12-43-234345" },   
	{ id: "4", name: "Mary Poppendieck", number: "39-23-6423122" }, 
];
```
- This is an array of persons objects, each with an `id`, `name`, and `number`. it acts as a mock database for the API.

## Middleware
```javascript
app.use(express.json());
```
- This middleware parses incoming JSON request and makes the parsed data available in `request.body`.

## Root Routes
```javascript
app.get("/", (request, response) => {
  response.send("<h1>Hello World!</h1>");
});

```
- Responds with a simple HTML message when the root URL is accessed.

## Get all Persons
```javascript
app.get("/api/persons", (request, response) => 
		{   response.json(persons); }
);
```
- Send the entire `persons` array as a JSON response when `/api/persons` is accessed.

## Info Route
```javascript
app.get("/api/info", (request, response) => {
	const size = persons.length;   
	const date = new Date();   
	response.send(`<p>Phonbook has info for ${size} people</p> <p>${date}</p>`); 
});
```
- Provides information about the number of persons and the current date.

## Get Person by ID
```javascript
app.get("/api/persons/:id", (request, response) => {  
	const id = String(request.params.id);  
	const person = persons.find((person) => person.id === id);   
	if (person) {   
		response.json(person);  
	} else {    
		console.log("x");    
		response.status(404).end();  
	} 
});
```
- Retrieves a specific person based on the provided ID. if the person is found, it returns their details; otherwise, it responds with a 404 status.

## Create New Person
```javascript
app.post("/api/persons", (request, response) => {   
	const body = request.body;   
	console.log("Received body:", body); 

	if (!body.name || !body.number) {    
			return response.status(400).json({      
			error: "name or number missing",     
		});
	}
	const duplicateName = persons.find((person) => person.name === body.name);   
	const duplicateNumber = persons.find((person) => person.number === body.number);  
	  
	if (duplicateName && duplicateNumber) {    
		return response.status(409).json({      
		error: "name and number must be unique",    
		});  
	}     
	const person = {     
		id: generateId().toString(),    
		name: body.name,
		number: body.number,  
	};   
	persons = persons.concat(person);
	console.log("New person added:", person);     
	response.json(person); 
});
```
- Accepts a new person's data via a POST request. It checks for missing name or number and ensures uniqueness. If valid, it adds the person to the `persons` array.

## Delete Person
```javascript
app.delete("/api/persons/:id", (request, response) =>{
	const id = Number(request.params.id);   
	persons = persons.filter((person) => person.id !== id);  
	response.status(204).end();
}
```
- Deletes a person by their ID and responds with a 204 status (No Content).

## ID Generator
```javascript
const generateId = () => {  
	const maxId = persons.length > 0 ? Math.floor(Math.random() * 3000) : 0;  
	return maxId + 1; 
};
```
- Generates a unique ID for new persons. It ensures that the ID is greater than any existing IDs.

## Server Initialization
```javascript
const PORT = 3002; 
app.listen(PORT, () => {   
	console.log(`Server running on port ${PORT}`); 
});
```
- The application listens on port 3002, and logs a message when the server is running.


# Summary
This code sets up a basic Express API for managing a phonebook with the ability to:

- Get all persons.
- Get information about the phonebook.
- Get a person by ID.
- Add a new person.
- Delete a person.

## Exercise 3.7 - 3.8
```javascript
const express = require("express");
const morgan = require("morgan");
const app = express();

app.use(express.json());

morgan.token("body", (req) => {
  return JSON.stringify(req.body);
});

app.use(
  morgan(function (tokens, req, res) {
    return [
      tokens.method(req, res),
      tokens.url(req, res),
      tokens.status(req, res),
      tokens.res(req, res, "content-length"),
      "-",
      tokens["response-time"](req, res),
      "ms",
      tokens.body(req, res),
    ].join(" ");
  })
);

...
```

-  `morgan`: This imports the Morgan library, which is used for logging HTTP request.
- `morgan.token("body",...)`: This line creates a custom token named `"body`.
- **Callback Function**: The function takes the request (`req`) as an argument and returns a stringified version of req.body. This allows you to log the body of the incoming request in your logs.
- `app.use(morgan(...))`: This applies Morgan as middleware in your Express app.
- **Callback Function**:
	- The function takes `tokens`, `req`, and `res` as parameters.
	- It constructs an array of log parts, which includes:
	    - **`tokens.method(req, res)`**: Logs the HTTP method used (e.g., GET, POST).
	    - **`tokens.url(req, res)`**: Logs the requested URL.
	    - **`tokens.status(req, res)`**: Logs the HTTP status code of the response.
	    - **`tokens.res(req, res, "content-length")`**: Logs the length of the response body.
	    - **`tokens["response-time"](req, res)`**: Logs how long the request took in milliseconds.
	    - **`tokens.body(req, res)`**: Logs the stringified body of the request, thanks to the custom token created earlier.
- The `join(" ")` method combines these log parts into a single string, separated by spaces.
### Summary
By using Morgan in this way, your Express application logs important information about each incoming HTTP request, including the method, URL, status, response time, and request body. This can be useful for debugging, monitoring, and analyzing request traffic. Just remember to be cautious about logging sensitive information, especially in production environments.