



-I Prepared the backend Ubuntu server

Obtain the location of nodejs software from ubuntu repositories over the internet.

```curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash ```

Install Node.js 

```sudo apt-get install -y nodejs```

-Verify if node js and npm is installed
```node -v      npm -v```



*Application Code Setup*


- Create the directory Todo on the desktop
``` mkdir Todo```

Move into the Todo directory

```cd Todo```


- Use the command npm init to initialise your project
```sudo npm init```
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/init.jpg)

- I Installed ExpressJS*
```npm install express```


I created a file index.js
```sudo touch index.js```

I Installed the dotenv module
```npm install dotenv```
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/dotenv.jpg)


I edited the index.js file as shown
sudo nano index.js
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/index.jpg)


- I started the server

```node index.js```

![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/server.jpg)


I Opened my your browser and opened ```localhost:5000```
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/welcome.jpg) 


-To Create a task,i created `routes` that will define various endpoints that the `todo` app

I created a folder `routes`

```sudo  mkdir routes```

I moved directory into the routes folder.

`cd routes` 


- I created a file `api.js`
`sudo touch api.js`

I edited the `api.js` file as follows
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/api.jpg)






*Creating a Schema and a model* 


- I changed directory back Todo folder with `cd ..`

- I installed Mongoose

`npm install mongoose`


-I  created a folder  `models`
And move directory into the newly created `models` folder
`sudo mkdir models && cd models`

- I created a file named `todo.js` in the `models` folder



I moved back into the `routes` directory,i opened `api.js` with `sudo nano api.js`, delete the code inside with and replace with the code below :

```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
```


* Setting up a MongoDB database where data will be stored *
- I used mLab to provide MogoDB database as a service solution


- I signed up for a shared clusters free account via https://www.mongodb.com/atlas-signup-from-mlab

I created a MongoDB database and collection inside mLab as shown below
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/db.jpg)


- Create a file named `.env` in the `TODO` directory .

`sudo touch .env`


-  Add a connection string to access the database in it as shown below:
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/env.jpg)
*Please note connection string was obtained from the connection tab of MongoDB cluster.*



- I updated the index.js file to reflect the use of `dotenv` with the code below

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

- I started the node js server
`node index.js`


** Testing The Code Without A Frontend User Interface**

- I used `Postman` to test for the API

- I installed `postman` as shown
![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/postman.jpg)


- I opened Postman, created a post request to the API http://localhost:5000/api/todos as shown below:

![](https://github.com/drazen-dee28/MERN-STACK/blob/main/Images/post.jpg)






CREDITS

[DevOps Experts](www.darey.io)

[Mongodb Clusters](https://www.mongodb.com/atlas)

[How to install Postman](https://linuxize.com/post/how-to-install-postman-on-ubuntu-20-04/)