# Command-line CRUD Application

In this pre-reading, you'll be building an app that helps motivate people to go outside and seek out nature. The person will go outside, try to (safely) find as many animals as possible, then record them. Rarer animals (fauna) will be worth more points.

## Lesson Objectives

By the end of this lesson

- Develop a list of minimal features
- Create an app that uses user input to
- Create data
- Read data
- Delete data
- Update data

## Getting started

With coding, there is always a temptation to jump into coding and solve whatever comes along. However, it's essential to take a few steps back. When you code without a plan, you can create features that no one asked for, make your code too complex, wholly misunderstand what you are building, or try to build something impossible within the time and technology constraints.

Building an app that lets users look for animals in NYC can conjure a lot of cool features. User Stories will help define which features are worth building. A user story is an informal, general explanation of a software feature written from the perspective of the end user. Its purpose is to articulate how a software feature will provide value to the customer. The list below uses a particular pattern that usually starts with the words "A user can...". Writing out features this way can be helpful because a non-technical person can understand what the feature should do and a technical person can understand what functionality they need to build.

**User Story List**

- A user can add a map location where the animal was seen.
- A user can see a list of animals.
- A user can see a more detailed view of an animal, including a picture and description of each animal.
- Create a checking system to ensure users are not making up what they've seen.
- Allow a user to take a photo and let AI determine what animal it is.
- If an animal is found, add a pin to a map location and store that for the user.
- Call a wildlife center if an animal is dangerous or sick.
- Allow users to participate in a high scores daily board.
- Allow users to participate in an all-time high scores board.
- Allow users to compete against friends.

Suddenly, a "simple" app has grown quite complex and would require numerous technologies. Most business ideas start with something called `Minimum Viable Product`. It is the most basic form of the app for users to use. For example, when eBay started, there was no payment system. People had to mail each other checks or cash. When building your apps, figuring out the minimum functionality required is crucial. Most of the time, the app will be underwhelming and clunky in places. The user experience will be less than ideal. Let's whittle down the above list into something you can build today.

- A user can have a list of animals they've seen
- A user can create a new animal
- A user can see the details of a new animal
- A user can delete an animal
- A user can update an animal
- A user can see their score

Based on the user stories above, create scripts that would run the following:

**Note:** (`<animal_id>` represents the unique id that was created by a package you will later install called `nanoid`, which creates random ids. An id will be created for each animal you later create.)

```
npm run index
npm run create bat
npm run show <animal_id>
npm run update <animal_id> "red fox"
npm run destroy <animal_id>
npm run score
```

Code along and get your hands dirty.

- `mkdir fauna-go`
- `cd fauna-go`
- `touch .gitignore` (and add appropriate files and folders)
- `touch index.js`
- `mkdir src`
- `touch src/helpers.js`
- `npm init -y`
- `npm install nanoid@3`

Replace the `"scripts"` object in `package.json` with this code:

```json
 "scripts":{
  "index": "node index.js index",
  "create": "node index.js create",
  "show": "node index.js show",
  "update": "node index.js update",
  "destroy": "node index.js destroy",
  "score": "node index.js score"
}
```

In the `index.js` file, create a `run` function that will handle your actions (index, create, show, update, destroy, score).

Additionally, `console.log` will serve two purposes, to `inform` your user about something or for you (the developer) to debug. To help clarify which console log does what, you can give `console.log` an alias called `inform`. You will use call this variable holding the console.log function in your actions. This will allow the user to see information in the terminal.

```js
// create an alias called inform to store the console.log function
// When providing user feedback in the terminal use `inform`
// When developing/debugging use `console.log`
const inform = console.log;

function run() {
  const action = process.argv[2];
  const animal = process.argv[3];
  switch (action) {
    case 'index':
      inform(action);
      break;
    case 'create':
      inform(action, animal);
      break;
    case 'show':
      inform(action, animal);
      break;
    case 'update':
      inform(action, animal);
      break;
    case 'destroy':
      inform(action, animal);
      break;
    case 'score':
      inform(action);
      break;
    default:
      inform('There was an error.');
  }
}

run();
```

Test each script to be sure things work as expected. Each response should be a console log of the information given as arguments and nothing more:

```
npm run index
npm run create bat
npm run show <make up a fake id>
npm run update <make up a fake id> "red fox"
npm run destroy <make up a fake id>
npm run score
npm run oops
```

