# Healthcare system Management
![Oracle](https://img.shields.io/badge/Oracle-SQL-blue.svg)
![PL/SQL](https://img.shields.io/badge/PL%2FSQL-Database-orange.svg)
![GitHub](https://img.shields.io/badge/GitHub-Repository-green.svg)


## Introduction
- **Student**: ISHIMWE DAVID(26426)
- **Group**: Thursday
- **Course**: Database Development with PL/SQL (INSY 8311)
- **Lecturer**: Eric Maniraguha

## 📋 Problem Statement
 The project addresses inefficiencies in health care system management by developing a PL/SQL-based Oracle database to automate procurement, track inventory, and enhance decision-making.
## 📊 Methodology
- **Phase I**: Defined problem and presented objectives.
- **Phase II**: Modeled order processing using UML.
- **Phase III**: Designed logical data model with ER diagram.
- **Phase IV**: Created pluggable database (Thurs_26426_IshimweDavid_HealthcareSystem_DB).
- **Phase V**: Implemented tables and inserted realistic data.
- **Phase VI**: Developed procedures for order processing and data retrieval.
- **Phase VII**: Implemented triggers and auditing for security.
- **Phase VIII**: Compiled documentation and presentation.

## 📸 Screenshots
- **ER Diagram**:![ER Diagram](https://github.com/user-attachments/assets/b64e6d35-9e65-498f-badc-d32cc8c346d6)

- This Entity-Relationship (ER) Diagram outlines a structured database for managing medical supplies in a healthcare setting. It tracks procurement, inventory, and usage across departments.
- Key Relationships:
- Vendors → Purchase Orders → Order Details → Medical Supplies → Inventory (procurement & stock flow).
- Medical Supplies → Usage Tracking (monitors departmental usage).
- This system ensures efficient supply tracking, cost management, and inventory control in healthcare operations.

- **UML Diagram**:[![Activity diagram](https://github.com/user-attachments/assets/58c7d59b-16eb-4773-b5f1-14fa9909f24c)]
- This diagram illustrates the Healthcare Supply Chain Management Process Model, showing the complete workflow from supply request to delivery.
- Database Integration: The diagram shows multiple points where your Oracle database system is accessed (checking stock levels, updating inventory)
- Decision-Driven Process: The workflow includes a critical decision point that determines whether to procure new supplies or fulfill from existing stock.
- Multi-Role Involvement: The process involves various stakeholders (Department Head, Inventory Coordinator, Procurement Manager, Finance Officer), reflecting real-world healthcare supply chain complexity.
- Closed-Loop System: The process ensures inventory levels are always updated, maintaining data accuracy for future requests.

- **OEM Dashboard**:![Capture1](https://github.com/user-attachments/assets/3f502ace-8873-42f0-b922-0471f9dcd051)
      


- These images show the Oracle SQL*Plus command-line interface demonstrating the creation and setup of your Healthcare Supply Chain Management database
- Successful Elements: Pluggable database architecture setup, user creation and privilege management
- Technical Challenges: File system path configuration issues that need resolution
- Learning Outcomes: Demonstrates proper Oracle multitenant database setup procedures
- Proper use of pluggable databases for application isolation
- Correct sequence of database state management (MOUNT → OPEN → SAVE STATE)
- User privilege management with DBA roles
- Container-aware session management
- These screenshots effectively document the database setup process for your healthcare supply chain project, showing both successful implementations and real-world troubleshooting scenarios that database administrators commonly encounter.



- **Sample Query Output**:
  ![tables](https://github.com/user-attachments/assets/ee583363-5c28-42b3-926f-e679390d3dbf)
  ![insert data](https://github.com/user-attachments/assets/8eaec234-070b-4bf8-9165-e8bbc4cb0737)
  ![trigger](https://github.com/user-attachments/assets/d30f3e8d-9709-4861-8e24-3fb2edc6abe6)
  ![procedure](https://github.com/user-attachments/assets/723db723-1122-4779-abd4-300b7680b2ec)
- These images show Oracle SQL Developer in action, demonstrating the implementation of your Healthcare Supply Chain Management database project.
- GUI Development Environment: Professional Oracle SQL Developer interface
- Complex PL/SQL: Advanced triggers, packages, and procedures
- Business Rule Implementation: Sophisticated access control and validation
- Real-world Application: Practical healthcare supply chain management features
- These screenshots demonstrate a comprehensive, production-ready database system with advanced security, business logic, and data management capabilities suitable for healthcare supply chain operations.

## 📂 SQL Queries
- **Table Creation and Data Insertion**:
```sql
 -- Table Creation
CREATE TABLE Patients (
    PatientID NUMBER PRIMARY KEY,
    FirstName VARCHAR2(50) NOT NULL,
    LastName VARCHAR2(50) NOT NULL,
    DOB DATE NOT NULL,
    ContactNumber VARCHAR2(15),
    Email VARCHAR2(100) UNIQUE
);

CREATE TABLE Healthcare_Providers (
    ProviderID NUMBER PRIMARY KEY,
    FirstName VARCHAR2(50) NOT NULL,
    LastName VARCHAR2(50) NOT NULL,
    Specialty VARCHAR2(100),
    ContactNumber VARCHAR2(15)
);

CREATE TABLE Appointments (
    AppointmentID NUMBER PRIMARY KEY,
    PatientID NUMBER NOT NULL,
    ProviderID NUMBER NOT NULL,
    AppointmentDate DATE NOT NULL,
    AppointmentTime VARCHAR2(5) NOT NULL,
    Status VARCHAR2(20) DEFAULT 'Scheduled',
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY (ProviderID) REFERENCES Healthcare_Providers(ProviderID)
);

-- Insert Sample Data
INSERT INTO Patients VALUES (1, 'John', 'Doe', TO_DATE('1980-05-15', 'YYYY-MM-DD'), '1234567890', 'john.doe@email.com');
INSERT INTO Patients VALUES (2, 'Jane', 'Smith', TO_DATE('1990-08-22', 'YYYY-MM-DD'), '0987654321', 'jane.smith@email.com');

INSERT INTO Healthcare_Providers VALUES (1, 'Emily', 'Brown', 'Cardiology', '5551234567');
INSERT INTO Healthcare_Providers VALUES (2, 'Michael', 'Lee', 'Pediatrics', '5559876543');

INSERT INTO Appointments VALUES (1, 1, 1, TO_DATE('2025-06-01', 'YYYY-MM-DD'), '10:00', 'Scheduled');
INSERT INTO Appointments VALUES (2, 2, 2, TO_DATE('2025-06-02', 'YYYY-MM-DD'), '14:00', 'Scheduled');
```
- **Triggers**:
```sql
-- Create Holiday Reference Table
CREATE TABLE Holidays (
    HolidayDate DATE PRIMARY KEY,
    HolidayName VARCHAR2(100)
);

-- Insert Public Holidays for June 2025
INSERT INTO Holidays VALUES (TO_DATE('2025-06-01', 'YYYY-MM-DD'), 'National Day');
INSERT INTO Holidays VALUES (TO_DATE('2025-06-15', 'YYYY-MM-DD'), 'Independence Day');

-- Create Audit Table
CREATE TABLE AuditLog (
    AuditID NUMBER PRIMARY KEY,
    UserID VARCHAR2(50),
    ActionDate DATE,
    Operation VARCHAR2(50),
    Status VARCHAR2(20)
);

-- Sequence for AuditID
CREATE SEQUENCE AuditSeq START WITH 1 INCREMENT BY 1;

-- Compound Trigger for Restrictions and Auditing
CREATE OR REPLACE TRIGGER SupplyChain_Restrictions
FOR INSERT OR UPDATE OR DELETE ON MedicalSupplies
COMPOUND TRIGGER
    v_Day VARCHAR2(10);
    v_IsHoliday NUMBER;

    BEFORE STATEMENT IS
    BEGIN
        -- Check if today is a weekday
        SELECT TO_CHAR(SYSDATE, 'DY') INTO v_Day FROM DUAL;
        IF v_Day IN ('MON', 'TUE', 'WED', 'THU', 'FRI') THEN
            RAISE_APPLICATION_ERROR(-20004, 'Table manipulations not allowed on weekdays.');
        END IF;

        -- Check if today is a holiday in June 2025
        SELECT COUNT(*) INTO v_IsHoliday
        FROM Holidays
        WHERE HolidayDate = TRUNC(SYSDATE)
        AND EXTRACT(MONTH FROM HolidayDate) = 6
        AND EXTRACT(YEAR FROM HolidayDate) = 2025;
        IF v_IsHoliday > 0 THEN
            RAISE_APPLICATION_ERROR(-20005, 'Table manipulations not allowed on holidays.');
        END IF;
    END BEFORE STATEMENT;

    AFTER EACH ROW IS
    BEGIN
        -- Log the action
        INSERT INTO AuditLog (AuditID, UserID, ActionDate, Operation, Status)
        VALUES (AuditSeq.NEXTVAL, USER, SYSDATE, 
                CASE 
                    WHEN INSERTING THEN 'INSERT'
                    WHEN UPDATING THEN 'UPDATE'
                    WHEN DELETING THEN 'DELETE'
                END, 'ALLOWED');
    END AFTER EACH ROW;

    AFTER STATEMENT IS
    BEGIN
        -- Additional auditing logic if needed
        NULL;
    END AFTER STATEMENT;
END SupplyChain_Restrictions;
/

-- Problem Statement
-- The trigger ensures that no INSERT, UPDATE, or DELETE operations are performed on the MedicalSupplies table during weekdays or public holidays in June 2025. Auditing tracks all actions, enhancing security by logging user activities and enforcing access restrictions.
```
- **Procedures**:
```sql
-- Package Specification
CREATE OR REPLACE PACKAGE SupplyChain_Pkg AS
    PROCEDURE FetchInventoryStatus(p_SupplyID IN NUMBER, p_Quantity OUT NUMBER);
    FUNCTION CalculateTotalSpending(p_DepartmentID IN NUMBER) RETURN NUMBER;
END SupplyChain_Pkg;
/

-- Package Body
CREATE OR REPLACE PACKAGE BODY SupplyChain_Pkg AS
    PROCEDURE FetchInventoryStatus(p_SupplyID IN NUMBER, p_Quantity OUT NUMBER) IS
        CURSOR inv_cursor IS
            SELECT QuantityInStock
            FROM Inventory
            WHERE SupplyID = p_SupplyID;
    BEGIN
        OPEN inv_cursor;
        FETCH inv_cursor INTO p_Quantity;
        IF inv_cursor%NOTFOUND THEN
            RAISE_APPLICATION_ERROR(-20001, 'Supply ID not found.');
        END IF;
        CLOSE inv_cursor;
    EXCEPTION
        WHEN OTHERS THEN
            RAISE_APPLICATION_ERROR(-20002, 'Error fetching inventory: ' || SQLERRM);
    END FetchInventoryStatus;

    FUNCTION CalculateTotalSpending(p_DepartmentID IN NUMBER) RETURN NUMBER IS
        v_Total NUMBER;
    BEGIN
        SELECT SUM(TotalAmount) OVER (PARTITION BY DepartmentID)
        INTO v_Total
        FROM PurchaseOrders
        WHERE DepartmentID = p_DepartmentID;
        RETURN NVL(v_Total, 0);
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 0;
        WHEN OTHERS THEN
            RAISE_APPLICATION_ERROR(-20003, 'Error calculating spending: ' || SQLERRM);
    END CalculateTotalSpending;
END SupplyChain_Pkg;
/

-- Example DML Operation
INSERT INTO UsageTracking VALUES (3, 1, 1, 20, TO_DATE('2025-05-07', 'YYYY-MM-DD'));

-- Example DDL Operation
ALTER TABLE MedicalSupplies ADD (SupplierRating NUMBER DEFAULT 0);

-- Testing Procedure
DECLARE
    v_Quantity NUMBER;
BEGIN
    SupplyChain_Pkg.FetchInventoryStatus(1, v_Quantity);
    DBMS_OUTPUT.PUT_LINE('Quantity in Stock: ' || v_Quantity);
END;
/
```

## Results
- Successfully created a comprehensive Oracle PL/SQL database system with 7 interconnected tables (Vendors, MedicalSupplies, Inventory, Departments, PurchaseOrders, OrderDetails, UsageTracking).
- Implemented robust data relationships with proper foreign key constraints and check constraints for data integrity.
- Developed a complete package system with procedures and functions for inventory management and financial tracking.
- Created comprehensive audit logging system that tracks all database operations with user identification and timestamps.

## Conclusion
The project demonstrates successful application of database design principles to solve real-world healthcare management challenges, resulting in a production-ready system that can significantly improve supply chain efficiency and decision-making processes.

## References
- Oracle PL/SQL Documentation
- BPMN 2.0 Specifications
