# DataBaseManagementSystem-Project
Advanced SQL Medical Inventory System 🏥 | A comprehensive relational database project for tracking inventory, patient prescriptions, and supplier logistics. Features predictive analytics to forecast stock needs. Built with pure SQL.




-- 1. Suppliers
CREATE TABLE Suppliers (
    SupplierID INT PRIMARY KEY AUTO_INCREMENT,
    SupplierName VARCHAR(100) NOT NULL,
    ContactPerson VARCHAR(100),
    Phone VARCHAR(20),
    Email VARCHAR(100),
    Address TEXT
);

-- 2. Medical Items
CREATE TABLE MedicalItems (
    ItemID INT PRIMARY KEY AUTO_INCREMENT,
    ItemName VARCHAR(100) NOT NULL,
    Category VARCHAR(50),
    Unit VARCHAR(20),
    ReorderLevel INT,
    ExpiryPeriodMonths INT
);

-- 3. Inventory
CREATE TABLE Inventory (
    InventoryID INT PRIMARY KEY AUTO_INCREMENT,
    ItemID INT,
    QuantityOnHand INT,
    QuantityUsed INT DEFAULT 0,
    ExpiryDate DATE,
    LastUpdated TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (ItemID) REFERENCES MedicalItems(ItemID)
);

-- 4. Purchase Orders
CREATE TABLE PurchaseOrders (
    OrderID INT PRIMARY KEY AUTO_INCREMENT,
    SupplierID INT,
    OrderDate DATE,
    Status VARCHAR(20),
    TotalAmount DECIMAL(10, 2),
    FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID)
);

-- 5. Order Items
CREATE TABLE OrderItems (
    OrderItemID INT PRIMARY KEY AUTO_INCREMENT,
    OrderID INT,
    ItemID INT,
    Quantity INT,
    UnitPrice DECIMAL(10, 2),
    FOREIGN KEY (OrderID) REFERENCES PurchaseOrders(OrderID),
    FOREIGN KEY (ItemID) REFERENCES MedicalItems(ItemID)
);

-- 6. Usage Logs
CREATE TABLE UsageLogs (
    UsageID INT PRIMARY KEY AUTO_INCREMENT,
    ItemID INT,
    QuantityUsed INT,
    Department VARCHAR(100),
    UsedBy VARCHAR(100),
    UsageDate DATE,
    FOREIGN KEY (ItemID) REFERENCES MedicalItems(ItemID)
);

-- 7. Transport
CREATE TABLE Transport (
    TransportID INT PRIMARY KEY AUTO_INCREMENT,
    TransportMode VARCHAR(50),
    CarrierName VARCHAR(100),
    TrackingNumber VARCHAR(50),
    DepartureDate DATE,
    ArrivalDate DATE,
    Origin VARCHAR(100),
    Destination VARCHAR(100),
    OrderID INT,
    FOREIGN KEY (OrderID) REFERENCES PurchaseOrders(OrderID)
);

-- 8. Patients
CREATE TABLE Patients (
    PatientID INT PRIMARY KEY AUTO_INCREMENT,
    FirstName VARCHAR(100),
    LastName VARCHAR(100),
    Gender VARCHAR(10),
    DateOfBirth DATE,
    Phone VARCHAR(20),
    Email VARCHAR(100),
    Address TEXT
);

-- 9. Prescriptions
CREATE TABLE Prescriptions (
    PrescriptionID INT PRIMARY KEY AUTO_INCREMENT,
    DoctorName VARCHAR(100),
    PatientID INT,
    ItemID INT,
    Quantity INT,
    PrescriptionDate DATE,
    Notes TEXT,
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY (ItemID) REFERENCES MedicalItems(ItemID)
);


CREATE TABLE PatientConditions (
    ConditionID INT PRIMARY KEY AUTO_INCREMENT,
    PatientID INT,
    DiseaseName VARCHAR(100) NOT NULL,
    InferenceDate DATE,
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID)
);


-- INSERT SAMPLE DATA