### Data

This `animals.json` file in the `data` directory will store any animals that you create. As if you were storing animals in a database:

- `mkdir data`
- `touch data/animals.json`

Add an empty array in the `animals.json` file

## Helpers

In the `helpers.js` file, create the functionality that will allow the application to read from and write information to your file using the `fs` or `file system` package that is automatically included with NodeJS:

```js
const { readFileSync, writeFileSync } = require('fs');

function readJSONFile(path, fileName) {
  const collection = readFileSync(`${path}/${fileName}`, 'utf8');
  return collection ? JSON.parse(collection) : [];
}

function writeJSONFile(path, fileName, data) {
  data = JSON.stringify(data);
  return writeFileSync(`${path}/${fileName}`, data, { encoding: 'utf-8' });
}

module.exports = {
  readJSONFile,
  writeJSONFile
};
```

### Index

Import the `readJSONFile` and `writeJSONFile`functions from the `helpers.js` file into `index.js`.

In the `index.js` file's run function, import the data from the `animals.json`

```js
const { readJSONFile, writeJSONFile } = require('./src/helpers');
```

The `readJSONFile` will read from the object in `animals.json`

```js
const animals = readJSONFile('./data', 'animals.json');
```

Add an `animals` parameter to the "index" case "inform" function call

```js
 case "index":
   inform(action, animals);
```

For now, when you run `npm run index` you should see an empty array since `animals.json` only holds an empty array at the moment.

### Controller Directory

In our `index.js` file. We want to be able to call all of the actions such as show, create, destroy etc. But the `single concern` of this file is to create and run the `run` function for the application. We need a file that will handle the business of creating all of our action functions. This is where our `controller` directory and file will come into play. We name our controller file something that refers to the subject it will be handling.

In the `src` folder, make a new file called `animalController.js`

### Create

The user will run the command `npm run create <name of animal>` to create a new animal. A new animal should be a new entry into the user's data that will be stored in the `animals.json` file. Each animal should have a unique id, a name, and the points associated with it. Then the animal should be added to the end of the animals array.

This functionality controls what happens to the data. Therefore it is a separate concern from the other functionality you've built. That is why we will store this in the `animalController.js` file.

```js
function create(animals, animalName) {
  const animal = { name: animalName };
  animals.push(animal);
  return animals;
}

module.exports = {
  create
};
```

Remember that we've added an npm package to generate a unique id called `nanoid`. (check your `package.json` dependencies object)

In your `animalsController.js` require `nanoid` and use it in your `create` function

```js
const { nanoid } = require('nanoid');

function create(animals, animalName) {
  // Limit the id to be 4 characters long by placing a 4 as the argument for nanoid
  const animal = { name: animalName, id: nanoid(4) };
  animals.push(animal);
  return animals;
}
```

### Resource

We are going to create a separate resource in our `data` folder called `animalPoints.json`. This file will allow us to grab points for animals that are already stored in our application, but not in our `animals.json`. When we `create` an animal for our `animals.json` file, our application will search the `animalPoints.json` to see if the animal exists in your 'database' so to speak. If it does, it will then create an object holding the points for this animal, along with the name of the animal and the generated id. If the animal the user cretes does not exist, you will default the animal to have 10 points. When the `create` action is called, the information will be stored in your `animals.json` file.

**Note:** A JSON file can also hold a single object. Not only an array.

Add the object below to your `animalPoints.json` file.

```json
{
  "squirrel": 2,
  "coyote": 40,
  "owl": 10,
  "bald eagle": 100,
  "seal": 30,
  "virginia opossum": 40,
  "osprey": 20,
  "peregrine falcon": 30,
  "piping plover": 20,
  "red-tailed hawk": 50,
  "bat": 30,
  "racoon": 10,
  "spotted salamander": 50,
  "spring peeper": 50,
  "whale": 200,
  "canadian goose": 5,
  "horseshoe crab": 10,
  "painted turtle": 20,
  "white-tailed deer": 20,
  "american bullfrog": 40,
  "diamond-backed terrapin": 20,
  "red-backed salamander": 60,
  "monarch butterfly": 50,
  "red fox": 80
}
```

## animalController.js update

You will access the points by key or set the default:

