# DSC623 - Reliable Rentals - Database Design Project

The project moves a real-world case study through the full database design lifecycle: conceptual model - logical model - physical implementation.

## Authors
- Fabrisio Ponte Vela
- Salome Collante Ramirez
- Martineulr Marasigan

## Case Study
Reliable Rentals operates multiple outlets that rent vehicles to clients. Each outlet maintains a stock of vehicles and a team of staff. Clients sign hire agreements to rent vehicles for varying periods. The database tracks outlets, vehicles, clients, staff, and hire agreements with full referential integrity.

## Repository Structure

```
DSC623-Project_DB-Reliable_Rentals/
├── README.md
├── Part 1 - Conceptual/
│   └── Project_-_Conceptual_Design.pdf
├── Part 2 - Logical/
│   └── Project_-_Logical_Design.pdf
└── Part 3 - Implementation/
    ├── Reliable_Rentals.py        # Embedded SQL implementation
    ├── Reliable_Rentals.db        # Generated SQLite database
    └── Project_-_Implementation.pdf
```

## Project Phases

### Part 1 — Conceptual Design
ER diagram of the case study with entities, attributes, relationships, multiplicities, and assumptions.

### Part 2 — Logical Design
- Relational schema derived from the conceptual ER model
- Normalization validated to **3NF**
- Logical model validated against 5 user transactions
- Integrity constraints (primary keys, foreign keys, alternate keys, domain constraints, general constraints)
- Logical-level ER diagram with foreign keys as explicit attributes

### Part 3 — Physical Implementation
SQLite database created using **embedded SQL** in Python via the `sqlite3` module. Includes:
- Schema with all enforceable integrity constraints from Part 2
- 5+ tuples per relation as sample data
- The 5 user transaction queries from Part 2.C

## Running the Code

**Requirements:** Python 3.8+ and pandas (`pip install pandas`)

```
cd "Part 3 - Implementation"
python Reliable_Rentals.py
```

This creates `Reliable_Rentals.db`, populates it with sample data, and executes the 5 user transactions, displaying the results in the terminal.

The generated `.db` file can also be opened in DBeaver (or any SQLite client) to inspect the tables visually.

## Schema Summary

| Relation | Primary Key | Foreign Keys |
|---|---|---|
| Outlet | outletNo | — |
| Staff | staffNo | outletNo → Outlet |
| Vehicle | registrationNo | outletNo → Outlet |
| Client | clientNo | — |
| HireAgreement | hireNo | clientNo → Client, registrationNo → Vehicle |

Alternate keys: `Outlet.phone_number`, `Outlet.fax_number`, `Client.driving_license_number`.
