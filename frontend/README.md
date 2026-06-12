# Uber  - Frontend

A React + Vite application for an Uber-like ride-sharing platform with separate interfaces for Users and Captains (Drivers).

## Getting Started

### Prerequisites
- Node.js (v16 or higher)
- npm or yarn

### Installation

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build
```

## Application Navigation Guide

### Home Page - Entry Point
**Route:** `/`

The application starts with a welcome screen featuring:
- Uber branding and logo
- Background image showcase
- "Get Started" button that redirects to User Login

---

## User Interface

### User Login
**Route:** `/login`

**How to Access:**
1. Start at the Home page (`/`)
2. Click the "Continue" button

**Login Form Fields:**
- **Email:** Enter your email address (e.g., `user@example.com`)
- **Password:** Enter your password

**Actions:**
- After entering credentials, click the **Login** button to authenticate
- First time user? Click the **"Don't have an account? Sign up"** link to go to signup

### User Signup
**Route:** `/signup`

**How to Access:**
1. From the Home page (`/`), click "Continue"
2. On the User Login page, click **"Don't have an account? Sign up"**
3. Or navigate directly to `/signup`

**Signup Form Fields:**
- **Full Name:** Enter your complete name
- **Email:** Enter a valid email address
- **Password:** Create a strong password
- **Confirm Password:** Re-enter your password to confirm

**Actions:**
- Complete all fields and click the **Sign Up** button to create a new account
- Already have an account? Click **"Already have an account? Login"** to return to login

---

## Captain (Driver) Interface

### Captain Login
**Route:** `/captain-login`

**How to Access:**
1. From the Home page (`/`), you can navigate to captain login
2. Or use the URL directly: `/captain-login`

**Login Form Fields:**
- **Email:** Enter your captain/driver email (e.g., `captain@example.com`)
- **Password:** Enter your password

**Actions:**
- Enter your credentials and click the **Login** button to authenticate as a captain
- New captain? Click the **"Don't have an account? Sign up"** link to register

### Captain Signup
**Route:** `/captain-signup`

**How to Access:**
1. Navigate to Captain Login page (`/captain-login`)
2. Click the **"Don't have an account? Sign up"** link
3. Or navigate directly to `/captain-signup`

**Signup Form Fields:**
- **Full Name:** Enter your complete name
- **Email:** Enter a valid email address
- **Vehicle Type:** Select your vehicle type (e.g., Economy, Premium)
- **Vehicle Plate:** Enter your vehicle's license plate
- **Password:** Create a strong password
- **Confirm Password:** Re-enter your password to confirm

**Actions:**
- Complete all required fields and click the **Sign Up** button to register as a captain
- Already registered? Click **"Already have an account? Login"** to return to captain login

---

## Route Map

| Route | Component | Purpose |
|-------|-----------|---------|
| `/` | Start | Welcome/Home page |
| `/login` | UserLogin | User authentication |
| `/signup` | UserSignup | User registration |
| `/captain-login` | CaptainLogin | Captain/Driver authentication |
| `/captain-signup` | CaptainSignup | Captain/Driver registration |
| `/home` | Home | Dashboard (after login) |

---

## User Flow Diagram

```
Home (/)
    ├── [Continue] → User Login (/login)
    │               ├── [Login] → Home Dashboard
    │               └── [Sign up link] → User Signup (/signup)
    │                                     ├── [Sign Up] → Home Dashboard
    │                                     └── [Login link] → User Login (/login)
    │
    └── [Captain] → Captain Login (/captain-login)
                    ├── [Login] → Captain Dashboard
                    └── [Sign up link] → Captain Signup (/captain-signup)
                                         ├── [Sign Up] → Captain Dashboard
                                         └── [Login link] → Captain Login (/captain-login)
```

---

## Technology Stack

- **React** - UI library
- **Vite** - Build tool and dev server
- **React Router** - Client-side routing
- **Tailwind CSS** - Styling
- **ESLint** - Code quality

## Project Structure

```
src/
├── pages/
│   ├── Start.jsx          # Home/Welcome page
│   ├── UserLogin.jsx      # User login form
│   ├── UserSignup.jsx     # User registration form
│   ├── CaptainLogin.jsx   # Captain login form
│   ├── CaptainSignup.jsx  # Captain registration form
│   └── Home.jsx           # Main dashboard
├── context/
│   └── UserContext.jsx    # Global user state management
├── App.jsx                # Main app component with routes
└── main.jsx               # Entry point
```

---

## Features

- **Dual Role System:** Separate login/signup for Users and Captains
- **Responsive Design:** Mobile-first approach with Tailwind CSS
- **Context API:** Global state management for user data
- **Form Validation:** Email and password validation on forms
- **Dynamic Routing:** Clean navigation between different interfaces

---

## Development

Start the development server to see changes in real-time:

```bash
npm run dev
```

Navigate to `http://localhost:5173` in your browser.

---

## Build for Production

```bash
npm run build
```

This creates an optimized production build in the `dist/` directory.

---

## Support

For issues or questions about the application flow, refer to the route structure above or check individual component files in the `src/pages/` directory.
