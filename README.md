# mock-server
how to implement your frontend job before backend complete his job
With this job,you can start your job any time your want. Of course you have to communicate with your buddy to confirm the structure and content in your project.

### + complete your package.json

we need use restify to return data;
```
{
  "name": "mock-service",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "restify": "^6.3.4"
  }
}
```

### + complete your index.js
 we need config  the api which data file  should it return 
 ```
 var restify = require('restify');
var _ = require('lodash');

const server = restify.createServer({
  name: 'myapp',
  version: '1.0.0'
});

server.use(restify.plugins.acceptParser(server.acceptable));
server.use(restify.plugins.queryParser());
server.use(restify.plugins.bodyParser());
server.pre(restify.pre.sanitizePath());

['products', 'items', 'sections'].forEach((item) => {
  const data = require(`./entities/${item}.json`);
  server.get(`/api/${item}`, (req, res, next) => {
    res.send(200, data);
    return next();
  })
})

const collections = require('./entities/collections.json')
server.get('/api/collections', (req, res, next) => {

	if(req.query.priceLow && req.query.priceHigh) {
		res.send(200, _.filter(collections, (c) => c.price >= req.query.priceLow && c.price <= req.query.priceHigh))
	} else {
		res.send(200, collections);
	}

	return next();
})

server.get('/api/collections/:id', (req, res, next) => {
  const c = _.find(collections, (c) => c.id == req.params.id)
  res.send(200, c);

  return next();
})

server.listen(8080, function () {
  console.log('%s listening at %s', server.name, server.url);
});
 
 ```
 
 


### + complete your return json files
 the data file
 
```
[
	{"key": 1, "name": "iPhone 8", "price": 8000, "date": "2017-12-15T02:56:36.377Z"},
	{"key": 2, "name": "Huawei P9", "price": 8500, "date": "2017-11-15T02:56:36.377Z"},
	{"key": 3, "name": "Xiaomi Meta", "price": 3500, "date": "2017-10-15T02:56:36.377Z"}
]
```
