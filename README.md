# Express.js RESTful Store
This is an ESM-only module, it must be imported using the `import` declaration or the `import()` syntax. It cannot be required from a CommonJS module.

## Usage in express
Example:
```javascript
import express from 'express'
import session from 'express-session'
import RestStore from './index.js'

const app = express()

app.use(session({
  secret: 'xxxxxxxxxxxxxxxx',
  store: new RestStore('http://127.0.0.1/sessions'),
  cookie: {
    maxAge: 24 * 60 * 60 * 1000,
    httpOnly: true,
    secure: false
  },
  saveUninitialized: false,
  resave: false
}))
```

## RESTful API Specification (endpoint)

### Get list of all Sessions
---

Request

```
GET /
```

Response

```json
[
  {
    "key": "value"
  },
  ...sessions
]
```

### Remove everything
----

Request
```
DELETE /
```

Reponse
```json
{
  "status": "OK"
}
```

### Get one Session
----

```
GET /{sid}
```

Response

```json
{
  "key": "value"
}
```
If the session doesn't exist, the API should respond with a 404 error code.

### Delete one Session
---
```
DELETE /{sid}
```

Reponse

```json
{
  "status": "OK"
}
```

### Add one Session
---

Request

```json
POST /{sid}

{
  "key": "value"
}
```

Response

```json
{
  "key": "value"
}
```

This can also be used in conjuction with the `?ping` URL parameter. Then the API is able to update any existing expire times of the session if any.
If the session with `{sid}` already exists, the API should update that one.