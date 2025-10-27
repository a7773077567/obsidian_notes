
## NPM Dependencies

```bash
npm install express drizzle-orm pg jsonwebtoken bcrypt dotenv
```

## Auth Routes

| Method | Endpoint  | Description             | Auth Required |
| ------ | --------- | ----------------------- | ------------- |
| POST   | `/singup` | Register a new user     | ❌             |
| POST   | `/login`  | Login and receive token | ❌             |

## URL Routes

| Method | Endpoint      | Description                                    | Auth Required |
| ------ | ------------- | ---------------------------------------------- | ------------- |
| POST   | `/shorten`    | Create a short URL form a long one             | ✅             |
| GET    | `/:shortCode` | Redirect to the original URL                   | ❌             |
| GET    | `/urls`       | Get all URLs created by the logged-in user     | ✅             |
| DELETE | `/urls/:id`   | Delete a short URL (if it belongs to the user) | ✅             |
