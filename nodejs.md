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

### Email Handler Variant 2

```javascript
import express from "express";
import sendEmail from "../services/emailService.js";

const router = express.Router();

router.post("/send", async (req, res) => {
  const { to, subject, text, attachments } = req.body;

  if (!to || !subject || !text) {
    return res
      .status(400)
      .json({ status: "failed", message: "Missing fields" });
  }

  const result = await sendEmail({ to, subject, text, attachments });

  if (result.success) {
    res.json({ status: "success", message: "Email sent" });
  } else {
    res.status(500).json({ status: "failed", message: result.message });
  }
});

export default router;
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

### Auth Middleware

```javascript
import dotenv from "dotenv";
import jwt from "jsonwebtoken";

// configurations
dotenv.config();

const authHandler = async (req, res, next) => {
  try {
    // Extract the token from the Authorization header
    const authHeader = req.headers.authorization;

    if (!authHeader || !authHeader.startsWith("Bearer ")) {
      return res.status(401).json({ message: "Unauthorized" });
    }

    const token = authHeader.split(" ")[1];

    // Verify the token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);

    // Attach user information to the request object for downstream use
    req.user = decoded;

    // Proceed to the next middleware or route handler
    next();
  } catch (error) {
    // Handle errors (e.g., invalid token, expired token)
    res.status(401).json({ message: "Unauthorized" });
  }
};

export default authHandler;
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
// Forgot password
export const forgotPassword = async (req, res) => {
  // Logic to send a password reset link to the user's email
  const { email } = req.body;

  if (!email) throw new Error("Please provide an email");

  const getUser = await User.findOne({ email });

  if (!getUser) throw new Error("User does not exist");

  const resetCode = Math.floor(100000 + Math.random() * 900000);

  await User.updateOne(
    {
      email,
    },
    { reset_code: resetCode },
    {
      runValidators: true,
    }
  );

  await emailHandler(getUser.email);
};
```

### resetPassword Handler

```javascript
export const resetPassword = async (req, res) => {
  const { email, new_password, reset_code } = req.body;

  // validations
  if (!email) throw new Error("Please provide an email");
  if (!new_password) throw new Error("Please provide a new password");
  if (!reset_code) throw new Error("Please provide a reset code");

  const userExist = await User.findOne({ email });

  if (!userExist) throw new Error("User does not exist");

  if (reset_code !== userExist.reset_code)
    throw new Error("Invalid reset code");

  const hashedPassword = await bcrypt.hash(new_password, 12);

  await User.updateOne(
    {
      email,
    },
    {
      password: hashedPassword,
      reset_code: null,
    },
    {
      runValidators: true,
    }
  );

  await emailHandler(userExist.email);
  // Generate JWT token for the logged-in user
  const token = jwtManager(userExist);

  res.status(200).json({
    status: "Password Reset Successfully",
  });
};
```
