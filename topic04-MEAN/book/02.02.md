### Define Model and Routes

## Lab Goal:
Configure the model and routes for the REST API

### 4. Configure model and routes for the REST API
First, we need to define a model for our data that has a text field of type string.
MongoDB will automatically generate the `_id` field for each record created.
Copy this just before the `app.listen` in your `server.js`

```javascript
var Todo = mongoose.model('Todo', {
    text : String
});
```
Next, we need to generate our Express routes to handle our API calls. Add this to your `server.js` file
just after the model definition above. This will make our application's API accessible from /api/todos.

```javascript
app.get('/api/todos', function(req, res) {

    // use mongoose to get all todos in the database
    Todo.find(function(err, todos) {

        // if there is an error retrieving, send the error. nothing after res.send(err) will execute
        if (err)
            res.send(err)

        res.json(todos); // return all todos in JSON format
    });
});

// create todo and send back all todos after creation
app.post('/api/todos', function(req, res) {

    // create a todo, information comes from AJAX request from Angular
    Todo.create({
        text : req.body.text,
        done : false
    }, function(err, todo) {
        if (err)
            res.send(err);

        // get and return all the todos after you create another
        Todo.find(function(err, todos) {
            if (err)
                res.send(err)
            res.json(todos);
        });
    });

});

// delete a todo
app.delete('/api/todos/:todo_id', function(req, res) {
    Todo.remove({
        _id : req.params.todo_id
    }, function(err, todo) {
        if (err)
            res.send(err);

        // get and return all the todos after you create another
        Todo.find(function(err, todos) {
            if (err)
                res.send(err)
            res.json(todos);
        });
    });
});
```

|   HTTP Verb	|   URL	|  DESCRIPTION 	|
|-------------|-------|---------------|
|   GET 	    |/api/todos|  Get all of the todos 	|
|   POST 	    |/api/todos| Create a single todo   |
|   DELETE    |/api/todos/:todo_id| Delete a single todo|

To see expected results for this step checkout the branch step-04: `git checkout step-04`
