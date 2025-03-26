# React Context API - Full Breakdown

## 1. UserContext.js

### 📂 File: `UserContext.js`

### 🎯 Purpose:
This file creates the context object, which acts as a global store for user data.

### 🛠️ Used For:
- Creating a centralized store for user authentication.
- Allowing other components to access user data without prop drilling.

### 📌 Use in Other Files:
- Imported in `UserContextProvider.jsx` to provide data.
- Used in `Login.jsx` and `Profile.jsx` to access user data.

### 📜 Code with Comments and Explanation:
```jsx
import React from 'react'; // Import React to use createContext

// Create a global context object for user data
const UserContext = React.createContext(); 

export default UserContext; // Export it so other components can access it
```

### 🔍 Explanation:
- `React.createContext()` → Creates a global state to manage user-related data.
- `UserContext` will hold user details and be shared across components.

---

## 2. UserContextProvider.js

### 📂 File: `UserContextProvider.jsx`

### 🎯 Purpose:
This component stores and provides the user state to all components inside it.

### 🛠️ Used For:
- Managing authentication state globally.
- Allowing components to access user data without passing props manually.

### 📌 Use in Other Files:
- Wraps the entire app in `App.jsx`.
- Used in `Login.jsx` and `Profile.jsx` to update or read user data.

### 📜 Code with Comments and Explanation:
```jsx
import React, { useState } from "react"; // Import React and useState hook
import UserContext from "./UserContext"; // Import the created UserContext

// Define a provider component that will store user data
const UserContextProvider = ({ children }) => {
    const [user, setUser] = useState(null); // State to hold the user data

    return (
        // Provide user data and update function to all child components
        <UserContext.Provider value={{ user, setUser }}> 
            {children} {/* Renders all children inside the provider */}
        </UserContext.Provider>
    );
};

export default UserContextProvider; // Export the provider
```

### 🔍 Explanation:
- `useState(null)` → Initializes user state as `null`.
- `UserContext.Provider` → Wraps all child components and makes `user`, `setUser` available.
- `{children}` → Ensures that all child components inside get access to context data.

---

## 3. Login.js

### 📂 File: `Login.jsx`

### 🎯 Purpose:
This component lets users log in and updates the user state in the context.

### 🛠️ Used For:
- Accepting user input (username & password).
- Updating the global user state on login.

### 📌 Use in Other Files:
- Uses `UserContext` to set user data.
- Used in `App.jsx` to render the login form.

### 📜 Code with Comments and Explanation:
```jsx
import React, { useState, useContext } from 'react'; // Import hooks
import UserContext from '../Context/UserContext'; // Import UserContext

function Login() { 
    const [username, setUserName] = useState(''); // State for username
    const [password, setPassword] = useState(''); // State for password

    const { setUser } = useContext(UserContext); // Get setUser function from context

    const handleSubmit = (e) => {
        e.preventDefault(); // Prevent page refresh
        setUser({ username, password }); // Update global user state
    };

    return (
        <div>
            <h2>Login</h2>
            <input
                type="text"
                value={username}
                onChange={(e) => setUserName(e.target.value)} // Update username state
                placeholder='username'
            />
            <input
                type="password"
                value={password}
                onChange={(e) => setPassword(e.target.value)} // Update password state
                placeholder='password'
            />
            <button onClick={handleSubmit}>Submit</button> {/* Calls handleSubmit */}
        </div>
    );
}

export default Login; // Export the component
```

### 🔍 Explanation:
- `useContext(UserContext)` → Gives access to `setUser`, so the login updates global state.
- `handleSubmit()` → Prevents page refresh and updates user context with `username`/`password`.

---

## 4. Profile.js

### 📂 File: `Profile.jsx`

### 🎯 Purpose:
This component displays the logged-in user’s name or asks them to log in.

### 🛠️ Used For:
- Checking if a user is logged in.
- Displaying the username after login.

### 📌 Use in Other Files:
- Uses `UserContext` to get user data.
- Displayed in `App.jsx`.

### 📜 Code with Comments and Explanation:
```jsx
import React, { useContext } from 'react'; // Import React and useContext
import UserContext from '../Context/UserContext'; // Import UserContext

function Profile() { 
    const { user } = useContext(UserContext); // Get user data from context

    if (!user) { // If no user is logged in
        return <div>Please Login</div>;
    }

    return <div>Welcome {user.username}</div>; // Display the logged-in username
}

export default Profile; // Export the component
```

### 🔍 Explanation:
- `useContext(UserContext)` → Accesses the user state.
- If `user` is `null` → It shows "Please Login".
- Else → It displays "Welcome {username}".

---

## 5. App.js

### 📂 File: `App.jsx`

### 🎯 Purpose:
This file wraps everything inside the `UserContextProvider`.

### 🛠️ Used For:
- Structuring the app layout.
- Ensuring `Login` and `Profile` can access global user data.

### 📜 Code with Comments and Explanation:
```jsx
import './App.css'; // Import styles
import Login from './Components/Login'; // Import Login component
import Profile from './Components/Profile'; // Import Profile component
import UserContextProvider from './Context/UserContextProvider'; // Import Provider

function App() {  
    return (
        <>
            <UserContextProvider> {/* Wraps components to provide global context */}
                <h1>React Context API</h1>
                <Login /> {/* Render Login component */}
                <Profile /> {/* Render Profile component */}
            </UserContextProvider>
        </>
    );
}

export default App; // Export App component
```

---

## 6. Diagram of How Everything Works
```
+-------------------------------------------------+
|                App.jsx                          |
|  +-------------------------------------------+  |
|  |  UserContextProvider                     |  |
|  |  +-----------+    +-----------------+    |  |
|  |  | Login.jsx | →  | Updates Context |    |  |
|  |  +-----------+    +-----------------+    |  |
|  |                                         |  |
|  |  +------------+    +-----------------+  |  |
|  |  | Profile.jsx | →  | Uses Context    |  |  |
|  |  +------------+    +-----------------+  |  |
|  +-------------------------------------------+  |
+-------------------------------------------------+