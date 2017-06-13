### Configure Frontend

## Lab goal:
- Configure frontend route.
- Create an angular module, controllers and define functions to handle todos.
- Create a view to be used by the app.

### 5. Configure frontend routes and core.js
Add the following route to the `server.js` file just before the `app.listen` and after the API routes.

```javascript
   app.get('*', function(req, res) {
       res.sendfile('./public/index.html');
   });
```
This will load this view and angular will handle the changes on the frontend.

Add the following to your `public/core.js` file
- Create an angular module.

```javascript
var toDoApp = angular.module('todo-app', []);
```

- Create a controller and define functions for getting, creating, and deleting todos.

```javascript
function mainController($scope, $http) {
    $scope.formData = {};

    // when landing on the page, get all todos and show them
    $http.get('/api/todos')
        .success(function(data) {
            $scope.todos = data;
            console.log(data);
        })
        .error(function(data) {
            console.log('Error: ' + data);
        });

    // when submitting the add form, send the text to the node API
    $scope.createTodo = function() {
        $http.post('/api/todos', $scope.formData)
            .success(function(data) {
                $scope.formData = {}; // clear the form so our user is ready to enter another
                $scope.todos = data;
                console.log(data);
            })
            .error(function(data) {
                console.log('Error: ' + data);
            });
    };

    // delete a todo after checking it
    $scope.deleteTodo = function(id) {
        $http.delete('/api/todos/' + id)
            .success(function(data) {
                $scope.todos = data;
                console.log(data);
            })
            .error(function(data) {
                console.log('Error: ' + data);
            });
    };

}
```

- Configure Frontend View to interact with Angular
This should assign the Angular module and controller, initialize the page that shows all of the todos,
have a form to create todos and delete todos when they are checked.

In your `public/index.html` file, add the following below:
```html
<!doctype html>

<!-- ASSIGN OUR ANGULAR MODULE -->
<html ng-app="scotchTodo">
<head>
    <!-- META -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1"><!-- Optimize mobile viewport -->

    <title>Node/Angular Todo App</title>

    <!-- SCROLLS -->
    <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css"><!-- load bootstrap -->
    <style>
        html                    { overflow-y:scroll; }
        body                    { padding-top:50px; }
        #todo-list              { margin-bottom:30px; }
    </style>

    <!-- SPELLS -->
    <script src="//ajax.googleapis.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script><!-- load jquery -->
    <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular.min.js"></script><!-- load angular -->
    <script src="core.js"></script>

</head>
<!-- SET THE CONTROLLER AND GET ALL TODOS -->
<body ng-controller="mainController">
    <div class="container">

        <!-- HEADER AND TODO COUNT -->
        <div class="jumbotron text-center">
            <h1>I'm a Todo-aholic <span class="label label-info">{{ todos.length }}</span></h1>
        </div>

        <!-- TODO LIST -->
        <div id="todo-list" class="row">
            <div class="col-sm-4 col-sm-offset-4">

                <!-- LOOP OVER THE TODOS IN $scope.todos -->
                <div class="checkbox" ng-repeat="todo in todos">
                    <label>
                        <input type="checkbox" ng-click="deleteTodo(todo._id)"> {{ todo.text }}
                    </label>
                </div>

            </div>
        </div>

        <!-- FORM TO CREATE TODOS -->
        <div id="todo-form" class="row">
            <div class="col-sm-8 col-sm-offset-2 text-center">
                <form>
                    <div class="form-group">

                        <!-- BIND THIS VALUE TO formData.text IN ANGULAR -->
                        <input type="text" class="form-control input-lg text-center" placeholder="I want to buy a puppy that will love me forever" ng-model="formData.text">
                    </div>

                    <!-- createToDo() WILL CREATE NEW TODOS -->
                    <button type="submit" class="btn btn-primary btn-lg" ng-click="createTodo()">Add</button>
                </form>
            </div>
        </div>

    </div>

</body>
</html>

```

To see expected results for this step checkout the branch step-05: `git checkout step-05`
