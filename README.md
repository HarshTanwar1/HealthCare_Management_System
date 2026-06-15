<div align="center">

# 🏥 E-Health Care Management System

A desktop application to digitize hospital operations — patient admissions, discharges, billing, and staff management — for **Apex Hospital**

<br/>

![Java](https://img.shields.io/badge/Java-13-orange?style=for-the-badge&logo=openjdk&logoColor=white)
![Swing](https://img.shields.io/badge/GUI-Java%20Swing-blue?style=for-the-badge&logo=java&logoColor=white)
![Apache Derby](https://img.shields.io/badge/Database-Apache%20Derby-red?style=for-the-badge&logo=apache&logoColor=white)
![JDBC](https://img.shields.io/badge/Connectivity-JDBC-green?style=for-the-badge)
![Ant](https://img.shields.io/badge/Build-Apache%20Ant-yellow?style=for-the-badge&logo=apache&logoColor=white)

</div>

<br>

## 📖 Overview

The **E-Health Care Management System** is a Java Swing desktop application that replaces paper-based hospital record-keeping with a simple, database-backed workflow. Staff can securely log in, admit and discharge patients, search records, and generate a discharge bill automatically — all from an intuitive multi-window interface backed by a relational database over JDBC.

> Built end-to-end: from GUI design and event handling, to JDBC data access, to packaging a distributable runnable JAR

<br>

## ✨ Features

| Module                          | What it does                                                                                                  |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| 🔐 **Staff Login**              | Authenticates users against the `HOSPITAL_STAFF` table.                                                       |
| 🛡️ **Admin-gated Registration** | New accounts require an admin key before a staff member can be registered.                                    |
| 🧭 **Task Dashboard**           | A central menu that routes to every operation in the system.                                                  |
| ➕ **Admit Patient**            | Records name, ailment, and admit date (via a calendar picker) and auto-suggests the next free patient number. |
| 🚪 **Discharge Patient**        | Looks up a patient, updates their status, and opens the billing window.                                       |
| 🧾 **Automatic Billing**        | Computes the bill from the number of days admitted (₹500/day).                                                |
| 🔎 **View & Search Records**    | Displays all patients in a live table and supports search by name.                                            |

<br>

## 🛠️ Tech Stack

<div align="center">

| Category          | Technology                                                                |
| ----------------- | ------------------------------------------------------------------------- |
| **Language**      | Java (SE 13)                                                              |
| **GUI**           | Java Swing (NetBeans GUI Builder / Matisse)                               |
| **Database**      | Apache Derby (JavaDB), network mode on `localhost:1527`                   |
| **Data Access**   | JDBC (`java.sql`)                                                         |
| **Build Tool**    | Apache Ant                                                                |
| **Key Libraries** | JCalendar (date picker) · AbsoluteLayout · Derby client · MySQL connector |

</div>

<br>

## 🏗️ Architecture & Flow

```
                   ┌─────────────────┐
                   │  Login (Staff)  │
                   └────────┬────────┘
            register │                │ authenticate
        ┌────────────▼───┐    ┌───────▼─────────────┐
        │  Admin Key →   │    │   Task Dashboard    │
        │  Sign Up Staff │    └──┬─────────┬────────┘
        └────────────────┘       │         │        │
                          ┌──────▼──┐ ┌────▼────┐ ┌─▼──────────┐
                          │  Admit  │ │Discharge│ │View / Search│
                          │ Patient │ │ Patient │ │  Records    │
                          └─────────┘ └────┬────┘ └─────────────┘
                                           │
                                    ┌──────▼──────┐
                                    │  Auto Bill  │
                                    └─────────────┘
```

### Data Model

- **`HOSPITAL_STAFF`** — `USERNAME`, `PASSWORD`
- **`PATIENTS`** — `PATIENT_NUMBER`, `PATIENT_NAME`, `PATIENT_AILMENT`, `STATUS`, `ADMITDATE`, `DISCHARGEDATE`

<br>

## 🚀 Getting Started

### Prerequisites

- **JDK 13+**
- **Apache Derby (JavaDB)** with the network server running on `localhost:1527`
- **Apache Ant** _(optional — only to build from source; NetBeans can build/run too)_

### 1️⃣ Set up the database

Start the Derby network server, create a database named **`Apex Hospital`** (user `root`, password `abc`), and create the tables:

```sql
CREATE TABLE HOSPITAL_STAFF (
    USERNAME VARCHAR(50),
    PASSWORD VARCHAR(50)
);

CREATE TABLE PATIENTS (
    PATIENT_NUMBER   INT PRIMARY KEY,
    PATIENT_NAME     VARCHAR(100),
    PATIENT_AILMENT  VARCHAR(100),
    STATUS           VARCHAR(20),
    ADMITDATE        VARCHAR(20),
    DISCHARGEDATE    VARCHAR(20)
);
```

> 💡 The connection details (`jdbc:derby://localhost:1527/Apex Hospital`, user `root` / password `abc`) are defined in each frame — adjust them if your setup differs

### 2️⃣ Run the application

**Option A — pre-built JAR**

```bash
cd dist
java -jar "E-Health_Care_Management_System.jar"
```

_(The `dist/lib` folder must sit next to the JAR)_

**Option B — build from source with Ant**

```bash
ant clean
ant jar
ant run        # main class: healthlet.NewJFrame
```

**Option C — NetBeans** → open the project and press **Run (F6)**. Main class: `healthlet.NewJFrame`

<br>

## 🎓 What I Learned

- Designing **multi-window Java Swing** applications and managing navigation between `JFrames`.
- Using the **NetBeans GUI Builder** to lay out forms and wire up event-driven button handlers.
- Connecting Java to a relational database with **JDBC** — executing `INSERT` / `SELECT` / `UPDATE` queries and reading `ResultSet`s.
- Populating a Swing `JTable` dynamically from query results with `DefaultTableModel`.
- Integrating third-party components (the **JCalendar** date picker) and formatting dates with `SimpleDateFormat`.
- Implementing real business logic, like deriving a bill from date arithmetic.
- Packaging a Java desktop app into a **distributable runnable JAR** with Ant and bundled dependencies.

<br>

## 🔭 Future Improvements

- **🔒 Use `PreparedStatement`** — queries are currently built via string concatenation, which is open to SQL injection and breaks on special characters. Parameterized queries fix both.
- **♻️ Adopt try-with-resources** — to auto-close connections and prevent resource leaks.
- **🏛️ Apply MVC** — separating UI, business logic, and data access would make the app testable and maintainable.
- **🏷️ Rename auto-generated classes** — `NewJFrame1`…`NewJFrame5` → `Dashboard`, `AddPatientFrame`, etc.
- **⚙️ Make billing configurable** and add input validation.

<br>

---

<div align="center">

⭐ _If you found this project helpful or interesting, consider giving it a star!_ ⭐

</div>
