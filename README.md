Medical Inventory and Patient Management System

This project provides a comprehensive SQL database schema for managing a medical facility's complete lifecycle of inventory, from procurement and supply chain to patient prescription and usage. It's designed to offer robust data tracking and powerful analytics to improve operational efficiency, reduce waste, and ensure continuity of patient care.



Database Schema

The database is designed with a normalized structure to ensure data integrity and reduce redundancy. The schema interconnects ten tables that manage suppliers, inventory, procurement, patients, and prescriptions.





Features & Analytical Capabilities



This database schema is a powerful analytical tool. The included queries enable a wide range of insights:

Inventory Control: Track current stock levels and instantly identify items that have fallen below their reorder thresholds.

Expiry Management: Proactively generate reports on all medical items nearing their expiry date to minimize waste.

Supply Chain Analysis: Analyze supplier performance by calculating total order values and average delivery times.

Procurement & Order Tracking: View detailed contents of any purchase order and track the transport status of delivered goods.

Patient & Prescription Management: Maintain detailed patient records and view complete prescription histories for any individual.

Usage Analytics: Monitor the top 5 most frequently used items and track consumption by department.

Intelligent Data Inference: The schema includes a query to automatically populate the PatientConditions table by mapping prescribed medications to likely health conditions, creating a valuable dataset for patient analysis.


Technologies Used


Database: SQL

Dialect: The schema and queries are written in a standard dialect compatible with MySQL, but are easily adaptable to other relational databases like PostgreSQL, SQL Server.