Import the `animalPoints` into the `animalController.js` file.

```js
const animalPoints = require('../data/animalPoints.json');
```

Add the create function to the `animalController.js` file

```js
function create(animals, animalName) {
  const animal = {
    name: animalName,
    id: nanoid(4),
    points: animalPoints[animalName] || 10 // if the animal you create does not exist in the resource object of animals, it will be given a default points value of 10
  };
  animals.push(animal);
  return animals;
}
```

## Save the created animal to the `animals.json` file.

In your `index.js` file, start by importing the `create` function.

```js
const { create } = require('./src/animalController');
```

Next add two variables to the top of the `run` function. One variable, `writToFile`, will hold a boolean value that acts as a toggle. The other variable,`updateAnimals`, will hold an array of the updated or created animals

```js
function run {
  let writeToFile = false;
  let updatedAnimals = [];
  // Rest of code...
```

When creating or updating, we must tell the application whether or not it should write to a file. The best way to do this is to use a boolean to only write to a file when true. We defaulted that boolean `writeToFile` earlier to false. When a create or update has happened, we need to convert that boolean to true. At the end of the function, we will write a conditional to decide whether or not the function should write to the `animals.json` file.

Update the `create` case.

```js
 case "create":
  updatedAnimals = create(animals, animal);
  writeToFile = true;
  break;
```

You will notice that we changed or `toggled` the `writeToFile` variable to true. At the end of the function, we now need write logic to check that variable. If the variable is true, we update the `animals.json` file with the new animal.

Add this block of code below the `switch` statement

```js
if (writeToFile) {
  writeJSONFile('./data', 'animals.json', updatedAnimals);
}
```

### Minimum Viable Product

You may wonder, what if the user enters `Bat` instead of `bat`? Or what if the user has seen an animal not on the list? What if the user misspells `peregrine` or only knows they saw a `falcon`? Wouldn't it be nice to solve these things?

Yes.

However, look back to your initial plan. None of these features are part of the plan. When working on a team, you will be given features to build, and you should only build those features. If you think you need to make something else, you would talk to your team/manager and decide whether to switch what you are working on or add these features to a list of things to work on in the future.

### Index (again)

Typically, an index is a list of things with limited details. When you think of an online store, you usually see one image, a name, and a price. Then when you click the item, you will see more details.

Add the index functionality to the `animalsController.js` file so that we can import it and call it as an action in the `index.js`.

```js
function index(animals) {
  return animals.map((animal) => animal.id + ' ' + animal.name).join('\n');
}
```

Export the `index` function and import this function to `index.js`.

In the `index.js` file, update the `index` case.:

```js
 case "index":
  const animalsView = index(animals);
  inform(animalsView);
  break;
```

### Show

Thinking about an online store, you also want to see a detailed view of an item. In this case, the user would pass the id to see a detailed view. The sample id shown is the generated `<animal_id>`. The user will eventually type the command below where the id is the id that is stored for a particular animal in the `animals.json` file.

```
npm run show <animal_id>
```

`show` function/action will search the array of animals and find the object that has a matching id to the `animalID` parameter. It will then send back a human-readable view:

Add the show function to the `animalController.js`

```js
function show(animals, animalId) {
  const animal = animals.find((animal) => animal.id === animalId);
  return animal.id + ' ' + animal.name + ' ' + animal.points + ' points';
}
```

Export the `show` function and import this function to `index.js`.

### Destroy

Destroying an animal object will also require the animal id.

> **Note**: `delete` is a keyword in JavaScript. You cannot name a function using this word. Therefore, the alternative word `destroy` will be used.

```
npm run destroy <animal_id>
```

In the `animalController.js` file, declare this `inform` variable. It allows us to use `console.log` Also add the `destroy` function

```js
const inform = console.log;

function destroy(animals, animalId) {
  const index = animals.findIndex((animal) => animal.id === animalId);
  if (index > -1) {
    animals.splice(index, 1);
    inform('Animal successfully removed from collection');
    return animals;
  } else {
    inform('Animal not found. No action taken');
    return animals;
  }
}
```

Export the `destroy` function and import this function to `index.js`.

In your `index.js` file, import the `destroy` function and update the `destroy` case

```js
 case "destroy":
   updatedAnimals = destroy(animals, animal);
   writeToFile = true;
   break;
```

### Update

