# Mainframe Insurance Management System

## 🚀 Project Summary

A COBOL/CICS-based insurance management system simulating a real-world enterprise solution.

The application supports customer management, invoice handling, and statistical reporting, all handled through multiple interactive CICS screens and backed by DB2.

## 📁 Project Structure

- `Source.cbl` – Main CICS program handling flow and user interaction  
- `CICS.bms` – BMS mapset defining the UI screens  

---

## 📌 Notes

This is a simplified version of the full solution.

The original project included additional modules, copybooks, DB2 table definitions and components which are not included here, but the main program demonstrates the overall structure and core functionality.

## 💼 Business Context

The system represents a simplified insurance platform where users can:

* Manage customer records (create, update, delete, retrieve)
* Create and manage insurance policies
* Generate and maintain invoices
* View statistics for business insights (daily, weekly, monthly, yearly)

---

## ✨ Key Features

- Multi-screen CICS application (Home, Customer, Insert, Statistics)
- Interactive user input handling via BMS maps
- DB2 integration for persistent data storage
- Real-time calculations (premium totals, insurance count, customer score)
- Statistical reporting (daily, weekly, monthly, yearly)
- Function key navigation (PF1–PF10)

---

## ⚙️ System Overview

Technologies used:

* COBOL (business logic)
* CICS (transaction & screen handling)
* BMS (screen definitions)
* DB2 (persistent storage)

---

## 🧱 Architecture

The solution includes:

* Multiple CICS screens:

  * Home screen
  * Customer portal
  * Insert screen
  * Statistics screen

* DB2 tables:

  * CUSTINFO
  * INSINFO
  * INVOICE

* Transaction-based navigation using function keys (PF1–PF10)

---

## 🔁 Transaction & Screen Flow

User interaction is handled across multiple CICS transactions using COMMAREA:

```cobol
EVALUATE TRUE
  WHEN EIBCALEN = ZERO
     PERFORM C-SEND-MAP3

  WHEN EIBAID = DFHPF4
     PERFORM C-SEND-MAP
     SET 1STSCR-SWITCH TO TRUE

  WHEN EIBAID = DFHPF5
     PERFORM C-SEND-MAP2
     SET 2NDSCR-SWITCH TO TRUE

  WHEN EIBAID = DFHPF6
     PERFORM C-SEND-MAP4
     PERFORM QE-DAILYSTAT-CUST
     PERFORM QE-WEEKLYSTAT-CUST
     PERFORM QE-MONTHLYSTAT-CUST
     PERFORM QE-YEARLYSTAT-CUST
```

---

## 🧠 Business Logic (CRUD Operations)

Example of input handling and action routing:

```cobol
EVALUATE ACTIONI
  WHEN 'S'
    PERFORM FB-SELECT
  WHEN 'D'
    PERFORM FC-DELETE
  WHEN 'U'
    PERFORM FD-UPDATE
  WHEN OTHER
    MOVE 'WRONG INPUT IN ACTION FIELD' TO MSG1O
END-EVALUATE
```

---

## 🗄️ DB2 Integration

Example of DB2 insert operation:

```cobol
EXEC SQL
  INSERT INTO INSURE.CUSTINFO
         (CUSTID,
          FIRSTNAME,
          LASTNAME,
          CADDRESS,
          EMAIL,
          PHONE,
          DOB)
VALUES
         (:DCLCUSTINFO.CUSTID,
          :FIRSTNAME,
          :LASTNAME,
          :CADDRESS,
          :EMAIL,
          :PHONE,
          :DOB)
END-EXEC
```

---

## 📊 Statistical Reporting

The system includes dynamic statistics using DB2 queries:

```cobol
EXEC SQL
  SELECT COUNT(INSNR)
    INTO :CS-DAILY-NEWINS
  FROM INSURE.INSINFO
  WHERE STARTDATE > CURRENT DATE - 1 DAY
END-EXEC
```

Supports:

* Daily statistics
* Weekly statistics
* Monthly statistics
* Yearly statistics

---

## 🖥️ BMS Screen Example

```cobol
HOMESCR  DFHMDI SIZE=(24,80)

PROJECTTXT DFHMDF POS=(05,24),
           LENGTH=33,
           ATTRB=(BRT,PROT),
           COLOR=GREEN,
           INITIAL='PROJECT -> INSURE CUSTOMER PORTAL'
```

---

## 👤 My Contribution

- Developed core COBOL program logic (INSMAIN)
- Implemented DB2 integration (SELECT, INSERT, UPDATE, DELETE)
- Built input validation and business rules
- Worked with CICS screen handling and navigation
- Contributed to system design and data flow

---

## 📸 Demo

## 🎥 Demo

This demo showcases the system running in a mainframe environment, including screen navigation and DB2 integration.

[▶️ Watch demo on YouTube](https://www.youtube.com/watch?v=9sXSpuZvVd0&t=5083s)

Direct link:  
https://www.youtube.com/watch?v=9sXSpuZvVd0&t=5083s

(The demo starts at 1:24:43)

## 📚 What I Learned

- How CICS applications handle user interaction across multiple screens  
- Working with DB2 using embedded SQL in COBOL  
- Structuring a mainframe application with clear separation between UI, logic, and data  
- Handling state between transactions using COMMAREA  
- Designing simple but realistic business logic in a legacy environment  
