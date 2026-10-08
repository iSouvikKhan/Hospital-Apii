# Hospital-API

A REST API built with Node.js, Express and MongoDB for the doctors of a hospital allocated by the government for testing, quarantine and well-being of COVID-19 patients. Doctors register and log in, register patients, and create test reports for them.

## Features

There are 2 types of users: *Doctors* and *Patients*.

- Doctors can register and log in. Login returns a JWT token.
- Each time a patient visits, the doctor follows 2 steps:
    - Register the patient in the app using a phone number. If the patient already exists, the API returns the existing patient info.
    - After the checkup, create a report.
- A patient report has the following fields:
    - Created by doctor
    - Status: one of *[0: Negative, 1: Travelled-Quarantine, 2: Symptoms-Quarantine, 3: Positive-Admit]*
    - Date (creation timestamp)
- List all reports of a patient, oldest first.
- List all reports across all patients filtered by status.

## Tech Stack

- Node.js, Express
- MongoDB with Mongoose
- Passport with `passport-jwt` and `jsonwebtoken` for authentication
- `body-parser`, `morgan`, `config`
- Mocha, Chai and `chai-http` for tests

## Prerequisites

- Node.js and npm
- A MongoDB database (MongoDB Atlas or a local instance)

## How to Install and Run

1. Clone the project and move into the folder:

   ```bash
   git clone https://github.com/iSouvikKhan/Hospital-Apii.git
   cd Hospital-Apii
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Configure the database. The MongoDB connection string is set directly in `config/mongoose.js` (there is no `.env` support). Replace it with your own connection string, for example a local database such as `mongodb://127.0.0.1/HospitalAPI` (a commented-out line for this already exists in the file).

4. Start the server:

   ```bash
   node index.js
   ```

   For auto-reload during development, `npm start` runs `nodemon index.js`. Note that this script uses the Windows `SET NODE_ENV=dev` syntax, so it works in Windows Command Prompt. On Linux/macOS run nodemon directly instead:

   ```bash
   NODE_ENV=dev npx nodemon index.js
   ```

The server listens on the port given by the `PORT` environment variable, or `9000` by default.

## How to Use

Base URL: `http://localhost:9000/api/v1`

Routes marked "requires JWT" need the header `Authorization: Bearer <token>`, where the token comes from `/doctors/login`.

#### Endpoints

1. `/doctors/register` (POST): Register a new doctor using `name`, `email` and `password` (all required).

- INPUT:

![](/Images/1.JPG)

- OUTPUT

![](/Images/2.JPG)

2. `/doctors/login` (POST): A doctor logs in using `email` and `password`.

- INPUT:

![](/Images/3.JPG)

- OUTPUT (generated JWT token)

![](/Images/4.JPG)

3. `/patients/register` (POST, requires JWT): A doctor registers a patient using `name` and `phone`.

#### Token needed for this request:

![](/Images/5.JPG)

- INPUT:

![](/Images/6.JPG)

- OUTPUT

![](/Images/7.JPG)

4. `/patients/:id/create_report` (POST, requires JWT): A doctor creates a report for the patient.

#### Token needed for this request:

![](/Images/8.JPG)

- INPUT: send `status` (0: Negative, 1: Travelled-Quarantine, 2: Symptoms-Quarantine, 3: Positive-Admit) and `doctor` (the doctor's id).

![](/Images/9.JPG)

- OUTPUT

![](/Images/10.JPG)

5. `/patients/:id/all_reports` (GET): Retrieve all reports of a patient by patient id.

- OUTPUT

![](/Images/11.JPG)

6. `/reports/:status` (GET): Retrieve all reports from the database filtered by the status code (0-3) sent in the URL.

- OUTPUT

![](/Images/12.JPG)

## Unit Testing

Run the tests with:

```bash
npm test
```

- Uses `mocha` as the test runner and `chai` / `chai-http` for assertions and HTTP requests.

Tests cover:

1. `/patients/register`
2. `/patients/:id/create_report`
3. `/patients/:id/all_reports`

The tests use a hard-coded JWT token and patient/doctor ids that must exist in the connected database, so they will only pass against matching data.

- Output (on the console):

![](/Images/13.JPG)

## Folder Structure

- **index.js**: Entry point. Sets up Express, body parsing, logging, Passport and routes.
- **config**: Mongoose connection, Passport JWT strategy and the report status mapping.
- **controllers/api/v1**: Controllers for the doctor, patient and report APIs.
- **models**: Mongoose schemas for doctors, patients and reports.
- **routes**: Routers for `/api/v1/doctors`, `/api/v1/patients` and `/api/v1/reports`.
- **test**: Mocha test files for the patient routes.
- **Images**: Screenshots used in this README.
