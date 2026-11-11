-- =================================================================
-- SQL CHEAT SHEET & REFERENCE GUIDE
-- =================================================================

-- 1. DATABASE CREATION
CREATE DATABASE IF NOT EXISTS practice;
USE practice;

-- 2. TABLE CREATION WITH CONSTRAINTS
-- Parent Table
CREATE TABLE drivers (
    driver_id INT PRIMARY KEY,
    driver_name VARCHAR(50) NOT NULL,
    rating DECIMAL(3,2) DEFAULT 5.00
);

-- Child Table with Foreign Key, Check Constraint, and Soft Delete Flag
CREATE TABLE rides (
    ride_id INT PRIMARY KEY,
    driver_id INT,
    fare DECIMAL(8,2) CHECK (fare > 0),
    is_deleted BOOLEAN DEFAULT FALSE,
    FOREIGN KEY (driver_id) REFERENCES drivers(driver_id) ON DELETE CASCADE
);

-- 3. SAMPLE INSERTS
INSERT INTO drivers VALUES (101, 'Ramesh', 4.8), (102, 'Kumar', 4.5);
INSERT INTO rides VALUES (111, 101, 700.00), (113, 101, 800.00), (112, 102, 900.00);

-- 4. BASIC QUERIES & FILTERING (Hiding Soft Deletes)
SELECT * 
FROM rides 
WHERE is_deleted = FALSE;

-- 5. AGGREGATIONS & GROUPING
SELECT driver_id, SUM(fare) AS total_earnings, COUNT(ride_id) AS total_rides
FROM rides
WHERE is_deleted = FALSE
GROUP BY driver_id
HAVING total_earnings > 500;

-- 6. JOINS
SELECT d.driver_name, r.ride_id, r.fare
FROM drivers d
JOIN rides r ON d.driver_id = r.driver_id
WHERE r.is_deleted = FALSE;

-- 7. SOFT DELETE OPERATION (Instead of hard DELETE)
UPDATE rides 
SET is_deleted = TRUE 
WHERE ride_id = 111;
