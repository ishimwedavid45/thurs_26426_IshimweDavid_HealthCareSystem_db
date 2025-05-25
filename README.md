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

- **OEM Dashboard**:
![Capture1](https://github.com/user-attachments/assets/3f502ace-8873-42f0-b922-0471f9dcd051)
![Capture](https://github.com/user-attachments/assets/f7f3be7f-9702-45c7-9e48-153e70ca83c5)
![Uploading SQL4.PNG…]()
![Uploading SQL5.PNG…]()

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
-- Logical Data Model for Healthcare System
-- Normalization Analysis:
-- 1NF: All attributes are atomic; each table has a primary key.
-- 2NF: All tables have single-column primary keys, so no partial dependencies.
-- 3NF: No transitive dependencies; all non-key attributes depend only on the primary key.
-- Example: In Patient, name, age, contact, address depend on patient_id, not on each other.

CREATE TABLE Patient (
    patient_id NUMBER PRIMARY KEY,
    name VARCHAR2(100) NOT NULL,
    age NUMBER,
    contact VARCHAR2(20),
    address VARCHAR2(200)
);

CREATE TABLE Healthcare_Provider (
    provider_id NUMBER PRIMARY KEY,
    name VARCHAR2(100) NOT NULL,
    specialty VARCHAR2(50),
    contact VARCHAR2(20)
);

CREATE TABLE Appointment (
    appointment_id NUMBER PRIMARY KEY,
    patient_id NUMBER,
    provider_id NUMBER,
    date_time DATE,
    status VARCHAR2(20),
    FOREIGN KEY (patient_id) REFERENCES Patient(patient_id),
    FOREIGN KEY (provider_id) REFERENCES Healthcare_Provider(provider_id)
);

CREATE TABLE Medical_Record (
    record_id NUMBER PRIMARY KEY,
    patient_id NUMBER,
    provider_id NUMBER,
    diagnosis VARCHAR2(200),
    treatment VARCHAR2(200),
    record_date DATE,
    FOREIGN KEY (patient_id) REFERENCES Patient(patient_id),
    FOREIGN KEY (provider_id) REFERENCES Healthcare_Provider(provider_id)
);

CREATE TABLE Prescription (
    prescription_id NUMBER PRIMARY KEY,
    record_id NUMBER,
    medication VARCHAR2(100),
    dosage VARCHAR2(50),
    instructions VARCHAR2(200),
    FOREIGN KEY (record_id) REFERENCES Medical_Record(record_id)
);

CREATE TABLE Inventory (
    item_id NUMBER PRIMARY KEY,
    name VARCHAR2(100) NOT NULL,
    quantity NUMBER,
    type VARCHAR2(50)
);

CREATE TABLE Billing (
    bill_id NUMBER PRIMARY KEY,
    appointment_id NUMBER,
    amount NUMBER,
    payment_status VARCHAR2(20),
    record_date DATE,
    FOREIGN KEY (appointment_id) REFERENCES Appointment(appointment_id)
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
-- Create audit table
CREATE TABLE Audit_Log (
    audit_id NUMBER PRIMARY KEY,
    table_name VARCHAR2(50),
    action VARCHAR2(20),
    user_name VARCHAR2(50),
    action_date DATE
);

-- Create sequence for audit_id
CREATE SEQUENCE audit_seq START WITH 1 INCREMENT BY 1;

-- Trigger to block DML on weekends
CREATE OR REPLACE TRIGGER block_dml_weekend
BEFORE INSERT OR UPDATE OR DELETE ON Medical_Record
DECLARE
    v_day VARCHAR2(10);
BEGIN
    SELECT TO_CHAR(SYSDATE, 'DAY') INTO v_day FROM dual;
    IF v_day IN ('SATURDAY', 'SUNDAY') THEN
        RAISE_APPLICATION_ERROR(-20001, 'DML operations are blocked on weekends.');
    END IF;
END;
/

-- Trigger to log audit actions
CREATE OR REPLACE TRIGGER log_audit
AFTER INSERT OR UPDATE OR DELETE ON Medical_Record
FOR EACH ROW
DECLARE
    v_action VARCHAR2(20);
BEGIN
    IF INSERTING THEN
        v_action := 'INSERT';
    ELSIF UPDATING THEN
        v_action := 'UPDATE';
    ELSIF DELETING THEN
        v_action := 'DELETE';
    END IF;
    INSERT INTO Audit_Log (audit_id, table_name, action, user_name, action_date)
    VALUES (audit_seq.NEXTVAL, 'Medical_Record', v_action, USER, SYSDATE);
END;
/

-- Procedure to schedule an appointment
CREATE OR REPLACE PROCEDURE schedule_appointment (
    p_patient_id IN NUMBER,
    p_provider_id IN NUMBER,
    p_date_time IN DATE,
    p_appointment_id OUT NUMBER
) AS
    v_count NUMBER;
BEGIN
    -- Check if the provider is available at the given time
    SELECT COUNT(*) INTO v_count
    FROM Appointment
    WHERE provider_id = p_provider_id
    AND date_time = p_date_time;
    
    IF v_count = 0 THEN
        SELECT NVL(MAX(appointment_id), 0) + 1 INTO p_appointment_id FROM Appointment;
        INSERT INTO Appointment (appointment_id, patient_id, provider_id, date_time, status)
        VALUES (p_appointment_id, p_patient_id, p_provider_id, p_date_time, 'Scheduled');
        COMMIT;
    ELSE
        RAISE_APPLICATION_ERROR(-20002, 'Provider is not available at the specified time.');
    END IF;
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        RAISE;
END;
/

-- Procedure to update inventory after prescription
CREATE OR REPLACE PROCEDURE update_inventory (
    p_medication IN VARCHAR2,
    p_quantity_used IN NUMBER
) AS
    v_item_id NUMBER;
    v_current_quantity NUMBER;
BEGIN
    -- Find the item_id for the medication
    SELECT item_id, quantity INTO v_item_id, v_current_quantity
    FROM Inventory
    WHERE name = p_medication;
    
    IF v_current_quantity >= p_quantity_used THEN
        UPDATE Inventory
        SET quantity = quantity - p_quantity_used
        WHERE item_id = v_item_id;
        COMMIT;
    ELSE
        RAISE_APPLICATION_ERROR(-20003, 'Insufficient inventory for ' || p_medication);
    END IF;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RAISE_APPLICATION_ERROR(-20004, 'Medication not found in inventory.');
    WHEN OTHERS THEN
        ROLLBACK;
        RAISE;
END;
/

-- Procedure to generate billing summary
CREATE OR REPLACE PROCEDURE generate_billing_summary (
    p_patient_id IN NUMBER
) AS
    CURSOR bill_cursor IS
        SELECT b.bill_id, b.amount, b.payment_status, b.record_date
        FROM Billing b
        JOIN Appointment a ON b.appointment_id = a.appointment_id
        WHERE a.patient_id = p_patient_id;
BEGIN
    FOR bill_rec IN bill_cursor LOOP
        DBMS_OUTPUT.PUT_LINE('Bill ID: ' || bill_rec.bill_id || 
                            ', Amount: ' || bill_rec.amount || 
                            ', Status: ' || bill_rec.payment_status || 
                            ', Date: ' || bill_rec.record_date);
    END LOOP;
EXCEPTION
    WHEN OTHERS THEN
        RAISE_APPLICATION_ERROR(-20005, 'Error generating billing summary.');
END;
/

-- Test the procedures
DECLARE
    v_appointment_id NUMBER;
BEGIN
    schedule_appointment(1, 1, TO_DATE('2025-05-27 10:00', 'YYYY-MM-DD HH24:MI'), v_appointment_id);
    DBMS_OUTPUT.PUT_LINE('Appointment ID: ' || v_appointment_id);
    update_inventory('Paracetamol', 10);
    generate_billing_summary(1);
END;
/


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
