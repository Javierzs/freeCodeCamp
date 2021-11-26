# freeCodeCamp
Curso: Back End Development and APIs

Code: https://glitch.com/edit/#!/bow-pleasant-sauroposeidon
Live Site: https://bow-pleasant-sauroposeidon.glitch.me

En myApp.js se puede encontrar el código solución para cada punto

var express = require("express");
var app = express();

/* 11) Use body-parser to Parse POST Requests*/
var bodyParser = require("body-parser");

app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

/** 7) Implement a Root-Level Request Logger Middleware*/
app.use(function(req, res, next) {
  console.log(req.method + " " + req.path + " - " + req.ip);
  next();
});

/**1) Meet the Node Console*/
console.log("Hello World");

module.exports = app;

/**2)Start a Working Express Server*/
/*app.get("/", function(req, res) {
  res.send("Hello Express");
});*/

/** 3) Serve an HTML File*/
app.get("/", function(req, res) {
  res.sendFile(__dirname + "/views/index.html");
});

/** 4)Serve Static Assets*/
// Normal usage
//app.use(express.static(__dirname + "/public"));

// Assets at the /public route
app.use("/public", express.static(__dirname + "/public"));

/** 5) Serve JSON on a Specific Route*/
/*app.get("/json", function(req, res) {
  res.json({
    message: "Hello json"
  });
});*/

/** 6) Use the .env File*/
let message = { message: "Hello json" };

app.get("/json", function(req, res) {
  if (process.env.MESSAGE_STYLE == "uppercase") {
    res.json({
      message: "HELLO JSON"
    });
  } else {
    res.json(message);
  }
});

/**8) Chain Middleware to Create a Time Server*/
app.get(
  "/now",
  (req, res, next) => {
    req.time = new Date().toString();
    next();
  },
  (req, res) => {
    res.send({
      time: req.time
    });
  }
);

/*9) Get Route Parameter Input from the Client*/
/*app.get("/:word/echo", (req, res) => {
  const { word } = req.params;
  res.json({
    echo: word
  });
});*/
app.get("/:word/echo", (req, res) => {
  res.json({ echo: req.params.word });
});

/* 10) Get Query Parameter Input from the Client*/
//{name: 'firstname lastname'}
app.get("/name", (req, res) => {
  var firstName = req.query.first;
  var lastName = req.query.last;
  // OR you can destructure and rename the keys
  //var { first: firstName, last: lastName } = req.query;
  // Use template literals to form a formatted string
  res.json({
    name: `${firstName} ${lastName}`
  });
});

/* 12) Get Data from POST Requests*/
// {name: 'firstname lastname'}
app.post("/name", (req, res) => {
  var firstName = req.body.first;
  var lastName = req.body.last;
  // OR you can destructure and rename the keys
  //var { first: firstName, last: lastName } = req.query;
  // Use template literals to form a formatted string
  res.json({
    name: `${firstName} ${lastName}`
  });
});
