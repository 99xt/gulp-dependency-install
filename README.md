# gulp-dependency-install
This gulp plugin provides commands to install npm dependencies in multiple package.json files with a single command. It also allows to define custom local dependencies inside package.json.

## Installation
To include dependency-install in your project use
`npm install gulp-dependency-install --save`

## Usage
### With directory path argument
Edit your gulpfile.js and include the following (If gulpfile.js not exits, create a new gulpfile.js in the project root).
```javascript
var gulp = require('gulp');
var gulpDi = require('gulp-dependency-install');

gulp.task('npm-install', function () {
	return gulpDi.install();
});
```
Open a shell/commandline and run the followig command `gulp npm-install --dir <your-directory>`
Note: This executes npm install for all the nested package.json files inside '<your-directory>' path.

### With directory path variables
```javascript
var gulp = require('gulp');
var gulpDi = require('gulp-dependency-install');

gulp.task('npm-install', function () {
	return gulpDi.install(['<your-directory>']);
});
```
Open a shell/commandline and run the followig command `gulp npm-install`
Note: You can also include multiple directories inside the array.

### Custom dependencies
What if you need to share code, but don't wish to publish packages in NPM Registry?
You can use the Custom Dependancies feature.

1. Create a directory(e.g 'your_local_dependancies') in your codebase to store each local dependency(library) code. Create directories for each of the dependencies with a index.js inside, as shown below.
    ```
    __/your_local_dependancies
        |__dependency-a
            |__index.js
        |__dependency-b
            |__index.js   
            |__package.json // Optional if you have other npm dependancies
    ```

    e.g of index.js inside dependency-a
    ```javascript
    var sayHello = function() {
        console.log("hello");
    };
    module.exports.sayHello = sayHello;
    ```
    
2. Open a package.json file in your code base which depends on a local dependency (lets say 'dependency-a' and 'dependency-b') and include the section 'customDependencies', as shown below.
    ```json
    {
            "dependencies": {},
            "dependencies": {
                "dependency-a" : "local",
                "dependency-b" : "local"
            }
    }
    ```
    
3. Initialize the custom dependency path for the library, as shown below.
    ```javascript
    var gulp = require('gulp');
    var gulpDi = require('gulp-dependency-install');

    gulp.task('npm-install', function () {
        gulpDi.init('your_local_dependancies'); // Optionally initialize the directory where custom dependencies resides
        return gulpDi.install(['<your-directory>']);
    });
    ```
    Open a shell/commandline and run the followig command `gulp npm-install`
    
    Note: This executes 'npm install' for all the package.json files, installing NPM dependencies from NPM Registry and copying the custom dependancies from 'your_local_dependancies' directory to the respective 'node_modules' directory..
    
4. Finally in your code requrie the dependency similar in using a module from NPM Registry, as shown below.
    ```javascript
    var dependencyA = require('dependency-a');
    dependencyA.sayHello(); // Prints "Hello"
    ```

## License
  [MIT](LICENSE)