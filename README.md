# Axios bug hunter

In this exercise you need to find all the bugs and make sure you can list all characters, create a new character, update a character and delete one of them.

We'll be using the same API as the class example with [https://ih-crud-api.herokuapp.com/characters/](https://ih-crud-api.herokuapp.com/characters/)

If you need to refresh the methods and paths checkout the student portal :)

### Bugs, bugs and more bugs

-   [ ] Run the command `npm run dev` and try to make a request on Postman (or the provider that you prefer) to http://localhost:5005/api and check if the response is:

```json
"All good in here"
```

Hint: Check your terminal later you should see something along those lines:

```bash
Server listening on http://localhost:5000
Connected to Mongo! Database name: "bug-hunting"
```

<details> 
  <summary> Spoiler: Solution </summary>

on the `package.json` change the dev script to (You'll need to stop the script and start it again):

```json
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
```

</details>

---

-   [ ] Run the command `npm run dev` and try to make a request on Postman (or the provider that you prefer) to http://localhost:5005/api and check if the response is:

```json
"All good in here"
```

Hint: Check your terminal, the error was artificially created, check the `server.js`

<details> 
  <summary> Spoiler: Solution </summary>

on the `server.js` remove one of the `app.listen()`

```javascript
app.listen(PORT, () => {
    console.log(`Server listening on http://localhost:${PORT}`);
});

// app.listen(PORT, () => {
//     console.log(`Server listening on http://localhost:${PORT}`);
// });
```

</details>

---

-   [ ] As you can see when we request on `http://localhost:5005/character` we get:

```json
{
    "message": "This route does not exist"
}
```

This means the route doesn't exist, but if we check `characters.routes.js` we can see the route exists already.

<details> 
  <summary> Spoiler: Solution </summary>

on the `app.js` add

```javascript
const charRoutes = require('./routes/characters.routes.js');
app.use('/', charRoutes);
```

</details>

---

-   [ ] After we fix our first bug we can see the app is crashing directly, read the error, found the bug and fix it (I was typing in a hurry and used the wrong syntax, in this context, for that one).

<details> 
  <summary> Spoiler: Solution </summary>

on the `characters.routes.js` change:

change from:

```javascript
import axios from 'axios';
```

to:

```javascript
const axios = require('axios');
```

</details>

---

-   [ ] Ok, we fixed the syntax mistakes, now what? The server it's still crashing, why? Read the `TypeError` the code on `characters.routes.js` is missing a key point at the end.

<details> 
  <summary> Spoiler: Solution </summary>

on the `characters.routes.js` add:

```javascript
module.exports = router;
```

</details>

---

-   [ ] Nice, everything is working!! But when we try to click on the link on the home page we still got a problem saying that the route doesn't exist.
    -   Compare the url on Postman and on the `.get('/')`
    -   All our routes should be `http://localhost:5000/characters`, how we can achieve that without changing the routes inside the parenthesis on `characters.routes.js`?

<details> 
  <summary> Spoiler: Solution </summary>

on the `app.js` change to:

```javascript
app.use('/characters', charRoutes);
```

</details>

---

-   [ ] How many bugs you get on the code Felipe? Now everything should be fine, so why we don't have any `res`ponse on Postman? Check the routes.

<details> 
  <summary> Spoiler: Solution </summary>

on the `characters.routes.js` change to:

```javascript
res.status(200).json({ characters: response.data });
```

</details>

---

-   [ ] Try to create a character using Postman making a POST request to `http://localhost:5005/characters/create`

with (debt is optional):

```json
{
	"name": string,
	"occupation": string,
	"weapon": string,
	"debt": boolean
}
```

Try to edit the character using Postman making a PUT request to
The id being used in the URL should be of the character just created :)
`http://localhost:5005/characters/id/edit`

`Hint: Check the method being used`

<details> 
  <summary> Spoiler: Solution </summary>

on the `characters.routes.js` on the route that updates a character change the method to PUT:

```javascript
router.put('/:id/edit', (req, res, next) => {...})

axios.put(`https://ih-crud-api.herokuapp.com/characters/${req.params.id}`, updatedCharacter);
```

</details>

---

### Bonus

-   [ ] Up to you now implement the functionality to delete a character.

-   [ ] How you could make the code DRYer?

    -   Remove some inconsistency
    -   Reuse some pieces of code

-   [ ] If you need to change the API call, it would be a pain to change in all the places, you could use a `variable` in this case, but if you need that info across different files / folders we can have a little bit of repetition so try to create a `environment variable` that will hold the `BASE_URL`.

-   [ ] Change all the calls of axios to a service, so if you need to change to another library you could change in a single place :D