-- Suppliers
INSERT INTO Suppliers (SupplierName, ContactPerson, Phone, Email, Address) VALUES
('MediTrust Solutions Inc.', 'Johnathan Miller', '617-555-0101', 'j.miller@meditrust.com', '100 Medical Park Dr, Boston, MA 02115'),
('PharmaLogix Distributors', 'Rebecca Chen', '212-555-0202', 'r.chen@pharmalogix.com', '250 Broadway, New York, NY 10007'),
('Global Health Supplies', 'Daniel Rodriguez', '312-555-0303', 'd.rodriguez@ghs.com', '333 W Wacker Dr, Chicago, IL 60606'),
('BioMed Innovations', 'Sophia Lee', '415-555-0404', 's.lee@biomedin.com', '1 Embarcadero Center, San Francisco, CA 94111'),
('United Medical Group', 'Thomas Jefferson', '202-555-0505', 't.jefferson@umg.net', '1600 K St NW, Washington, D.C. 20006'),
('Capital Surgical Instruments', 'Maria Santos', '713-555-0606', 'm.santos@capitalsurg.com', '800 Walker St, Houston, TX 77002'),
('Apex Pharma', 'Michael O''Connell', '305-555-0707', 'm.oconnell@apexpharma.com', '123 Biscayne Blvd, Miami, FL 33132'),
('Medi-Equip International', 'Fatima Al-Hamad', '404-555-0808', 'f.hamad@mediequip.com', '555 Peachtree St NE, Atlanta, GA 30308'),
('PharmaWorld Ltd.', 'Elena Petrova', '972-555-1010', 'e.petrova@pharmaworld.com', '1200 Commerce St, Dallas, TX 75202'),
('HealthBridge Distributors', 'Jessica Brown', '786-555-1212', 'j.brown@healthbridge.com', '1111 Brickell Ave, Miami, FL 33131'),
('MedServ Logistics', 'David Kim', '857-555-1313', 'd.kim@medserv.com', '45 Main St, Cambridge, MA 02142'),
('Optima Medical Systems', 'Sarah Johnson', '310-555-1414', 's.johnson@optimamed.com', '2100 Pico Blvd, Santa Monica, CA 90405'),
('TheraGoods Inc.', 'Alexei Volkov', '407-555-1515', 'a.volkov@theragoods.com', '1500 E Colonial Dr, Orlando, FL 32803');


-- Medical Items
INSERT INTO MedicalItems (ItemName, Category, Unit, ReorderLevel, ExpiryPeriodMonths) VALUES
('N95 Masks', 'PPE', 'Box', 100, 24),
('IV Fluids', 'Medication', 'Bottle', 50, 12),
('Latex Gloves', 'PPE', 'Box', 200, 36),
('Syringes 10ml', 'Equipment', 'Pack', 75, 48),
('Paracetamol 500mg', 'Medication', 'Strip', 100, 24),
('Amoxicillin 250mg', 'Antibiotic', 'Strip', 80, 18),
('Insulin Vial', 'Medication', 'Vial', 40, 6),
('COVID-19 Vaccine', 'Vaccine', 'Vial', 300, 6),
('Metformin 500mg', 'Medication', 'Strip', 90, 36),
('Salbutamol Inhaler', 'Medication', 'Unit', 60, 24),
('Surgical Gowns', 'PPE', 'Unit', 150, 60),
('Alcohol Swabs', 'Disposables', 'Box', 300, 36),
('Sterile Gauze Pads', 'Disposables', 'Pack', 250, 48),
('Blood Pressure Cuff', 'Equipment', 'Unit', 20, 120),
('Stethoscope', 'Equipment', 'Unit', 10, 240),
('Hydrocortisone Cream', 'Medication', 'Tube', 40, 30),
('Adhesive Bandages', 'Disposables', 'Box', 500, 60),
('Infusion Pump', 'Equipment', 'Unit', 5, 180),
('Thermometers', 'Equipment', 'Unit', 30, 240),
('Fentanyl Citrate', 'Medication', 'Vial', 15, 24),
('Surgical Tape', 'Disposables', 'Roll', 80, 48),
('Pulse Oximeter', 'Equipment', 'Unit', 25, 120),
('Exam Gloves (Nitrile)', 'PPE', 'Box', 250, 48),
('Antiseptic Wipes', 'Disposables', 'Pack', 150, 24),
('Catheters', 'Equipment', 'Unit', 50, 36),
('Lidocaine', 'Medication', 'Vial', 35, 30),
('Hand Sanitizer', 'Disposables', 'Gallon', 75, 36),
('Face Shields', 'PPE', 'Unit', 90, 60),
('Disposable Bed Linens', 'Disposables', 'Unit', 400, 60),
('Defibrillator', 'Equipment', 'Unit', 2, 240);


