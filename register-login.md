# **standard steps for user registration and login**

---

## 📝 User Registration Steps

1. **Receive User Input**  
   Collect data like username, email, and password from the client (usually via a form).

2. **Validate Input**  
   Ensure all required fields are provided and meet format requirements (e.g., valid email, strong password).

3. **Check for Existing User**  
   Search the database to see if a user with the same email or username already exists.

4. **Hash the Password**  
   Use a hashing algorithm (like bcrypt) to securely encrypt the password before storing it.

5. **Create and Save the User**  
   Store the new user record in the database with the hashed password.

6. **Generate JWT Token**  
   Create a JSON Web Token to identify the user in future requests.

7. **Set Cookie (Optional)**  
   Store the token in an HTTP-only cookie for secure client-side access.

8. **Send Response**  
   Return a success message and optionally the token or user data to the client.

---

## 🔐 User Login Steps

1. **Receive Login Credentials**  
   Collect email and password from the client.

2. **Validate Input**  
   Ensure both fields are provided and properly formatted.

3. **Find User in Database**  
   Look up the user by email.

4. **Compare Passwords**  
   Use bcrypt to compare the provided password with the stored hashed password.

5. **Generate JWT Token**  
   If the password matches, create a new token for the session.

6. **Set Cookie (Optional)**  
   Store the token in a secure cookie to maintain login state.

7. **Send Response**  
   Return a success message and optionally the token or user data.

---

## 🍪 Cookies vs 🗂️ Sessions (Quick Overview)

- **Cookies**:  
  - Stored on the client  
  - Can hold JWT tokens  
  - Often used for stateless authentication

- **Sessions**:  
  - Stored on the server  
  - Identified by a session ID in a cookie  
  - Useful for storing user state securely

---

- **Jeff Arribion - codnify**
