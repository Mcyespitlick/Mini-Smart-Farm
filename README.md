# 🌱 Mini Smart Farm

A web-based dashboard for monitoring and managing a small-scale smart farm. Built as a university project (PSU), this system integrates IoT hardware, an MQTT broker, a backend database, and a React frontend to provide real-time visibility into farm conditions.

---

## Table of Contents

- Overview
- Tech Stack
- Project Structure
- Prerequisites
- Getting Started
  - 1. Frontend (React App)
  - 2. MQTT Broker (Eclipse Mosquitto)
  - 3. Backend (Node-RED + MySQL)
  - 4. Database (XAMPP + MySQL)
- Environment & Execution Policy Notes
- Contributing

---

## Overview

Mini Smart Farm is an IoT-connected web application designed to monitor and display farm data from physical sensors. It uses **Meshtastic** devices for wireless sensor communication, **Eclipse Mosquitto** as an MQTT message broker, **Node-RED** for backend data flow, **MySQL** (via XAMPP) for data storage, and a **React + TypeScript** frontend to visualize everything in a dashboard.

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React 18 + TypeScript | UI framework |
| Vite | Build tool & dev server |
| Tailwind CSS v4 | Styling |
| MUI (Material UI) v7 | Component library |
| Radix UI / shadcn-ui | Accessible UI primitives |
| Recharts | Data visualization / charts |
| React Router v7 | Client-side routing |
| MQTT.js | MQTT client for browser |
| Motion | Animations |

### Backend / Infrastructure
| Technology | Purpose |
|---|---|
| Eclipse Mosquitto | MQTT broker (IoT message bus) |
| Node-RED | Visual backend / data pipeline |
| MySQL (XAMPP) | Database |
| Python 3 | Sensor scripting / data processing |
| Meshtastic | Long-range wireless sensor mesh |

---

## Project Structure

```
Mini-Smart-Farm/
├── src/                        # React frontend source
├── public/
│   └── chickenImages/          # Static farm images
├── Backend - Database connection/  # Node-RED flows & DB logic
├── PSU Project Website/        # Legacy or static site version
├── guidelines/                 # Project guidelines
├── index.html                  # App entry point
├── vite.config.ts              # Vite configuration
├── package.json                # Node dependencies
├── Some instructions.txt       # Setup notes for tools
└── Connecting react to SQL.txt # Notes on React ↔ SQL integration
```

---

## Prerequisites

Make sure the following are installed before getting started:

- [Node.js](https://nodejs.org/) (v18+)
- [Python 3.14+](https://www.python.org/)
- [Eclipse Mosquitto](https://mosquitto.org/download/) (MQTT broker)
- [XAMPP](https://www.apachefriends.org/) (for MySQL + phpMyAdmin)
- [Node-RED](https://nodered.org/) (installed globally via npm)

---

## Getting Started

### Note: 
There are two text documents, "Connecting react to SQL.txt" and "Some Instructions.txt" which has some futher details on the setup of the project.

### 1. Frontend (React App)

```bash
# Install dependencies
npm install

# Start the development server
npm run dev

# Build for production / deployment
npm run build
```

The dev server runs at `http://localhost:5173` by default.

---

### 2. MQTT Broker (Eclipse Mosquitto)

Mosquitto acts as the message broker between Meshtastic sensor devices and the rest of the system.

**Installation & Setup:**

1. Download and install [Eclipse Mosquitto](https://mosquitto.org/download/).
2. Add the Mosquitto install directory to your system `PATH` environment variable (e.g. `C:\Program Files\Mosquitto`).
3. Verify the installation by running in a terminal:
   ```bash
   mosquitto -v
   ```
   > If you get a "socket already in use" error, Mosquitto is already running — this is fine.

4. Optionally, configure Mosquitto to run automatically via **Windows Services**.

---

### 3. Backend (Node-RED + MySQL)

Node-RED handles data flow between the MQTT broker and the database.

**One-time install:**

```powershell
# On Windows, a temporary execution policy bypass may be needed:
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process

# Install Node-RED globally
npm install -g --unsafe-perm node-red
```

**Run Node-RED:**

```bash
node-red
```

Then open the URL shown in the terminal (e.g. `http://127.0.0.1:1880/`) in a browser.

**Install the MySQL node in Node-RED:**

1. Click the ☰ menu (top right) → **Manage Palette**
2. Go to the **Install** tab
3. Search for `node-red-node-mysql` and install the top result

> For a more permanent fix to the execution policy (so you don't need the bypass every time):
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

---

### 4. Database (XAMPP + MySQL)

The project uses MySQL via XAMPP for data persistence.

**Initial Setup:**

1. Open the XAMPP Shell and run:
   ```sql
   mysql -u root
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'PSUMySQL';
   FLUSH PRIVILEGES;
   EXIT;
   ```

2. Navigate to the phpMyAdmin config file:
   ```
   <xampp_install_dir>\phpMyAdmin\config.inc.php
   ```

3. Update the following lines:
   ```php
   $cfg['Servers'][$i]['user'] = 'root';
   $cfg['Servers'][$i]['password'] = 'PSUMySql';
   $cfg['Servers'][$i]['controluser'] = 'pma';
   $cfg['Servers'][$i]['controlpass'] = 'PSUMySql';
   ```

4. *(Optional)* Create a `pma` control user for full phpMyAdmin functionality:
   ```sql
   CREATE USER 'pma'@'localhost' IDENTIFIED BY 'PSUMySql';
   GRANT ALL PRIVILEGES ON `phpmyadmin`.* TO 'pma'@'localhost';
   FLUSH PRIVILEGES;
   ```

---

### Python Environment (Sensor Scripts)

```bash
# Create a virtual environment
py -3.14 -m venv venv

# Activate it
venv\Scripts\activate

# Install required libraries (example)
pip install pyserial numpy matplotlib pandas

# Export your dependencies
pip freeze > requirements.txt

# Restore dependencies on another machine
pip install -r requirements.txt
```

---

## Environment & Execution Policy Notes

If you encounter PowerShell execution errors when running Node-RED or activating the Python venv on Windows, use:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process
```

This applies only to the current terminal session and does not permanently change your system settings.

---
