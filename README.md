# BlogApp

## High-level Overview:

### Requirements:

1. **Functional Requirements**:
   - **User Authentication**: Register, login, and logout.
   - **CRUD for Blog Posts**: Users should be able to create, read, update, and delete their blog posts.
   - **Comments**: Users should be able to comment on blog posts.
   - **Likes/Reactions**: Users should be able to like or react to blog posts.
   - **Search**: Ability to search for blogs by title or content.
   - **Categories/Tags**: Categorize or tag posts.
   - **User Profiles**: Display user info and their posts.
   - **Admin Dashboard**: Moderation tools, analytics, and user management.
   
2. **Non-functional Requirements**:
   - **Performance**: The platform should load quickly and be responsive.
   - **Security**: Secure user data, prevent SQL injection, XSS attacks, etc.
   - **Scalability**: Should be able to handle an increase in the number of users and blog posts.

3. **Technical Requirements**:
   - **Backend**: Node.js with Express.js framework.
   - **Frontend**: React for UI components.
   - **Database**: MongoDB for storing data.
   - **State Management**: Use Context API or Redux.
   - **Styles**: CSS-in-JS libraries like Styled Components, or frameworks like Bootstrap or Material-UI.

### Development Breakdown:

1. **Backend Setup**:
   - Initialize a Node.js project.
   - Set up Express.js and connect it to MongoDB using Mongoose.
   - Define Mongoose schemas for users, posts, comments, etc.
   - Create API routes for CRUD operations.

2. **User Authentication**:
   - Set up Passport.js or another authentication middleware for Express.
   - Create routes for registration, login, and logout.
   - Use JWT (JSON Web Tokens) or sessions for maintaining user sessions.

3. **Frontend Setup**:
   - Initialize a React project (using Create React App or another starter).
   - Set up routing using React Router.
   - Create layout components (Navbar, Footer, Main Content).
   
4. **Integrate Backend with Frontend**:
   - Use Axios or Fetch to make HTTP requests from the React frontend to the Express backend.
   - Implement state management using Context API or Redux to manage user sessions and other global states.

5. **UI for Blog Posts**:
   - Create post components (List view, Single post view).
   - Implement forms for creating and editing posts.

6. **UI for User Profiles and Authentication**:
   - Create components for the registration and login forms.
   - Design user profile pages with their posts.

7. **UI for Comments and Reactions**:
   - Create components for displaying comments.
   - Implement a UI for adding reactions.

8. **Admin Dashboard**:
   - Design an admin dashboard with user management tools.
   - Display analytics or statistics about user activity, popular posts, etc.

9. **Testing**:
   - Use tools like Jest and React Testing Library for unit and integration tests.
   - Test backend routes using tools like Postman or Insomnia.

10. **Deployment**:
   - Deploy the backend to platforms like Heroku or DigitalOcean.
   - Deploy the frontend to platforms like Netlify or Vercel.
   - Set up the domain name and SSL for security.



## Initial setup, API design, and MongoDB schema design:

### **1. Initial Setup**

**Backend Setup (Express.js & Node.js)**:
   - Initialize a new Node project: `npm init`.
   - Install necessary packages: `npm install express mongoose cors`.
   - Optionally, for development: `npm install nodemon` (helps in auto-reloading the server upon changes).

**Frontend Setup (React)**:
   - Use Create React App (CRA) for simplicity: `npx create-react-app blog-frontend`.
   - Install Axios for HTTP requests: `npm install axios`.

### **2. MongoDB Schema Design**

Use `mongoose` to define schemas. 

**User Schema**:
```javascript
const userSchema = new mongoose.Schema({
   username: { type: String, required: true, unique: true },
   email: { type: String, required: true, unique: true },
   password: { type: String, required: true },
   posts: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Post' }],
   comments: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Comment' }],
});
```

**Post Schema**:
```javascript
const postSchema = new mongoose.Schema({
   title: { type: String, required: true },
   content: { type: String, required: true },
   author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
   comments: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Comment' }],
   likes: Number,
});
```