You'll use the `id` for this action as well. Additionally, you'll need to enter the new animal name.

```
npm run update <animal_id> "red fox"
```

In the `animalsController.js` add the edit function

```js
// Animals Controller
function edit(animals, animalId, updatedAnimal) {
  const index = animals.findIndex((animal) => animal.id === animalId);
  if (index > -1) {
    animals[index].id = animalId;
    animals[index].name = updatedAnimal;
    animals[index].points = animalPoints[updatedAnimal] || 10;
    inform('Animal successfully updated');
    return animals;
  } else {
    inform('Animal not found. No action taken');
    return animals;
  }
}
```

Export the `update` function and import this function to `index.js`.

In `index.js`, Update the `update` case

```js
 case "update":
   updatedAnimals = edit(animals, animal, process.argv[4]);
   writeToFile = true;
   break;
```

### Score

The score will be the sum of the animal points. Add the score function to the `animalsController.js`

```js
// Animals controller
function score(animals) {
  return animals.reduce((acc, current) => acc + current.points, 0);
}
```

Export the `score` function and import this function to `index.js`.

In the `index.js` update the `score` case

```js
// index.js
 case "score":
   inform(`Current points sum of all animals you've added to your database:`, score(animals));
   break;
```

### Future Improvements

You have now built a complete CRUD app. No matter how many features you build, an app is never quite finished. What are the next steps you'd take to improve this app?

## Reference Files

### index.js

```js
cconst { readJSONFile, writeJSONFile } = require("./src/helpers");

const {
  create,
  index,
  show,
  destroy,
  edit,
  score,
} = require("./src/animalController");

// create an alias called inform to store the console.log function
// When providing user feedback in the terminal use `inform`
// When developing/debugging use `console.log`
const inform = console.log;
const animals = readJSONFile("./data", "animals.json");

function run() {
  const action = process.argv[2];
  const animal = process.argv[3];

  let writeToFile = false;
  let updatedAnimals = [];

  switch (action) {
    case "index":
      const animalsView = index(animals);
      inform(animalsView);
      break;
    case "create":
      updatedAnimals = create(animals, animal);
      writeToFile = true;
      break;
    case "show":
      const animalView = show(animals, animal);
      inform(animalView);
      break;
    case "update":
      updatedAnimals = edit(animals, animal, process.argv[4]);
      writeToFile = true;
      break;
    case "destroy":
      updatedAnimals = destroy(animals, animal);
      writeToFile = true;
      break;
    case "score":
      inform(
        `Current points sum of all animals you've added to your database: ${score(
          animals
        )}`
      );
      break;
    default:
      inform("There was an error.");
  }
  if (writeToFile) {
    writeJSONFile("./data", "animals.json", updatedAnimals);
  }
}

run();

```

### animalController.js

```js
const { nanoid } = require('nanoid');

const animalPoints = require('../data/animalPoints.json');

function create(animals, animalName) {
  const animal = {
    name: animalName,
    id: nanoid(4),
    points: animalPoints[animalName] || 10
  };
  animals.push(animal);
  return animals;
}

function index(animals) {
  return animals.map((animal) => animal.id + ' ' + animal.name).join('\n');
}

function show(animals, animalId) {
  const animal = animals.find((animal) => animal.id === animalId);
  return animal.id + ' ' + animal.name + ' ' + animal.points + ' points';
}

// animalController.js
const inform = console.log;

function destroy(animals, animalId) {
  const index = animals.findIndex((animal) => animal.id === animalId);
  if (index > -1) {
    animals.splice(index, 1);
    inform('Animal successfully removed from collection');
    return animals;
  } else {
    inform('Animal not found. No action taken');
    return animals;
  }
}

function edit(animals, animalId, updatedAnimal) {
  const index = animals.findIndex((animal) => animal.id === animalId);
  if (index > -1) {
    animals[index].id = animalId;
    animals[index].name = updatedAnimal;
    animals[index].points = animalPoints[updatedAnimal] || 10;
    inform('Animal successfully updated');
    return animals;
  } else {
    inform('Animal not found. No action taken');
    return animals;
  }
}

function score(animals) {
  return animals.reduce((acc, current) => acc + current.points, 0);
}

module.exports = {
  create,
  index,
  show,
  destroy,
  edit,
  score
};
```