-- Inventory
INSERT INTO Inventory (ItemID, QuantityOnHand, QuantityUsed, ExpiryDate) VALUES
(1, 450, 50, '2026-10-15'),
(2, 200, 30, '2025-08-20'),
(3, 750, 100, '2027-01-30'),
(4, 300, 40, '2026-11-25'),
(5, 600, 100, '2026-09-01'),
(6, 350, 80, '2026-03-15'),
(7, 180, 30, '2025-12-05'),
(8, 800, 500, '2025-11-10'),
(9, 550, 150, '2027-02-20'),
(10, 250, 70, '2026-07-10'),
(11, 250, 50, '2029-12-31'),
(12, 600, 150, '2027-05-20'),
(13, 500, 80, '2028-01-10'),
(14, 25, 5, '2030-03-01'),
(15, 12, 2, '2035-06-15'),
(16, 80, 10, '2027-04-20'),
(17, 900, 200, '2030-01-01'),
(18, 6, 1, '2032-11-10'),
(19, 45, 15, '2031-09-05'),
(20, 25, 5, '2026-08-01'),
(21, 150, 20, '2028-06-25'),
(22, 35, 10, '2031-07-30'),
(23, 700, 120, '2029-05-15'),
(24, 300, 50, '2026-12-20'),
(25, 80, 15, '2028-09-10'),
(26, 60, 10, '2027-03-05'),
(27, 200, 30, '2029-02-15'),
(28, 120, 20, '2030-10-01'),
(29, 600, 100, '2030-11-20'),
(30, 3, 0, '2035-08-01');


-- Purchase Orders
INSERT INTO PurchaseOrders (SupplierID, OrderDate, Status, TotalAmount) VALUES
(1, '2025-07-25', 'Delivered', 9500.00),
(3, '2025-08-01', 'Shipped', 11500.00),
(5, '2025-08-10', 'Ordered', 5750.00),
(7, '2025-08-15', 'Delivered', 2200.00),
(9, '2025-08-20', 'Shipped', 9300.00);


-- Order Items
INSERT INTO OrderItems (OrderID, ItemID, Quantity, UnitPrice) VALUES
(1, 1, 300, 15.00),
(1, 3, 200, 10.00),
(1, 11, 150, 20.00),
(2, 14, 10, 250.00),
(2, 18, 2, 4500.00),
(3, 5, 400, 5.50),
(3, 6, 250, 7.00),
(3, 9, 300, 6.00),
(4, 12, 500, 2.00),
(4, 17, 800, 1.50),
(5, 8, 400, 20.00),
(5, 26, 100, 8.00),
(5, 28, 50, 10.00);


