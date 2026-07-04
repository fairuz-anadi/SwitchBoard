<div align="center">

# 🏢 SwitchBoard
**Lights, Fans, Discord: The Boss's Big Idea**

*A full-stack IoT office monitoring system built for Techathon Nationals (IUT Robotics Society).*

[![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com)
[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org)
[![Discord.js](https://img.shields.io/badge/Discord.js-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.js.org)
[![SQLite](https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white)](https://sqlite.org)

</div>

<hr />

## 📖 Overview

SwitchBoard is an end-to-end simulated IoT monitoring system designed to track electricity usage and device statuses across an office space. Born from "The Boss's Big Idea," this system ensures that lights and fans are never left running needlessly, saving power and reducing costs.

It provides a single, synchronized source of truth (Laravel API) consumed by two distinct interfaces:
1. **Live Web Dashboard:** A visual, real-time interface mapping out the office and displaying active power consumption.
2. **Discord Bot:** A conversational AI-powered assistant that lets employees and management query room statuses seamlessly from their workspace chat.

---

## ✨ Key Features

- 🔋 **Live Device Simulation:** A realistic hardware simulator that generates dynamic data for 15 devices (fans and lights) across 3 rooms.
- 📊 **Real-time Web Dashboard:** A responsive React-based UI that visualizes the office layout, updates device states instantly, and aggregates power consumption metrics.
- 🤖 **Conversational Discord Bot:** A smart bot powered by an LLM that parses live backend data into friendly, humanized responses (no robotic data dumps).
- 🔔 **Proactive Alerts:** An automated alert engine that monitors for anomalous situations (e.g., devices left on after office hours).
- 🔌 **Unified Shared Backend:** Both the dashboard and the Discord bot pull from the same API, ensuring perfectly synchronized data states.

---

## 🏗 System Architecture

The architecture enforces a strict decoupling between the data layer and presentation interfaces to ensure the Dashboard and the Bot always show the same reality.

```text
[ Hardware Simulator Layer ]
             │
             ▼
[     Laravel API Core     ] ── (SQLite Database)
             │
      ┌──────┴──────┐
      ▼             ▼
  [ Web UI ]   [ Discord Bot ]
```

*(Note: High-Level System Diagrams and Circuit Schematics are included in the repository deliverables).*

---

## ⚠️ Important Assumptions

> **Device Count Clarification:** 
> The problem statement contains a mathematical inconsistency regarding the total number of devices. The "Office Setup" section defines 3 rooms, each with 2 fans and 3 lights (3 × 5 = **15 devices**). However, the "Deliverables" section mentions "18 devices" in several places. 
> 
> **Resolution:** We have built the system and database for exactly **15 devices**, as this aligns perfectly with the provided top-down office layout diagram and the explicit per-room breakdown.

---

## 🚀 Getting Started

Follow these instructions to run the entire stack locally.

### Prerequisites
- **PHP 8.2+** & Composer
- **Node.js 18+** & npm
- A **Discord Bot Token** and **LLM API Key** (e.g. Groq/Gemini) for the bot.

### 1. Backend API (Laravel)

```bash
# Install dependencies
composer install

# Set up environment variables
cp .env.example .env
php artisan key:generate

# Run database migrations and seed the 15 devices
php artisan migrate:fresh --seed

# Start the Laravel development server (runs on port 8000)
php artisan serve
```

### 2. Live Device Simulator

In a new terminal window, start the simulator to generate dynamic, realistic power data (randomly flips states every 10 seconds and accumulates kWh usage):
```bash
php artisan devices:simulate
```

### 3. Web Dashboard (React + Vite)

In a new terminal window:
```bash
cd dashboard
npm install
npm run dev
```
*The dashboard will be available at `http://localhost:5174` (or `http://localhost:5173`).*

### 4. Discord Bot

In a new terminal window:
```bash
cd discord-bot
cp .env.example .env
```
Edit the `.env` file and fill in your `DISCORD_TOKEN` and LLM API Key.
```bash
npm install
npm start
```

---

## 💬 Discord Bot Commands

Once invited to your server, you can use the following commands as a quick-access remote control:

| Command | Action |
|---------|--------|
| `!status` | Get a conversational overview of all rooms and their active devices. |
| `!room <name>` | Check the exact state of a specific room (e.g., `!room work1`). |
| `!usage` | View real-time power draw and the estimated daily kWh usage. |

---

## 📡 API Endpoints

All endpoints are public (no authentication required for this demo).

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/health` | Health check to verify the API is running. |
| `GET` | `/api/v1/status` | Full status of all devices, grouped by room. |
| `GET` | `/api/v1/rooms/{id}` | Status of a single room and its devices. |
| `GET` | `/api/v1/usage` | Current total wattage + today's kWh estimate. |
| `GET` | `/api/v1/alerts` | All active (unresolved) anomaly alerts. |

---

## 🗄️ Database Schema

The SQLite database is structured around 4 core tables:

| Table | Description |
|-------|-------------|
| `rooms` | Core rooms in the office (`id`, `name`). |
| `devices` | 15 hardware devices (`id`, `room_id`, `name`, `type`, `status`, `power_watts`, `kwh_today`, `last_changed`). |
| `state_logs` | Audit trail of every device state change for history tracking. |
| `alerts` | System-generated anomaly alerts (`message`, `triggered_at`, `resolved`). |

---

<div align="center">
  <i>Built with ❤️ for Techathon Nationals.</i>
</div>
