# Backend Project - AI Coding Agent Instructions

## Architecture Overview

This is an **Express.js + MongoDB backend** using Node.js. The project follows a modular architecture with clear separation of concerns:

- **Controllers**: Handle HTTP request/response logic
- **Routes**: Define API endpoints and route handlers
- **Models**: Mongoose schemas for MongoDB documents
- **Middlewares**: Authentication (JWT), CORS, error handling
- **Utils**: Helper functions for common operations
- **Config**: Environment and database configuration

**Data Flow**: HTTP Request → Router → Middleware (Auth/CORS) → Controller → Model (DB) → Response

## Tech Stack & Dependencies

- **Express 5.x**: Web framework
- **Mongoose 9.x**: MongoDB ODM for schema management and validation
- **JWT (jsonwebtoken 9.x)**: Token-based authentication
- **bcryptjs 3.x**: Password hashing and comparison
- **CORS**: Cross-origin request handling
- **dotenv**: Environment variable management
- **Nodemon** (dev): Auto-restart server on file changes

## Development Workflows

### Setup & Running
```bash
npm install              # Install dependencies
npm start               # Start server with nodemon (dev mode)
npm test                # Run tests (currently placeholder)
```

**Important**: Always use `npm start` for local development - nodemon watches for changes and auto-restarts.

### Environment Configuration
Create `.env` file in project root with:
```
MONGO_URI=mongodb://...
JWT_SECRET=your-secret-key
PORT=3000
NODE_ENV=development
```

Load via `require('dotenv').config()` in config/index.js early in app startup.

## Code Patterns & Conventions

### Route Definition Pattern
Routes use Express Router with explicit HTTP methods and path validation:
```javascript
// routes/users.js
router.post('/', authenticateToken, userController.createUser);
router.get('/:id', userController.getUser);
```

### Authentication Pattern (JWT)
- Middleware `authenticateToken` validates JWT from Authorization header: `Bearer <token>`
- Store user ID in `req.user` for use in controllers
- Use bcryptjs for password: `bcryptjs.hashSync(password, 10)` and `bcryptjs.compareSync(password, hash)`

### Model Pattern
Define Mongoose schemas with validation in `models/`:
```javascript
const userSchema = new Schema({
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }
});
```

Always use `required` and type validation. Lean queries: `Model.find().lean()` for read-only operations (performance).

### Error Handling
- Use try-catch in controllers
- Pass errors to Express error middleware
- Respond with consistent JSON: `{ error: message, code: statusCode }`

## Project Structure Guidelines

```
src/
├── config/       # dotenv, database connection
├── models/       # Mongoose schemas (one file per entity)
├── controllers/  # Business logic, request handling
├── routes/       # Route definitions (one file per entity)
├── middlewares/  # Auth, CORS, error handling
└── utils/        # Helper functions (validation, formatters)
```

Keep each layer thin and focused. Move reusable logic to utils/ or create domain-specific helper files.

## Common Commands for Maintenance

| Task | Command |
|------|---------|
| Start dev server | `npm start` |
| Install new package | `npm install <package>` |
| Remove unused imports | Manual cleanup (no linter configured) |
| Check MongoDB connection | Use Mongoose `connection.once('open')` callback |

## Future Setup Notes

- **No testing framework configured yet** - plan to add Jest or Mocha
- **No linting configured** - consider ESLint for code consistency
- **No database seed script** - create scripts/seed.js for initial data
