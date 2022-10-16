# Robo 3T
A graphical user interface commonly used with MongoDB. WHich can replace the mongo shell / command line

You can :

1. Create a database
2. Create a collection (or table)
3. Insert document

#  Server setup

```javascript
npm install body-parser mongoose ejs express
```

```javascript
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require('mongoose');

const app = express();

app.set('view engine', 'ejs');

app.use(bodyParser.urlencoded({
  extended: true
}));
app.use(express.static("public"));

app.listen(3000, function() {
  console.log("Server started on port 3000");
});
```

# Mongoose

```javascript
mongoose.connect("mongodb://localhost:27017/wikiDB");
```

```javascript
const articleSchema = {
  title: String,
  content : String
}

const Article = mongoose.model("Article", articleSchema);
```

# REST API

## 1. GET All articles

```javascript
app.route("/articles")

.get(function(req,res){

  Article.find(function(err, foundArticles){
    if(!err){
        res.send(foundArticles);
    } else {
      res.send(err);
    }
  });
})
```

## 2. POST an article

But for POST, how can we post if we don't have a frontend : use Postman

```javascript
app.route("/articles")

.post(function(req,res) {

  console.log(req.body.title);
  console.log(req.body.content);

  const newArticle = new Article({
    title:req.body.title,
    content:req.body.content
  });

  newArticle.save(function(err){
    if (!err) {
      res.send("success");
    } else {
      res.send(err);
    }
  });
})
```

In POSTMAN :

1. change to POST request
2. put url
```javascript
localhost:3000/articles
```
3. go to body tab
4. change encoding to form-urlencoded
5. input the variable in 'key' column and values

<br>

## 3. DELETE all articles

```javascript
app.route("/articles")

.delete(function(req, res){

  Article.deleteMany(function(err){
    if(!err) {
      res.send("success");
    } else {
      res.send(err);
    }
  })
});
```

In POSTMAN :

1. change to DELETE request
2. put url
```javascript
localhost:3000/articles
```

<br>

## 4. GET a specific Article

```javascript
app.route("/articles/:articleTitle")

.get(function(req,res) {

  Article.findOne({title:req.params.articleTitle}, function(err,foundArticle){
    if (foundArticle) {
      res.send(foundArticle);
    }else {
      res.send("No article found");
    }
  });
});
```
in the url search bar, "space" is equal to %20

<br>

## 5. PUT a specific Article

```javascript
app.route("/articles/:articleTitle")

.put(function(req,res){

Article.replaceOne(
    {title: req.params.articleTitle},
    {title: req.body.title, content: req.body.content},
    function(err) {
      if (!err) {
        res.send("success");
      }
    }
  );
```


In POSTMAN :

1. change to PUT request
2. put url
```javascript
localhost:3000/articles/Jack%20Bauer
```
3. go to body tab
4. change encoding to form-urlencoded
5. input the variable in 'key' column and values

<br>

## 6. PATCH a specific Article

```javascript
app.route("/articles/:articleTitle")

.patch(function(req,res){

  Article.updateOne(
    {title: req.params.articleTitle},
    {$set: req.body},
    function(err){
      if(!err){
        res.send("success")
      } else {
        res.send(err);
      }
    }
  );
})
```


In POSTMAN :

1. change to PATCH request
2. put url
```javascript
localhost:3000/articles/Jack%20Bauer
```
3. go to body tab
4. change encoding to form-urlencoded
5. input the variable in 'key' column and values

<br>

## 7. DELETE a specific Article

```javascript
app.route("/articles/:articleTitle")

.delete(function(req,res) {
    
  Article.deleteOne(
    {title:req.params.articleTitle},
    function(err){
      if(!err){
        res.send("success");
      } else {
        res.send(err);
      }
    }
  );
```

In POSTMAN :

1. change to PATCH request
2. put url
```javascript
localhost:3000/articles/Jack%20Bauer
```
3. go to body tab
4. change encoding to form-urlencoded
5. input the variable in 'key' column and values