#====== Project Part 3 - Reliable Rentals ======
#====== Fabrisio Ponte Vela - Salome Collante-Ramirez - Martineulr Marasigan ======

import sqlite3
import pandas as pd


# Db File: Reliable_Rentals.db
db_connect = sqlite3.connect('Reliable_Rentals.db')

# Instantiate cursor object for executing queries
cursor = db_connect.cursor()

# Foreign key constraints 
cursor.execute("PRAGMA foreign_keys = ON;")


#---- 1. SCHEMA ----
#-------A. Outlet----------
query = """
    CREATE TABLE Outlet(
    outletNo VARCHAR(100) NOT NULL,
    address VARCHAR(200) NOT NULL,
    phone_number VARCHAR(20) NOT NULL UNIQUE,
    fax_number VARCHAR(20) NOT NULL UNIQUE,

    PRIMARY KEY(outletNo)
    );
    """
cursor.execute(query)

#-------B. Staff----------
query = """
    CREATE TABLE Staff(
    staffNo VARCHAR(100) NOT NULL,
    outletNo VARCHAR(100) NOT NULL,
    fName VARCHAR(100) NOT NULL,
    lName VARCHAR(100) NOT NULL,
    home_address VARCHAR(200) NOT NULL,
    phone_number VARCHAR(20) NOT NULL,
    DOB DATE NOT NULL,
    sex VARCHAR(1) NOT NULL CHECK(sex IN ('M', 'F')),
    company_start_date DATE NOT NULL,
    job_title VARCHAR(100) NOT NULL,
    salary REAL NOT NULL CHECK(salary > 0),

    PRIMARY KEY(staffNo),
    FOREIGN KEY(outletNo) REFERENCES Outlet(outletNo),
    CHECK(DATE(company_start_date) >= DATE(DOB, '+16 years'))
    );
    """
cursor.execute(query)

#-------C. Vehicle----------
query = """
    CREATE TABLE Vehicle(
    registrationNo VARCHAR(100) NOT NULL,
    outletNo VARCHAR(100) NOT NULL,
    model VARCHAR(100) NOT NULL,
    make VARCHAR(100) NOT NULL,
    engine_size REAL NOT NULL CHECK(engine_size > 0),
    capacity INT NOT NULL CHECK(capacity >= 1),
    current_mileage INT NOT NULL CHECK(current_mileage >= 0),
    daily_hire_rate REAL NOT NULL CHECK(daily_hire_rate > 0),

    PRIMARY KEY(registrationNo),
    FOREIGN KEY(outletNo) REFERENCES Outlet(outletNo)
    );
    """
cursor.execute(query)

#-------D. Client----------
query = """
    CREATE TABLE Client(
    clientNo VARCHAR(100) NOT NULL,
    fName VARCHAR(100) NOT NULL,
    lName VARCHAR(100) NOT NULL,
    home_address VARCHAR(200) NOT NULL,
    phone_number VARCHAR(20) NOT NULL,
    DOB DATE NOT NULL,
    driving_license_number VARCHAR(50) NOT NULL UNIQUE,

    PRIMARY KEY(clientNo)
    );
    """
cursor.execute(query)

#-------E. HireAgreement----------
# mileage_after is the only nullable field — unknown until vehicle is returned
query = """
    CREATE TABLE HireAgreement(
    hireNo VARCHAR(100) NOT NULL,
    clientNo VARCHAR(100) NOT NULL,
    registrationNo VARCHAR(100) NOT NULL,
    date_of_hire DATE NOT NULL,
    hire_termination_date DATE NOT NULL,
    mileage_before INT NOT NULL CHECK(mileage_before >= 0),
    mileage_after INT,

    PRIMARY KEY(hireNo),
    FOREIGN KEY(clientNo) REFERENCES Client(clientNo),
    FOREIGN KEY(registrationNo) REFERENCES Vehicle(registrationNo),
    CHECK(date_of_hire <= hire_termination_date),
    CHECK(mileage_after IS NULL OR mileage_after >= mileage_before)
    );
    """
cursor.execute(query)


#---- 2. SAMPLE DATA (5 tuples per relation) ----
#-------A. Outlet----------
query = """
    INSERT INTO Outlet VALUES
    ('O001', '120 Brickell Ave, Miami, FL',         '305-555-0101', '305-555-0201'),
    ('O002', '450 Collins St, Miami Beach, FL',     '305-555-0102', '305-555-0202'),
    ('O003', '88 SW 8th St, Coral Gables, FL',      '305-555-0103', '305-555-0203'),
    ('O004', '301 Las Olas Blvd, Fort Lauderdale',  '954-555-0104', '954-555-0204'),
    ('O005', '12 Ocean Dr, Cutler Bay, FL',         '305-555-0105', '305-555-0205');
    """
cursor.execute(query)

