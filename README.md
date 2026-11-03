<div align="center">

# EduConflux

### *Unify. Educate. Elevate.*

A next-generation, cloud-native educational management ecosystem that consolidates academic administration, virtual classrooms, real-time WebRTC video meetings, attendance tracking, and parent communication into a unified digital workspace.

[![Java 21](https://img.shields.io/badge/Java-21-orange.svg?style=flat-square&logo=openjdk)](https://openjdk.org/)
[![Spring Boot 3](https://img.shields.io/badge/Spring%20Boot-3.4+-brightgreen.svg?style=flat-square&logo=springboot)](https://spring.io/projects/spring-boot)
[![React 19](https://img.shields.io/badge/React-19-blue.svg?style=flat-square&logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)
[![Vite 8](https://img.shields.io/badge/Vite-8-purple.svg?style=flat-square&logo=vite)](https://vitejs.dev/)
[![Tailwind CSS v4](https://img.shields.io/badge/Tailwind%20CSS-v4-38B2AC.svg?style=flat-square&logo=tailwind-css)](https://tailwindcss.com/)
[![PostgreSQL Neon](https://img.shields.io/badge/PostgreSQL-Neon%20Serverless-4169E1.svg?style=flat-square&logo=postgresql)](https://neon.tech/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

[**Frontend Live App**](https://edu-conflux.vercel.app/) • [**Backend Repository**](https://github.com/EduConflux/educonflux-backend) • [**Frontend Repository**](https://github.com/EduConflux/educonflux-client)

</div>

---

## Table of Contents

- [Overview & Vision](#-overview--vision)
- [System Architecture](#-system-architecture)
- [Platform Capabilities & Modules](#-platform-capabilities--modules)
- [Role-Based Workflows](#-role-based-workflows)
- [Repository Ecosystem](#-repository-ecosystem)
- [Technology Stack](#-technology-stack)
- [Local Development Setup](#-local-development-setup)
  - [Backend Setup](#1-backend-setup-educonflux-backend)
  - [Frontend Setup](#2-frontend-setup-educonflux-client)
- [Production Cloud Deployment](#-production-cloud-deployment)
  - [Backend on Render](#backend-deployment-render-docker)
  - [Frontend on Vercel](#frontend-deployment-vercel)
- [Automated Testing & CI/CD](#-automated-testing--cicd)
- [License](#-license)

---

## Overview & Vision

Educational institutions routinely suffer from operational fragmentation — running disconnected software for student records, timetables, video conferences, assignments, daily attendance, and parent communication. 

**EduConflux** resolves this friction by providing a cohesive, multi-tenant digital backbone for modern institutions:

- **Unified Operations**: Administrators configure institutional structure, academic programs, semester calendars, and course allocations from one console.
- **Interactive Learning**: Instructors host interactive classrooms with announcements, rich discussions, resource distribution, and real-time WebRTC video lectures with screen sharing.
- **Transparent Progress**: Students monitor academic timetables, course attendance percentages, and assignment deliverables.
- **Parent Engagement**: Dedicated parent portal empowering guardians to monitor linked wards, review attendance patterns, and access daily schedules.

---

## System Architecture

```
                                  +---------------------------------------+
                                  |         CLIENTS & BROWSERS            |
                                  |    (Desktop, Tablet, Mobile Web)      |
                                  +---------------------------------------+
                                                      |
                                                      v
                                  +---------------------------------------+
                                  |     Vercel Edge CDN (Frontend)        |
                                  |       https://edu-conflux.vercel.app  |
                                  |    React 19 + Vite 8 + Tailwind v4    |
                                  +---------------------------------------+
                                          |                       |
                            HTTPS REST    |                       |  WSS STOMP
                            API Calls     |                       |  Signaling
                                          v                       v
                      +---------------------------------------------------------------+
                      |                    Render Web Service                         |
                      |              https://your-backend.onrender.com                |
                      |         Dockerized Eclipse Temurin JRE 21 Container           |
                      |---------------------------------------------------------------|
                      |       Spring Boot 3 REST APIs & Security Gateway (JWT)        |
                      |       WebSocket STOMP Broker (/topic, /queue, /app)           |
                      |       WebRTC Peer Signaling Relay (/ws)                       |
                      +---------------------------------------------------------------+
                                      |                               |
                                      v                               v
                     +----------------------------------+   +--------------------+
                     |    Neon PostgreSQL (Database)    |   |    Supabase S3     |
                     |   Serverless Relational Engine   |   |   Object Storage   |
                     +----------------------------------+   +--------------------+
                                                                      |
                                                                      v
                                                            +--------------------+
                                                            |  SMTP Mail Server  |
                                                            |  Gmail / SendGrid  |
                                                            +--------------------+
```

---

## Platform Capabilities & Modules

### 1. Identity & Access Management (RBAC)
- Multi-tenant data segregation by institution identifier.
- JWT-based authentication with expiration handling and automatic token renewal.
- Role-Based Access Control (`PLATFORM_ADMIN`, `INSTITUTION_ADMIN`, `TEACHER`, `STUDENT`, `PARENT`).
- Secure self-activation workflows with email invitation verification.

### 2. Academic Operations
- Multi-tier structure: Departments, Degree Programs, Academic Years, and Semesters.
- Course catalog management with credit weights and code identifiers.
- Class Section creation and student roster allocations.
- Course offerings and faculty-to-course scheduling assignments.

### 3. Real-Time WebRTC Classroom Video Meetings
- **One-Click Live Lectures**: Faculty can launch real-time video lectures directly inside any classroom.
- **WebRTC Mesh Architecture**: Peer-to-peer audio and video transmission with STOMP signaling broker.
- **In-Call Tools**: Screen sharing, microphone toggle, camera toggle, hand-raising indicator, and active speaker border highlights.
- **Classroom Chat & Roster**: Live in-call messaging drawer and participant list with Host controls (*End Meeting For All*).

### 4. Timetable & Scheduling
- Comprehensive weekly academic timetable generator.
- Collision avoidance for classrooms, faculty schedules, and student batches.
- Daily "Today's Schedule" quick glance for students and parents.

### 5. Attendance Management & Analytics
- Quick roster-based attendance register for faculty (`PRESENT`, `LATE`, `ABSENT`, `EXCUSED`).
- Automatic course-wise attendance percentage computations.
- Threshold alerts for low student attendance records.

### 6. Parent Portal & Ward Monitoring
- Multi-child switcher for parents with multiple enrolled siblings.
- Real-time access to child attendance summaries and percentage calculations.
- Live view of daily lecture timetables and assignment deadlines.

### 7. Cloud Storage & Notifications
- Supabase S3-compatible cloud bucket integration for course files, assignments, and submissions.
- Asynchronous JavaMail integration for transactional notifications and account invitations.

---

## Role-Based Workflows

| Role | Key Capabilities |
| :--- | :--- |
| **Institution Admin** | Configure departments, academic years, semesters, courses, manage faculty/student directories, link parents to wards, assign roles. |
| **Faculty (Teacher)** | Manage virtual classrooms, launch live video lectures, take student attendance, post announcements, create assignments, grade submissions. |
| **Student** | Access enrolled classrooms, join live video lectures, view timetables, submit coursework, participate in class chats, monitor personal attendance. |
| **Parent (Guardian)** | Dedicated guardian portal to monitor linked children, view attendance analytics, access weekly schedules, track academic deliverables. |

---

## Repository Ecosystem

| Repository | Tech Stack | Description | Deployment Target |
| :--- | :--- | :--- | :--- |
| [**educonflux-backend**](https://github.com/EduConflux/educonflux-backend) | Java 21, Spring Boot 3, Hibernate, PostgreSQL, Docker | Central REST API service, WebSocket STOMP broker, and WebRTC signaling engine | [Render](https://render.com/) (Docker Web Service) |
| [**educonflux-client**](https://github.com/EduConflux/educonflux-client) | React 19, TypeScript, Vite 8, Tailwind v4, TanStack Query | Modern single-page web frontend with responsive dashboard and video lecture room | [Vercel](https://vercel.com/) (Edge CDN SPA) |

---

## Technology Stack

### Backend Architecture
- **Language**: Java 21 (LTS)
- **Framework**: Spring Boot 3.4+, Spring Data JPA, Spring Security, Spring WebSocket
- **Build Tool**: Apache Maven 3.9+
- **Database**: PostgreSQL (Neon Serverless)
- **Object Storage**: Supabase Storage (AWS S3 SDK)
- **Realtime Protocols**: WebSocket + STOMP + WebRTC Peer-to-Peer
- **Containerization**: Docker (Eclipse Temurin JRE 21 Alpine)

### Frontend Architecture
- **Framework**: React 19 + TypeScript
- **Bundler & Tooling**: Vite 8
- **Styling**: Tailwind CSS v4, Lucide Icons, GSAP
- **State & Server Cache**: TanStack React Query v5
- **Realtime Client**: `@stomp/stompjs`, `sockjs-client`
- **Testing**: Vitest, React Testing Library, jsdom

---

## Local Development Setup

### 1. Backend Setup (`educonflux-backend`)

#### Prerequisites
- JDK 21+ installed and configured (`java -version`)
- Maven 3.9+ (`mvn -version`)
- PostgreSQL database instance (local or Neon)

#### Steps
```bash
# Clone the repository
git clone https://github.com/EduConflux/educonflux-backend.git
cd educonflux-backend

# Optional: Place local credentials in src/main/resources/application-local.properties
# Or export system environment variables:
export DATABASE_URL="jdbc:postgresql://localhost:5432/educonflux"
export DATABASE_USERNAME="postgres"
export DATABASE_PASSWORD="your_password"
export JWT_SECRET="your_secure_random_base64_secret_key"

# Run tests
mvn test

# Start the Spring Boot server
mvn spring-boot:run
```
The backend server runs on `http://localhost:8080`.

---

### 2. Frontend Setup (`educonflux-client`)

#### Prerequisites
- Node.js 20.x or later (`node -v`)
- npm 10.x or later (`npm -v`)

#### Steps
```bash
# Clone the repository
git clone https://github.com/EduConflux/educonflux-client.git
cd educonflux-client

# Install dependencies
npm install

# Run unit tests
npm test

# Launch the development server
npm run dev
```
The frontend client runs on `http://localhost:5173` with proxy forwarding to the local backend.

---

## Production Cloud Deployment

### Backend Deployment (Render Docker)
1. Push `educonflux-backend` to GitHub.
2. Log in to [Render](https://dashboard.render.com/) and create a **Web Service** pointing to `educonflux-backend`.
3. Set **Runtime** to `Docker`.
4. Leave **Root Directory** blank and ensure **Dockerfile Path** is set to `./Dockerfile`.
5. Set **Health Check Path** to `/api/health`.
6. Configure the following **Environment Variables**:
   - `DATABASE_URL`: Your production Neon PostgreSQL connection URL.
   - `DATABASE_USERNAME` & `DATABASE_PASSWORD`: Database credentials.
   - `JWT_SECRET`: Production JWT secret.
   - `FRONTEND_URL`: `https://edu-conflux.vercel.app`
   - `MAIL_USERNAME` & `MAIL_PASSWORD`: SMTP credentials.
   - `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_S3_ENDPOINT`: Storage keys.
7. Deploy!

*(Tip: To prevent Render free tier sleep, a GitHub Actions cron job is provided in `.github/workflows/keep-alive.yml` or run `node scripts/keep-alive.js`)*.

---

### Frontend Deployment (Vercel)
1. Push `educonflux-client` to GitHub.
2. Log in to [Vercel](https://vercel.com/) and click **Add New > Project**.
3. Import `educonflux-client`.
4. In **Environment Variables**, add:
   - `VITE_API_BASE_URL`: `https://your-backend.onrender.com/api`
   - `VITE_WS_BASE_URL`: `https://your-backend.onrender.com/ws` (optional; auto-derived)
5. Deploy! Vercel handles SPA client-side routing via [`vercel.json`](file:///d:/EduConflux/educonflux-client/vercel.json).

---

## 🧪 Automated Testing & CI/CD

Both repositories are equipped with isolated automated testing suites and dedicated GitHub Actions CI pipelines:

### Backend Testing Suite (24 Tests)
- Unit tests with **JUnit 5** and **Mockito** covering `AuthService`, `AttendanceService`, `ParentService`, and `ClassroomMeetingService`.
- Context integration tests isolated using an in-memory **H2 database** in PostgreSQL compatibility mode.
- Verified on every pull request and push to `main` via [`.github/workflows/ci.yml`](file:///d:/EduConflux/educonflux-backend/.github/workflows/ci.yml).

### Frontend Testing Suite (19 Tests)
- Modern unit tests with **Vitest** and **React Testing Library** covering `LoginForm`, `RoleSelector`, `MeetingToolbar`, `VideoTile`, and `parentApi`.
- Strict TypeScript type-checking (`tsc -b`) and production bundle validation.
- Verified on every pull request and push to `main` via [`.github/workflows/ci.yml`](file:///d:/EduConflux/educonflux-client/.github/workflows/ci.yml).

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2026 EduConflux

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

---

<div align="center">
  <b>EduConflux</b> — Unifying Education Through Technology.
</div>

