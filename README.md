# Med-Enter-system
Med-Enter Description


# PL/SQL Medicine Batch Entry and Validation System

## student Information

Student Name: DUSHIME Sangwa Bertrand

Student ID: 2797

Course: PL/SQL Database

Institution : AUCA

Date: 9th December 2025

## Project Overview

Med-Enter is a secure and automated PL/SQL-based pharmaceutical inventory system designed to manage medicine batch entries safely and efficiently.
It ensures that expired or near-expiry drugs (less than 60 days) are automatically rejected, protecting patient safety and maintaining accurate stock levels.

The system uses a single atomic stored procedure to:

Validate medicine expiry

Insert batch data

Update stock levels

Generate alerts

Maintain a full audit trail

## Key Features

🔐 Atomic Batch Entry
All operations are handled inside one secure stored procedure:
ADD_NEW_MEDICINE_BATCH

🚫 Critical Safety Check
Automatically rejects:

Expired medicines

Medicines with less than 60 days before expiry

📦 Real-Time Stock Management
Automatically updates total stock in the MEDICINES table.

🚨 Automated Alerts System
Generates LOW_STOCK alerts when quantity falls below reorder_level.

## Comprehensive Audit Trail
All actions (successful entries, rejections, warnings) are recorded in the ALERTS_LOG table.

🗃️ Database Schema
1️⃣ MEDICINES

Stores master information for all medicines.

Column Name	Description
med_id (PK)	Unique Medicine ID
med_name	Medicine Name
reorder_level	Minimum stock before reordering
2️⃣ BATCH_INVENTORY

Tracks individual medicine batches.

Column Name	Description
batch_id (PK)	Unique Batch ID
med_id (FK)	Reference to MEDICINES
batch_number	Supplier batch number
quantity_in_stock	Quantity available
expiry_date	Expiration date
3️⃣ ALERTS_LOG

Stores all system alerts and audit records.

Column Name	Description
alert_id (PK)	Unique Alert ID
med_id (FK)	Related medicine
alert_type	LOW_STOCK / EXPIRED / REJECTED
message	Alert description
alert_date	Date and time of alert
🔗 Relationships

One-to-Many (1:M)

MEDICINES → BATCH_INVENTORY

MEDICINES → ALERTS_LOG

##⚙️ Installation & Setup
##✅ Prerequisites

Oracle Database / Oracle XE

SQL Developer or SQL*Plus

##✅ Setup Steps

Create Database Tables

Run the DDL script for:

MEDICINES

BATCH_INVENTORY

ALERTS_LOG

Create PL/SQL Objects

Main Procedure:

ADD_NEW_MEDICINE_BATCH

Helper Procedures:

check_expiry_date

log_alert

## Usage Guide
✅ 1. Add New Medicine Batch
DECLARE
  v_status  VARCHAR2(50);
  v_message VARCHAR2(500);
BEGIN
  ADD_NEW_MEDICINE_BATCH(
    p_med_id       => 1,
    p_quantity     => 500,
    p_batch_number => 'BATCH001',
    p_expiry_date  => ADD_MONTHS(SYSDATE, 12),
    p_status       => v_status,
    p_message      => v_message
  );

  DBMS_OUTPUT.PUT_LINE('Status: ' || v_status);
  DBMS_OUTPUT.PUT_LINE('Message: ' || v_message);
END;
/

✅ 2. View Inventory Status
Description	Command
View All Batches	SELECT * FROM BATCH_INVENTORY ORDER BY expiry_date;
View Medicine Batches	EXEC view_medicine_batches(1);
View Alerts History	SELECT * FROM ALERTS_LOG ORDER BY alert_date DESC;
🏁 Conclusion

The Med-Enter System improves pharmaceutical stock safety through:

Automated expiry validation

Secure transaction handling

Real-time stock updates

Professional audit logging

This makes it suitable for hospitals, pharmacies, and medical stores.