-- Usage Logs
INSERT INTO UsageLogs (ItemID, QuantityUsed, Department, UsedBy, UsageDate) VALUES
(1, 40, 'Emergency', 'Dr. Smith', '2025-09-01'),
(3, 120, 'Surgery', 'Nurse Johnson', '2025-09-02'),
(5, 50, 'Outpatient Clinic', 'Nurse Anne', '2025-09-03'),
(6, 30, 'Pediatrics', 'Dr. Patel', '2025-09-04'),
(10, 5, 'Respiratory Therapy', 'Dr. Lee', '2025-09-05'),
(12, 80, 'Emergency', 'Paramedic Tom', '2025-09-06'),
(17, 150, 'General Ward', 'Nurse Emily', '2025-09-07'),
(23, 100, 'Dentistry', 'Dr. Chan', '2025-09-08'),
(27, 25, 'ICU', 'Nurse David', '2025-09-09'),
(28, 15, 'Emergency', 'Dr. Ryan', '2025-09-10'),
(2, 20, 'ICU', 'Nurse Sarah', '2025-09-11'),
(13, 60, 'Surgery', 'Dr. Miller', '2025-09-12'),
(24, 75, 'General Ward', 'Nurse Kim', '2025-09-13'),
(26, 5, 'Anesthesia', 'Dr. Garcia', '2025-09-14'),
(29, 200, 'Housekeeping', 'Alex Brown', '2025-09-15');


-- Transport
INSERT INTO Transport (TransportID, TransportMode, CarrierName, TrackingNumber, DepartureDate, ArrivalDate, Origin, Destination, OrderID) VALUES
(1, 'Air Cargo', 'Swift Global Freight', 'SGFA987654', '2025-07-26', '2025-07-28', 'Miami', 'San Diego', 1),
(2, 'Truck', 'Roadway Express', 'RRX321654', '2025-08-02', NULL, 'New York', 'San Diego', 2),
(3, 'Sea Freight', 'Oceanic Logistics', 'OL555444', '2025-08-12', NULL, 'Shanghai', 'San Diego', 3),
(4, 'Air Cargo', 'AeroShip Worldwide', 'ASW654987', '2025-08-16', '2025-08-18', 'Houston', 'San Diego', 4),
(5, 'Truck', 'City Courier Service', 'CCS112233', '2025-08-21', NULL, 'Los Angeles', 'San Diego', 5);


-- Patients
INSERT INTO Patients (FirstName, LastName, Gender, DateOfBirth, Phone, Email, Address) VALUES
('Pranav', 'yaparthi', 'Male', '2006-05-09', '555-1000', 'john.doe@email.com', '123 Main St, San Diego, CA'),
('Anna', 'Lee', 'Female', '1990-03-22', '555-2000', 'anna.lee@email.com', '456 Hilltop Rd, San Francisco, CA'),
('Mike', 'Chan', 'Male', '1978-11-05', '555-3000', 'mike.chan@email.com', '789 Ocean Blvd, LA, CA'),
('Sarah', 'Kim', 'Female', '1995-09-10', '555-4000', 'sarah.kim@email.com', '321 Sunset Dr, Sacramento, CA'),
('Tom', 'Rivera', 'Male', '1982-02-28', '555-5000', 'tom.rivera@email.com', '987 Forest Ln, Fresno, CA'),
('Emily', 'Jones', 'Female', '2001-07-20', '555-6000', 'emily.jones@email.com', '101 Pine St, San Diego, CA'),
('David', 'Miller', 'Male', '1965-04-12', '555-7000', 'david.miller@email.com', '202 Oak Ave, Berkeley, CA'),
('Olivia', 'Davis', 'Female', '1958-10-30', '555-8000', 'olivia.davis@email.com', '303 Elm Rd, San Jose, CA'),
('Samir', 'Khan', 'Male', '1993-01-25', '555-9000', 'samir.khan@email.com', '404 Maple Ln, Irvine, CA'),
('Jessica', 'White', 'Female', '1971-08-08', '555-1111', 'jessica.white@email.com', '505 Cedar Blvd, Anaheim, CA'),
('Ryan', 'Garcia', 'Male', '1988-12-03', '555-2222', 'ryan.garcia@email.com', '606 Birch Way, Pasadena, CA'),
('Laura', 'Hall', 'Female', '1997-05-18', '555-3333', 'laura.hall@email.com', '707 Spruce St, Long Beach, CA'),
('Chris', 'Martinez', 'Male', '1980-09-02', '555-4444', 'chris.martinez@email.com', '808 Redwood Dr, Oakland, CA'),
('Chloe', 'Wilson', 'Female', '2005-02-14', '555-5555', 'chloe.wilson@email.com', '909 Pinecone Rd, Santa Monica, CA'),
('Daniel', 'Brown', 'Male', '1975-06-27', '555-6666', 'daniel.brown@email.com', '111 Sycamore Ave, Napa, CA');