#-------B. Staff ----------
query = """
    INSERT INTO Staff VALUES
    ('ST001', 'O001', 'Maria',  'Gonzalez', '500 NE 1st Ave, Miami',          '305-555-1001', '1985-03-12', 'F', '2015-06-01', 'Outlet Manager',   62000),
    ('ST002', 'O002', 'James',  'Carter',   '720 Lincoln Rd, Miami Beach',    '305-555-1002', '1990-07-22', 'M', '2018-09-15', 'Rental Agent',     42000),
    ('ST003', 'O003', 'Aisha',  'Nguyen',   '15 Granada Blvd, Coral Gables',  '305-555-1003', '1992-11-04', 'F', '2019-01-10', 'Outlet Manager',   58000),
    ('ST004', 'O004', 'Daniel', 'Rivera',   '88 SE 17th St, Fort Lauderdale', '954-555-1004', '1988-05-18', 'M', '2016-04-20', 'Senior Agent',     51000),
    ('ST005', 'O005', 'Sophia', 'Kim',      '210 Marina Dr, Cutler Bay',      '305-555-1005', '1995-09-30', 'F', '2021-02-08', 'Rental Agent',     40000),
    ('ST006', 'O001', 'Liam',   'Patel',    '350 NW 7th St, Miami',           '305-555-1006', '1993-12-14', 'M', '2020-07-01', 'Maintenance Tech', 38000);
    """
cursor.execute(query)

#-------C. Vehicle----------
query = """
    INSERT INTO Vehicle VALUES
    ('FL-A1234', 'O001', 'Camry',   'Toyota',        2.5, 5, 18500,  55),
    ('FL-B5678', 'O001', 'Corolla', 'Toyota',        1.8, 5, 22000,  45),
    ('FL-C9012', 'O002', 'Civic',   'Honda',         2.0, 5, 31000,  50),
    ('FL-D3456', 'O002', 'C-Class', 'Mercedes Benz', 2.0, 5, 12500, 110),
    ('FL-E7890', 'O003', 'E-Class', 'Mercedes Benz', 3.0, 5,  8200, 145),
    ('FL-F2345', 'O003', 'RAV4',    'Toyota',        2.5, 5, 27500,  65),
    ('FL-G6789', 'O004', 'Mustang', 'Ford',          5.0, 4, 15000, 120),
    ('FL-H0123', 'O005', 'Model 3', 'Tesla',         0.1, 5,  9800, 130);
    """
cursor.execute(query)

#-------D. Client----------
query = """
    INSERT INTO Client VALUES
    ('C001', 'Olivia',   'Martinez', '101 Sunset Dr, Miami',          '305-555-2001', '1992-04-15', 'D-MART-9201'),
    ('C002', 'Ethan',    'Brown',    '55 Coral Way, Coral Gables',    '305-555-2002', '1985-08-22', 'D-BROW-8501'),
    ('C003', 'Isabella', 'Davis',    '780 Biscayne Blvd, Miami',      '305-555-2003', '1998-12-03', 'D-DAVI-9801'),
    ('C004', 'Noah',     'Wilson',   '22 Ocean Ave, Miami Beach',     '305-555-2004', '1979-06-10', 'D-WILS-7901'),
    ('C005', 'Ava',      'Thompson', '300 Las Olas, Fort Lauderdale', '954-555-2005', '2000-01-28', 'D-THOM-0001'),
    ('C006', 'Lucas',    'Garcia',   '14 Marina Rd, Cutler Bay',      '305-555-2006', '1990-10-19', 'D-GARC-9001');
    """
cursor.execute(query)

#-------E. HireAgreement----------
query = """
    INSERT INTO HireAgreement VALUES
    ('H10001', 'C002', 'FL-C9012', '2026-01-10', '2026-01-17', 30200, 30850),
    ('H10002', 'C004', 'FL-G6789', '2026-02-05', '2026-02-12', 14500, 14950),
    ('H10003', 'C001', 'FL-A1234', '2026-04-20', '2026-05-05', 18500, NULL),
    ('H10004', 'C003', 'FL-B5678', '2026-04-15', '2026-05-10', 22000, NULL),
    ('H10005', 'C006', 'FL-F2345', '2026-04-10', '2026-05-01', 27500, NULL),
    ('AXNZN',  'C005', 'FL-E7890', '2026-04-25', '2026-05-15',  8200, NULL),
    ('H10006', 'C002', 'FL-H0123', '2026-04-27', '2026-05-04',  9800, NULL);
    """
cursor.execute(query)

# Commit any changes to the database
db_connect.commit()

# Display all tables
for table in ['Outlet', 'Staff', 'Vehicle', 'Client', 'HireAgreement']:
    query = f"SELECT * FROM {table}"
    cursor.execute(query)
    column_names = [row[0] for row in cursor.description]
    table_data = cursor.fetchall()
    df = pd.DataFrame(table_data, columns=column_names)
    print(f"Data from {table} table:")
    print(df)
    print()


#---- 3. USER TRANSACTION QUERIES (from Part 2.C) ----

