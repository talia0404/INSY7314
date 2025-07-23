# 🛡️✍️ **Building SecureBlog – Project Foundations** 🛡️✍️

**Background**  
You’re creating **SecureBlog**, a modern, secure blogging platform where authors can post articles and readers can comment. Over this module, you’ll build it with the **MERN stack** (MongoDB, Express, React, Node.js), applying professional security and software‑engineering practices at every layer.

This week’s focus is on **project setup**: scaffolding a secure backend and frontend, enforcing coding hygiene, and preparing for end‑to‑end development. You’ll also reflect on why confidentiality and integrity matter in online publishing.

---

## Activity Steps

### 1. Understand the Problem  
- **Research (10 min):**  
  - Explore popular blogging platforms (e.g., **Medium**, **WordPress**, **Ghost**).  
  - Note their core features (rich‑text editor, comments, user profiles).  
  - Search for any security incidents (SQL injection, comment spam, XSS).  
- **Write (3–4 sentences):**  
  - In your own words, explain why data integrity and application security are critical for a public blog.

### 2. Build the Backend  
> **Follow Lab Guide  2.3: Backend API**  
1. **Initialize & Install:**  
   ```bash
   mkdir secureblog-backend
   cd secureblog-backend
   npm init -y
   npm install express cors helmet dotenv mongoose express-rate-limit
   npm install --save-dev nodemon


2. **Scaffold Structure:**

   ```
   secureblog-backend/
   ├── .env
   ├── package.json
   └── src/
       ├── server.js      # Entry point
       ├── app.js         # Express setup
       ├── config/db.js   # MongoDB connector
       ├── models/        # User.js, Post.js, Comment.js
       ├── routes/        # userRoutes.js, postRoutes.js, commentRoutes.js
       └── middleware/    # errorHandler.js, rateLimiter.js
   ```
3. In `app.js`, apply `helmet()`, `cors()`, `express.json()`, and add a GET `/` route that returns `"SecureBlog API running!"`.
4. In `server.js`, load `PORT` from `.env`, connect to MongoDB (Lab Guide  2.4), and start the server with nodemon.

### 3. Generate SSL Certificates

> **Follow Lab Guide  2.2: SSL Setup**

* Create your key & self‑signed cert for HTTPS:

  ```bash
  openssl genrsa -out privatekey.pem 2048
  openssl req -new -key privatekey.pem -out certrequest.csr \
    -subj "/C=ZA/ST=KZN/L=DBN/O=SecureBlog/OU=STUDENT/CN=localhost"
  openssl x509 -req -in certrequest.csr -signkey privatekey.pem \
    -out certificate.pem -days 365
  ```
* In `server.js`, switch to:

  ```js
  import https from 'https';
  import fs    from 'fs';
  const key  = fs.readFileSync('privatekey.pem');
  const cert = fs.readFileSync('certificate.pem');
  https.createServer({ key, cert }, app).listen(PORT, () => {
    console.log(`🔒 HTTPS server running on port ${PORT}`);
  });
  ```

### 4. Integrate MongoDB

> **Follow Lab Guide  2.4: Mongoose Integration**

* **User** model: `username`, `email`, `passwordHash`
* **Post** model: `title`, `content`, `authorId`, `createdAt`
* **Comment** model: `postId`, `authorId`, `text`, `createdAt`
* Implement CRUD endpoints under `/api/users`, `/api/posts`, `/api/comments`.

### 5. Build the Frontend

> **Follow Lab Guide 3.1: React Setup**

1. **Scaffold:**

   ```bash
   npm create vite@latest secureblog-frontend -- --template react
   cd secureblog-frontend
   npm install
   npm install axios react-router-dom
   ```
2. **Run:**

   ```bash
   npm run dev
   ```
3. In `src/App.jsx`, render:

   ```jsx
   function App() {
     return <h2>Welcome to SecureBlog 🔒✍️</h2>;
   }
   export default App;
   ```

### 6. Secure Project Hygiene

* Add a **`.gitignore`** in both repos containing:

  ```
  node_modules/
  .env
  dist/
  ```
* Create a **`.env`** in each project root (do **not** commit).
* Initialize Git in both folders and make at least two meaningful commits each.

### 7. Dependency Audit

* In each project root:

  ```bash
  npm audit
  npm audit fix
  ```
* Confirm no high/critical vulnerabilities remain.

### 8. JSON Endpoint & Data Display

1. **Backend:** Ensure `/api/posts` returns JSON array of posts.
2. **Frontend:** In `App.jsx`, fetch from `https://localhost:5000/api/posts` and display:

   * Total number of posts
   * Total number of comments (aggregate via `/api/comments`)

---

## For Class Discussion

1. **Written Reflection (3–4 sentences):**

   * Why is security essential for a public blogging platform?
   * Name one real‑world threat these apps face.
  
2. **Backend Repo:**

   * Working HTTPS Express API with `/api/users`, `/api/posts`, `/api/comments`.
   * README.md with setup & run instructions.
   * Two+ meaningful commits.
   * Correct `.gitignore` & `.env`.
  
3. **Frontend Repo:**

   * React app displaying “Welcome to SecureBlog” and post/comment counts.
   * README.md with usage instructions.
   * Two+ commits.
   * Correct `.gitignore` & `.env`.
  
4. **GitHub Classroom:**

   * Push both repos and claim your assignment link on Teams.

