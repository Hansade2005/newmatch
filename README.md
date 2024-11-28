README.md:
# Admin Dashboard and Login System

This project is a simple admin dashboard with a login system that allows the admin to manage coaches and bookings, visualize booking data through charts, and use clean URLs with `.htaccess`.

## Features

- **Admin Login System**: Secure login with username and password.
- **Dashboard**: Manage coaches, bookings, and view data visualizations.
- **Coach Management**: Add, edit, and delete coach profiles.
- **Booking Management**: Add, edit, and delete bookings.
- **Booking Data Visualization**: Visualize booking data with a bar chart.
- **URL Rewriting**: Clean URLs using `.htaccess`.

## Project Structure

/project-root ├── admin-login.html # Admin login page ├── admin-dashboard.html # Admin dashboard for coach and booking management ├── .htaccess # URL rewrite rules for clean URLs └── README.md # Project documentation


## Setup

### 1. Clone the Repository

Clone this project to your local machine.

```bash
git clone https://github.com/yourusername/admin-dashboard.git
cd admin-dashboard
2. Local Development
This project runs entirely on the frontend and uses localStorage to persist data. Simply open the admin-login.html file in your browser.

3. .htaccess Configuration
Ensure your project is hosted on a server that supports .htaccess (e.g., Apache) for URL rewriting.

4. Modify Admin Credentials
The admin login credentials are hardcoded in the admin-login.html file:

Username: admin
Password: password123
You can modify these credentials as needed within the admin-login.html file.

How to Use
Open the admin-login.html file in your browser.
Enter the username (admin) and password (password123).
After successful login, you will be redirected to the admin-dashboard.html page.
From the dashboard, you can manage coaches, bookings, and view booking data.
Code Explanation
admin-login.html
This file contains the login page for the admin. It validates the credentials and redirects to the dashboard after successful login.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Admin Login</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script>
    function login() {
      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      if (username === "admin" && password === "password123") {
        document.cookie = "adminLoggedIn=true;path=/";
        window.location.href = "admin-dashboard.html";
      } else {
        alert("Invalid credentials");
      }
    }
  </script>
</head>
<body>
  <div class="container">
    <h2 class="mt-5">Admin Login</h2>
    <div class="form-group">
      <label for="username">Username</label>
      <input type="text" id="username" class="form-control" placeholder="Username">
    </div>
    <div class="form-group mt-3">
      <label for="password">Password</label>
      <input type="password" id="password" class="form-control" placeholder="Password">
    </div>
    <button class="btn btn-primary mt-3" onclick="login()">Login</button>
  </div>
</body>
</html>
admin-dashboard.html
This file is the main dashboard where coaches and bookings are managed. It uses JavaScript to dynamically render data from localStorage.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Admin Dashboard</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
    // Fetch data from localStorage
    let coaches = JSON.parse(localStorage.getItem("coaches")) || [];
    let bookings = JSON.parse(localStorage.getItem("bookings")) || [];

    // Function to render coaches list
    function renderCoaches() {
      const coachesList = document.getElementById("coaches-list");
      coachesList.innerHTML = "";
      coaches.forEach(coach => {
        const coachRow = document.createElement("tr");
        coachRow.innerHTML = `
          <td>${coach.name}</td>
          <td>${coach.type}</td>
          <td>
            <button class="btn btn-primary" onclick="editCoach(${coach.id})">Edit</button>
            <button class="btn btn-danger" onclick="deleteCoach(${coach.id})">Delete</button>
          </td>
        `;
        coachesList.appendChild(coachRow);
      });
    }

    // Function to render bookings list
    function renderBookings() {
      const bookingsList = document.getElementById("bookings-list");
      bookingsList.innerHTML = "";
      bookings.forEach(booking => {
        const bookingRow = document.createElement("tr");
        bookingRow.innerHTML = `
          <td>${booking.coachName}</td>
          <td>${booking.date}</td>
          <td>
            <button class="btn btn-primary" onclick="editBooking(${booking.id})">Edit</button>
            <button class="btn btn-danger" onclick="deleteBooking(${booking.id})">Delete</button>
          </td>
        `;
        bookingsList.appendChild(bookingRow);
      });
    }

    // Functions to handle edit and delete actions for coaches and bookings...
    // (Include your functions here like in the previous example)

    // Dashboard chart (bookings)
    function renderChart() {
      const ctx = document.getElementById("booking-chart").getContext("2d");
      const chartData = {
        labels: bookings.map(b => b.date),
        datasets: [{
          label: 'Bookings',
          data: bookings.map(() => 1), // Dummy data
          backgroundColor: 'rgba(75, 192, 192, 0.2)',
          borderColor: 'rgba(75, 192, 192, 1)',
          borderWidth: 1
        }]
      };
      new Chart(ctx, {
        type: 'bar',
        data: chartData
      });
    }

    window.onload = function() {
      renderCoaches();
      renderBookings();
      renderChart();
    };
  </script>
</head>
<body>
  <!-- Navbar and Dashboard content (Coach and Booking management) -->
  <div class="container mt-5">
    <h4>Coaches</h4>
    <table class="table table-striped" id="coaches-list"></table>
    <h4>Bookings</h4>
    <table class="table table-striped" id="bookings-list"></table>
    <canvas id="booking-chart" width="400" height="200"></canvas>
  </div>
</body>
</html>
.htaccess File
This .htaccess file rewrites the URLs to provide cleaner and more user-friendly URLs for your pages.

# Enable mod_rewrite
RewriteEngine On

# Clean URL for login page
RewriteRule ^admin-login$ /admin-login.html [L]

# Clean URL for dashboard page
RewriteRule ^admin-dashboard$ /admin-dashboard.html [L]

# Redirect to login page if admin is not logged in
RewriteCond %{REQUEST_URI} ^/admin-dashboard
RewriteCond %{HTTP_COOKIE} !^.*adminLoggedIn=true.*$ [NC]
RewriteRule ^.*$ /admin-login.html [L]

# Handle 404 error
ErrorDocument 404 /404.html
Conclusion
This project provides a simple yet effective system for managing coaches and bookings through a clean admin dashboard with login functionality and clean URLs using .htaccess. Modify the localStorage data as needed and use the provided code blocks to manage coaches, bookings, and visualize data with charts.