#------- Q1: Most recently signed hire agreement----------
# Tables: Client, HireAgreement (joined via clientNo)
query = """
    SELECT ha.registrationNo, ha.mileage_before,
           c.fName || ' ' || c.lName AS client_full_name,
           ha.date_of_hire
    FROM HireAgreement ha, Client c
    WHERE ha.clientNo = c.clientNo
    ORDER BY ha.date_of_hire DESC
    LIMIT 1;
    """
cursor.execute(query)
column_names = [row[0] for row in cursor.description]
table_data = cursor.fetchall()
df = pd.DataFrame(table_data, columns=column_names)
print("Q1: Registration number, mileage before, and client full name for the most recently signed hire agreement")
print(df)
print()

#-------Q2: Highest daily hire rate across all hire agreements----------
# Tables: Vehicle, HireAgreement (joined via registrationNo to restrict to vehicles that have been hired)
query = """
    SELECT MAX(v.daily_hire_rate) AS highest_daily_hire_rate
    FROM Vehicle v, HireAgreement ha
    WHERE v.registrationNo = ha.registrationNo;
    """
cursor.execute(query)
column_names = [row[0] for row in cursor.description]
table_data = cursor.fetchall()
df = pd.DataFrame(table_data, columns=column_names)
print("Q2: Highest daily hire rate charged across all hire agreements")
print(df)
print()

#------- Q3: Outlets above 2-month avg mileage with 5% rate increase----------
# Tables: Outlet, Vehicle, HireAgreement
query = """
    SELECT DISTINCT o.outletNo, o.address, v.registrationNo,
           v.current_mileage,
           v.daily_hire_rate AS current_rate,
           ROUND(v.daily_hire_rate * 1.05, 2) AS projected_rate
    FROM Outlet o, Vehicle v, HireAgreement ha
    WHERE v.outletNo = o.outletNo
      AND ha.registrationNo = v.registrationNo
      AND v.current_mileage > (
          SELECT AVG(v2.current_mileage)
          FROM Vehicle v2, HireAgreement ha2
          WHERE ha2.registrationNo = v2.registrationNo
            AND ha2.date_of_hire >= DATE('now', '-2 months')
      )
    ORDER BY o.outletNo, v.registrationNo;
    """
cursor.execute(query)
column_names = [row[0] for row in cursor.description]
table_data = cursor.fetchall()
df = pd.DataFrame(table_data, columns=column_names)
print("Q3: Outlets with vehicles above the 2-month average mileage, with 5% rate increase")
print(df)
print()

#------- Q4: Active Toyota clients and models----------
# Active = today between date_of_hire and hire_termination_date
# Q4a: count of distinct active Toyota clients
query = """
    SELECT COUNT(DISTINCT ha.clientNo) AS active_toyota_clients
    FROM HireAgreement ha, Vehicle v
    WHERE ha.registrationNo = v.registrationNo
      AND v.make = 'Toyota'
      AND DATE('now') BETWEEN ha.date_of_hire AND ha.hire_termination_date;
    """
cursor.execute(query)
column_names = [row[0] for row in cursor.description]
table_data = cursor.fetchall()
df = pd.DataFrame(table_data, columns=column_names)
print("Q4a: Number of distinct clients with an active Toyota hire agreement")
print(df)
print()

# Q4b: distinct Toyota models currently being hired
query = """
    SELECT DISTINCT v.model
    FROM HireAgreement ha, Vehicle v
    WHERE ha.registrationNo = v.registrationNo
      AND v.make = 'Toyota'
      AND DATE('now') BETWEEN ha.date_of_hire AND ha.hire_termination_date
    ORDER BY v.model;
    """
cursor.execute(query)
column_names = [row[0] for row in cursor.description]
table_data = cursor.fetchall()
df = pd.DataFrame(table_data, columns=column_names)
print("Q4b: Distinct Toyota models currently being hired")
print(df)
print()

#------- Q5: Client contact details from hire AXNZN----------
# Tables: Client, HireAgreement, Vehicle
query = """
    SELECT c.clientNo,
           c.fName || ' ' || c.lName AS client_full_name,
           c.home_address, c.phone_number,
           v.make, v.model, v.engine_size
    FROM HireAgreement ha, Client c, Vehicle v
    WHERE ha.clientNo = c.clientNo
      AND ha.registrationNo = v.registrationNo
      AND ha.hireNo = 'AXNZN'
      AND v.make = 'Mercedes Benz'
      AND v.engine_size = (SELECT MAX(engine_size) FROM Vehicle WHERE make = 'Mercedes Benz');
    """
cursor.execute(query)
column_names = [row[0] for row in cursor.description]
table_data = cursor.fetchall()
df = pd.DataFrame(table_data, columns=column_names)
print("Q5: Contact details for the client on hire AXNZN (Mercedes Benz with highest engine size)")
print(df)
print()


db_connect.close()
