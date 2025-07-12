# Railway Reservation System

A multi-threaded Java client-server application for booking train tickets, backed by a PostgreSQL database.

## Features
- Add new trains and coaches (admin)
- Book tickets for multiple passengers (client)
- Multi-threaded server and client for concurrent requests
- PostgreSQL backend with stored procedures for ticketing logic

## Project Structure
- `ServiceModule.java`: Main server, handles client requests and interacts with the database
- `client.java`: Simulates multiple users booking tickets concurrently
- `admin.java`: Adds new trains to the system from a file
- `db.pgsql`: SQL script to set up the database schema and stored procedures
- `admin.txt`: Example input for adding trains
- `pool-*-input.txt`: Example input files for booking requests (used by client)
- `postgresql-42.5.0.jar`: PostgreSQL JDBC driver (required for Java DB connectivity)

## Prerequisites
- Java JDK 8 or higher
- PostgreSQL (tested with v12+)
- JDBC driver (`postgresql-42.5.0.jar` included)

## Database Setup
1. Start PostgreSQL and create a user (default in code: `postgres`/`shyam2672002`).
2. Run the SQL script to set up the schema and procedures:
   ```sh
   psql -U postgres -f db.pgsql
   ```
   This creates the `train_reservation_system` database and all required tables and procedures.

## Compiling the Code
Navigate to the `railway-reservation-system-main` directory and compile all Java files:
```sh
javac -cp .;postgresql-42.5.0.jar ServiceModule.java client.java admin.java
```
(Use `:` instead of `;` as the separator on Linux/Mac.)

## Running the System
### 1. Start the Server
```sh
java -cp .;postgresql-42.5.0.jar ServiceModule
```

### 2. Add Trains (Admin)
Edit `admin.txt` to specify trains to add (see format below), then run:
```sh
java -cp .;postgresql-42.5.0.jar admin
```

#### Example `admin.txt` format:
```
<train_id> <date:YYYY-MM-DD> <no_of_ac_coaches> <no_of_sl_coaches>
6909 2023-01-11 0 5
4523 2023-05-10 0 5
#
```

### 3. Book Tickets (Client)
Edit or create input files like `pool-1-thread-1_input.txt`:
```
<no_of_passengers> <name1>, <name2>, ... <train_id> <date:YYYY-MM-DD> <coach_type>
8 HYHH, JVTK, MCRI, MTIP, UMPY, MBOO, RCWL, RRIU 3472 2023-07-13 SL
#
```
Run the client:
```sh
java -cp .;postgresql-42.5.0.jar client
```

## Schema
| Tables     | Attributes                                                   |
| ---------- | ------------------------------------------------------------ |
| train      | train_ID, doj                                                |
| AC_COACH   | berth_no,berth_type                                          |
| SL_COACH   | berth_no,berth_type                                          |
| train_pool | train_ID, doj, coach_type,coah_no,berth_no,berth_type,empty  |
| passenger  | name,pnr                                                     |
| TICKET     | passenger_name,train_id,doj,pnr,coach_no,berth_no,berth_type |

## Stored Procedures
- `ADDTRAIN`: Adds new train in the database
- `BOOKTICKET`: Books ticket(s) for passengers

## Notes
- Update database credentials in the Java files if needed.
- Input/output file paths in `client.java` may need to be adjusted for your environment.
- The server listens on port 7005 by default.
- For multi-user simulation, multiple input/output files are used.

---
Feel free to improve or extend this system for your needs!