-- Prescriptions
INSERT INTO Prescriptions (DoctorName, PatientID, ItemID, Quantity, PrescriptionDate, Notes) VALUES
('Dr. Thaniya meghana', 1, 5, 2, '2025-09-01', 'For fever. Take every 6 hours.'),
('Dr. Liam Carter', 2, 6, 1, '2025-09-02', 'Antibiotic for 7 days.'),
('Dr. Marcus Cole', 3, 7, 1, '2025-09-03', 'Daily dose for insulin-dependent diabetes.'),
('Dr. Ava Rodriguez', 4, 9, 2, '2025-09-04', 'For blood sugar management.'),
('Dr. Evelyn Reed', 5, 10, 1, '2025-09-05', 'Use as needed for asthma.'),
('Dr. Liam Carter', 6, 5, 1, '2025-09-06', 'For pain relief after surgery.'),
('Dr. Evelyn Reed', 7, 9, 2, '2025-09-07', 'Continue current dosage for blood sugar control.'),
('Dr. Marcus Cole', 8, 16, 1, '2025-09-08', 'For skin inflammation, apply twice daily.'),
('Dr. Ava Rodriguez', 9, 26, 1, '2025-09-08', 'Local anesthetic for minor procedure.'),
('Dr. Evelyn Reed', 10, 6, 1, '2025-09-09', 'Refill on antibiotic, 7-day course.'),
('Dr. Liam Carter', 11, 5, 2, '2025-09-09', 'For fever and body ache.'),
('Dr. Sofia Miller', 12, 10, 1, '2025-09-10', 'Inhaler for exercise-induced asthma.'),
('Dr. Marcus Cole', 13, 9, 2, '2025-09-10', 'Follow-up prescription for diabetes.'),
('Dr. Ava Rodriguez', 14, 6, 1, '2025-09-11', 'To treat a bacterial infection.'),
('Dr. Beast', 15, 5, 1, '2025-09-11', 'General pain management.');



INSERT INTO PatientConditions (PatientID, DiseaseName, InferenceDate)
SELECT PatientID,DiseaseName,MIN(PrescriptionDate) AS InferenceDate -- Use the earliest prescription date as the inference date
FROM (SELECT  p.PatientID,p.PrescriptionDate,
        CASE
            WHEN p.ItemID = 5 THEN 'Fever / Pain'
            WHEN p.ItemID = 6 THEN 'Bacterial Infection'
            WHEN p.ItemID IN (7, 9) THEN 'Diabetes' -- Grouping Insulin and Metformin
            WHEN p.ItemID = 10 THEN 'Asthma'
            WHEN p.ItemID = 16 THEN 'Skin Inflammation'
            WHEN p.ItemID = 26 THEN 'Minor Procedure Recovery'
            ELSE NULL -- We will ignore medicines that don't map to a clear condition
        END AS DiseaseName
   FROM Prescriptions p
) AS InferredConditions
WHERE DiseaseName IS NOT NULL
GROUP BY PatientID, DiseaseName;



-- DISPLAY ALL DATA (optional)



SELECT mi.ItemName,i.QuantityOnHand FROM MedicalItems mi JOIN Inventory i ON mi.ItemID = i.ItemID ORDER BY mi.ItemName;

SELECT mi.ItemName,i.QuantityOnHand,mi.ReorderLevel FROM MedicalItems mi JOIN Inventory i ON mi.ItemID = i.ItemID WHERE i.QuantityOnHand < mi.ReorderLevel;

