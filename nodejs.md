# ALL THE CODE SNIPPETS for BACKEND DEVELOPMENT with NODE

### Creating the server

```javascript
import express from "express";
import dotenv from "dotenv";
import connectDB from "./config/db.js"; // Import database connection function
import errorHandler from "./middlewares/errorHandler.js"; // Custom error handler
import userRoutes from "./routes/userRoutes.js"; // Example route file
import cors from "cors";

// Load environment variables
dotenv.config();

// Constants
const PORT = process.env.PORT || 5000;

// Connect to MongoDB
connectDB();

// Enable cors
app.use(cors);

// Initialize the app
const app = express();

// Middlewares
app.use(express.json()); // Parse incoming JSON requests

// Routes
app.use("/api/users", userRoutes); // Example route
// Add more routes as needed, e.g.:
// app.use("/api/products", productRoutes);

// 404 Middleware for undefined routes
app.all("*", (req, res) => {
  res.status(404).json({
    status: "failed",
    message: "Resource not found",
  });
});

// Global Error Handler
app.use(errorHandler);

// Start the server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Connecting to MongoDB

```javascript
import mongoose from "mongoose";
import dotenv from "dotenv";

// Config database
dotenv.config();

const connectDB = () => {
  mongoose
    .connect(process.env.MONGO_URI)
    .then(() => console.log("MongoDB Connected"))
    .catch((err) => console.log(err));
};

export default connectDB;
```

## Middlewares & Handlers

### Error Handler Middleware

```javascript
const errorHandler = (error, req, res, next) => {
  if (error) {
    // Check if error has a message
    if (error.message) {
      return res.status(400).json({
        status: "failed", // Ensure 'failed' is a string
        error: error.message,
      });
    } else {
      return res.status(400).json({
        status: "failed",
        error: error,
      });
    }
  }

  // Pass control to the next middleware
  next();
};

export default errorHandler;
```

### Email Handler middleware

```javascript
import nodemailer from "nodemailer";

const emailHandler = async (to, subject, text, html) => {
  var transport = nodemailer.createTransport({
    host: "sandbox.smtp.mailtrap.io",
    port: 2525,
    auth: {
      user: " ", // username
      pass: " ", // userpassword
    },
  });

  transport.sendMail({
    to: to,
    from: "noreply@mailtrap.io",
    subject: subject,
    text: text,
    html: html,
  });

  res.status(200).json({ status: "Reset Email sent" });
};

export default emailHandler;
```

### JWT Handler

```javascript
import jwt from "jsonwebtoken";

const jwtManager = (user) => {
  // Generate a token
  const accessToken = jwt.sign(
    {
      id: user._id,
      email: user.email,
      balance: user.balance,
    },
    process.env.JWT_SECRET
  );

  return accessToken;
};

export default jwtManager;
```

## Controllers

### Register Controller

```javascript

```

### loginController

```javasript

```

### forgotPassword Controller

```javascript

```
