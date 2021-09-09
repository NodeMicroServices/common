# Node Micro-Services Common Code
For ease of code sharing between different projects

**Warning: It is for private use and might be pretty useless to you. 
Don't use it in your project unless you know what you're doing.**

---

### Install
```shell
$ npm i @beevk/express-common
```

### Usage Example:

```javascript
// Other imports
import { notFoundError, currentUser } from '@beevk/express-common';

const app = express();

app.get('/', currentUser, (req, res) => {
    // if user is logged in, this would be defined
    const userInfo = req.user;
    // Rest of stuffs
});

// Other routes

// route for 404
app.all('*', async () => {
    throw new NotFoundError();
})
```

## Custom Errors available
`authorizationError` **:** 401, Can use for unauthorized request.

`badRequestError` **:** 400, expects a `string` as parameter.

`databaseError` **:** 500, Failed to connect to DB.

`notFoundError` **:** 404, Not Found.

`requestValidationError` **:** 400, For failed validation (express-validator) requests.

## Custom Middlewares
`currentUser` **:**
- Checks cookie in request to see if user JWT is sent along with request.
- Attaches data in JWT to request as `req.user`.


`authorize` **:** fn
- Should only be used after `currentUser` middleware.
- Takes an optional array of roles (string) as parameter.
- Checks if that role is present in req.user.

`errorHandler` **:** 
- Global catch block.
- Use as last middleware for the App.

`validateRequest` **:** fn
- Accepts validation Schema (from express-validator) as parameter
- Validates incoming data in request against schema & throws error if data is invalid

---
### The middlewares marked as `fn` should be used as a function.

Example:
```javascript
const validRolesToCreateArticle = ['author', 'admin'];

app.post('/article', currentUser, authorize(validRolesToCreateArticle), (req, res) => {
    // Implementation for this route    
})
```

---
## License
This package is created & published under [MIT License](https://opensource.org/licenses/MIT).