SELECT mi.ItemName,i.ExpiryDate FROM MedicalItems mi JOIN Inventory i ON mi.ItemID = i.ItemID WHERE i.ExpiryDate BETWEEN CURDATE() AND DATE_ADD(CURDATE(), INTERVAL 6 MONTH);

SELECT po.OrderID,s.SupplierName,mi.ItemName,oi.Quantity,oi.UnitPrice FROM PurchaseOrders po JOIN OrderItems oi ON po.OrderID = oi.OrderID JOIN MedicalItems mi ON oi.ItemID = mi.ItemID JOIN Suppliers s ON po.SupplierID = s.SupplierID WHERE po.OrderID = 1;

SELECT s.SupplierName,SUM(oi.Quantity * oi.UnitPrice) AS TotalOrderValue FROM Suppliers s JOIN PurchaseOrders po ON s.SupplierID = po.SupplierID JOIN OrderItems oi ON po.OrderID = oi.OrderID GROUP BY s.SupplierName ORDER BY TotalOrderValue DESC;

SELECT po.OrderID,s.SupplierName,t.CarrierName,t.TrackingNumber,t.DepartureDate,t.ArrivalDate FROM PurchaseOrders po JOIN Transport t ON po.OrderID = t.OrderID JOIN Suppliers s ON po.SupplierID = s.SupplierID WHERE po.Status = 'Delivered';

SELECT mi.ItemName,ul.QuantityUsed,ul.Department,ul.UsedBy,ul.UsageDate FROM UsageLogs ul JOIN MedicalItems mi ON ul.ItemID = mi.ItemID WHERE ul.ItemID = 5; -- Example for ItemID 5 (Paracetamol 500mg)

SELECT p.FirstName,p.LastName,mi.ItemName,pr.Quantity,pr.PrescriptionDate,pr.DoctorName FROM Prescriptions pr JOIN Patients p ON pr.PatientID = p.PatientID JOIN MedicalItems mi ON pr.ItemID = mi.ItemID WHERE p.PatientID = 1; -- Example for PatientID 1 (John Doe)

SELECT  SUM(Quantity) AS TotalQuantityPrescribed FROM Prescriptions WHERE YEAR(PrescriptionDate) = 2025 AND MONTH(PrescriptionDate) = 9;

SELECT mi.ItemName,SUM(ul.QuantityUsed) AS TotalUsed FROM UsageLogs ul JOIN MedicalItems mi ON ul.ItemID = mi.ItemID GROUP BY mi.ItemName ORDER BY TotalUsed DESC LIMIT 5;

SELECT mi.ItemName,COUNT(DISTINCT pr.PatientID) AS UniquePatientCount FROM Prescriptions pr JOIN MedicalItems mi ON pr.ItemID = mi.ItemID GROUP BY mi.ItemName ORDER BY UniquePatientCount DESC;


SELECT s.SupplierName,AVG(DATEDIFF(t.ArrivalDate, po.OrderDate)) AS AverageDeliveryDays FROM PurchaseOrders po JOIN Transport t ON po.OrderID = t.OrderID JOIN Suppliers s ON po.SupplierID = s.SupplierID WHERE t.ArrivalDate IS NOT NULL GROUP BY s.SupplierName;


SELECT PatientID,FirstName,LastName,Phone,Email,Address FROM Patients WHERE Address LIKE '%San Diego%'; -- Example for patients in San Diego


SELECT
    mi.ItemName,
    mi.Category,
    i.QuantityOnHand,
    i.ExpiryDate,
    s.SupplierName,
    po.OrderDate,
    oi.Quantity AS QuantityOrdered,
    oi.UnitPrice
FROM
    MedicalItems mi
JOIN
    Inventory i ON mi.ItemID = i.ItemID
JOIN
    OrderItems oi ON mi.ItemID = oi.ItemID
JOIN
    PurchaseOrders po ON oi.OrderID = po.OrderID
JOIN
    Suppliers s ON po.SupplierID = s.SupplierID
WHERE
    mi.Category = 'PPE' OR mi.Category = 'Disposables' or mi.Category = 'Antibiotic';