**Comment Schema**:
```javascript
const commentSchema = new mongoose.Schema({
   content: { type: String, required: true },
   author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
   post: { type: mongoose.Schema.Types.ObjectId, ref: 'Post' },
});
```

### **3. API Structure**

**User Routes**:
   - `POST /users/register`: Register a new user.
   - `POST /users/login`: Authenticate and log in a user.
   - `GET /users/:userId`: Retrieve a user's profile.

**Post Routes**:
   - `GET /posts`: Retrieve all posts.
   - `GET /posts/:postId`: Retrieve a single post.
   - `POST /posts`: Create a new post.
   - `PUT /posts/:postId`: Update a specific post.
   - `DELETE /posts/:postId`: Delete a specific post.

**Comment Routes**:
   - `GET /comments/post/:postId`: Get all comments for a post.
   - `POST /comments/post/:postId`: Create a comment on a post.
   - `DELETE /comments/:commentId`: Delete a specific comment.

### **4. Starting Development**

**Backend**:

   - Initialize the express server and connect to MongoDB using `mongoose`.
   - Create routes and controllers for the defined API structure.
   - Use `bcrypt` for hashing passwords and `jsonwebtoken` for JWT-based authentication.
   - Protect routes that require authentication using middleware.

**Frontend**:

   - Create basic layout components (Header, Footer).
   - Create pages: Home (lists all posts), Post Detail, User Profile, Login, Register.
   - Use Axios for API calls: fetching posts, user authentication, etc.
   - Store user's token in the local storage or context for maintaining the session.
   - Redirect unauthorized users from protected routes/pages.

### **5. Security Considerations**:

   - Always hash passwords before storing them in the database.
   - Use environment variables to store sensitive data (like the MongoDB URI, JWT secret).
   - Enable CORS only for trusted domains.
   - Validate and sanitize all inputs to prevent MongoDB injection.
   - Use HTTPS in production.

### **6. Testing & Deployment**:

   - Write unit and integration tests using libraries like `Jest`, `Mocha`, or `Chai`.
   - Test the backend using Postman or similar tools.
   - For deployment, consider platforms like Heroku (backend) and Netlify or Vercel (frontend). Remember to set up environment variables appropriately on these platforms.
   - Ensure you connect to a production-ready MongoDB instance for the deployed app.

 

##  primary components for a blog platform UML:

### 1. **Use Case Diagram**:

**Actors**:
- Guest (unauthenticated user)
- Registered User
- Admin
- System

**Use Cases**:
- Guest: View Blog Posts, Register, Login
- Registered User: Create Blog Post, Edit Their Blog Post, Delete Their Blog Post, Comment on Blog Posts, Like Blog Posts, Logout
- Admin: Delete Any Blog Post, Delete Any Comment, Manage Users
- System: Notify User (e.g., confirmation emails, alerts)

### 2. **Class Diagram**:

**Classes**:
- **User**
  - Attributes: `userID`, `username`, `email`, `password`, `posts[]`, `comments[]`
  - Methods: `createPost()`, `editPost()`, `deletePost()`, `commentOnPost()`

- **Post**
  - Attributes: `postID`, `author`, `title`, `content`, `comments[]`, `likes`
  - Methods: `addComment()`, `removeComment()`, `like()`, `edit()`

- **Comment**
  - Attributes: `commentID`, `author`, `content`, `parentPost`
  - Methods: `edit()`, `delete()`

### 3. **Sequence Diagram**:

For the scenario "User creates a blog post":

1. User chooses "Create Post".
2. System presents "Post Creation" form.
3. User enters `title` and `content`.
4. User submits the form.
5. System validates the data.
6. System creates a new `Post` object and associates it with the `User`.
7. System confirms post creation to the user.

### 4. **Activity Diagram**:

For the activity "Creating a Blog Post":

1. Start
2. Click on "Create Post".
3. Fill out the post form with `title` and `content`.
4. Submit the form.
5. Check if data is valid.
   - If **No**: Go back to the form and show error.
   - If **Yes**: Proceed.
6. Save post to the database.
7. Confirm post creation to the user.
8. End.

