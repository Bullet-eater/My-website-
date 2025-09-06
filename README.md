
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Freelance Platform</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f9;
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 900px;
      margin: 0 auto;
      padding: 20px;
    }

    header {
      text-align: center;
      margin-bottom: 30px;
    }

    h1 {
      font-size: 3em;
      color: #333;
    }

    .login, .dashboard {
      background-color: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    input {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 5px;
    }

    button {
      width: 100%;
      padding: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 16px;
    }

    button:hover {
      background-color: #0056b3;
    }

    #login-error {
      color: red;
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>Freelance Platform</h1>
    </header>

    <!-- Login Form -->
    <section class="login">
      <h2>Login</h2>
      <form id="login-form">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" required>
        
        <label for="password">Password</label>
        <input type="password" id="password" name="password" required>
        
        <button type="submit">Log In</button>
      </form>
      <p id="login-error"></p>
    </section>

    <!-- Main Content Section (Dashboard) -->
    <section class="dashboard" style="display: none;">
      <h2>Welcome to the Freelance Platform</h2>
      <p>Explore freelance jobs and find opportunities here!</p>
      <button id="logout-button">Log Out</button>
    </section>
  </div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js"></script>

  <script>
    // Your Firebase configuration (replace this with your own Firebase project configuration)
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
      appId: "YOUR_APP_ID",
      measurementId: "YOUR_MEASUREMENT_ID"
    };

    // Initialize Firebase
    const app = firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const firestore = firebase.firestore();

    // Login Form
    document.getElementById("login-form").addEventListener("submit", function(event) {
      event.preventDefault();
      
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;
      
      auth.signInWithEmailAndPassword(email, password)
        .then((userCredential) => {
          const user = userCredential.user;
          console.log("User logged in:", user.email);
          document.querySelector(".login").style.display = "none";
          document.querySelector(".dashboard").style.display = "block";
        })
        .catch((error) => {
          const errorMessage = error.message;
          document.getElementById("login-error").textContent = errorMessage;
        });
    });

    // Logout functionality
    document.getElementById("logout-button").addEventListener("click", function() {
      auth.signOut().then(() => {
        console.log("User logged out");
        document.querySelector(".dashboard").style.display = "none";
        document.querySelector(".login").style.display = "block";
      }).catch((error) => {
        console.error("Error signing out:", error);
      });
    });

    // Check for logged-in user
    auth.onAuthStateChanged((user) => {
      if (user) {
        console.log("User is logged in:", user.email);
        document.querySelector(".login").style.display = "none";
        document.querySelector(".dashboard").style.display = "block";
      } else {
        console.log("No user is logged in.");
        document.querySelector(".login").style.display = "block";
        document.querySelector(".dashboard").style.display = "none";
      }
    });
  </script>
</body>
</html>
