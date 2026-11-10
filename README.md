# Hospital Appointment API

A complete Node.js + TypeScript + Prisma CRUD module for the hospital appointment domain.

## Project Overview

This project implements CRUD operations for a hospital appointment system using Prisma ORM with PostgreSQL. The schema was introspected from an existing PostgreSQL database using `prisma db pull` and cleaned to follow standard Prisma and TypeScript conventions.

### Database Models

1. **Patient (`patients`)**
   - Fields: `id`, `name`, `email`, `phone`, `dateOfBirth`, `createdAt`
   - Relations: One-to-many with `Appointment` (cascades on delete)

2. **Doctor (`doctors`)**
   - Fields: `id`, `name`, `specialty`, `email`, `createdAt`
   - Relations: One-to-many with `Appointment` (restrict on delete)

3. **Appointment (`appointments`)**
   - Fields: `id`, `appointmentDate`, `status`, `notes`, `createdAt`, `patientId`, `doctorId`
   - Relations: Belongs to `Patient` (`onDelete: Cascade`) and `Doctor` (`onDelete: Restrict`)

## Implemented Modules

- **`src/lib/prisma.ts`**: PrismaClient singleton using `@prisma/adapter-pg` driver adapter.
- **`src/patients.ts`**:
  - `createPatient(data)`
  - `getPatient(id)`
  - `searchPatients(nameFragment)`
  - `updatePatientPhone(id, phone)`
  - `deletePatient(id)`
- **`src/doctors.ts`**:
  - `createDoctor(data)`
  - `getDoctor(id)`
  - `listDoctorsBySpecialty(specialty)`
  - `deleteDoctor(id)`
- **`src/appointments.ts`**:
  - `bookAppointment(patientId, doctorId, date, notes)`
  - `getAppointmentFull(id)`
  - `getDoctorUpcomingAppointments(doctorId)`
  - `setAppointmentStatus(id, status)`
  - `cancelAllPatientAppointments(patientId)`
  - `deleteAppointment(id)`
- **`src/seed.ts`**: Database seed script populating sample doctors, patients, and scheduled appointments.
- **`src/test.ts`**: End-to-end test script exercising all CRUD operations across all models.

## Setup & Running

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Configure environment**:
   Create a `.env` file with your PostgreSQL connection URL:
   ```env
   DATABASE_URL="postgresql://<user>:<password>@localhost:5432/hospital_api?schema=public"
   ```

3. **Generate Prisma Client**:
   ```bash
   npx prisma generate
   ```

4. **Seed Database**:
   ```bash
   npx prisma db seed
   ```

5. **Run Test Script**:
   ```bash
   npx tsx src/test.ts
   ```
