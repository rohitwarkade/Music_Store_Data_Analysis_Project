
## Music_Store_Data_Analysis_Project

---

## **Step 1: Define Project Objectives**
- Analyze sales trends (e.g., popular genres, artists, or albums).
- Understand customer demographics and purchase behavior.
- Evaluate store performance over time (e.g., revenue by month or year).
- Identify opportunities for targeted marketing (e.g., popular genres by location).

---

## **Step 2: Database Design**
### **Entities**
1. **Customers**: Information about the buyers.
2. **Albums**: Metadata about the music albums.
3. **Artists**: Information about artists.
4. **Genres**: Musical genres associated with tracks.
5. **Tracks**: Information about individual tracks.
6. **Invoices**: Sales details for purchases.
7. **Invoice_Items**: Specific tracks purchased on an invoice.

### **Schema Design**
```sql
-- Customers Table
CREATE TABLE Customers (
    CustomerID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Email VARCHAR(100),
    Country VARCHAR(50),
    City VARCHAR(50)
);

-- Artists Table
CREATE TABLE Artists (
    ArtistID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100)
);

-- Albums Table
CREATE TABLE Albums (
    AlbumID INT AUTO_INCREMENT PRIMARY KEY,
    Title VARCHAR(100),
    ArtistID INT,
    FOREIGN KEY (ArtistID) REFERENCES Artists(ArtistID)
);

-- Genres Table
CREATE TABLE Genres (
    GenreID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(50)
);

-- Tracks Table
CREATE TABLE Tracks (
    TrackID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100),
    AlbumID INT,
    GenreID INT,
    UnitPrice DECIMAL(5, 2),
    FOREIGN KEY (AlbumID) REFERENCES Albums(AlbumID),
    FOREIGN KEY (GenreID) REFERENCES Genres(GenreID)
);

-- Invoices Table
CREATE TABLE Invoices (
    InvoiceID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerID INT,
    InvoiceDate DATE,
    Total DECIMAL(10, 2),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- Invoice_Items Table
CREATE TABLE Invoice_Items (
    InvoiceItemID INT AUTO_INCREMENT PRIMARY KEY,
    InvoiceID INT,
    TrackID INT,
    UnitPrice DECIMAL(5, 2),
    Quantity INT,
    FOREIGN KEY (InvoiceID) REFERENCES Invoices(InvoiceID),
    FOREIGN KEY (TrackID) REFERENCES Tracks(TrackID)
);
```

---

## **Step 3: Import Data**
- Use CSV files or existing datasets to populate tables.
- MySQL commands for importing data:
  ```sql
  LOAD DATA INFILE 'path/to/customers.csv'
  INTO TABLE Customers
  FIELDS TERMINATED BY ',' 
  LINES TERMINATED BY '\n'
  IGNORE 1 ROWS;
  ```

---

## **Step 4: Perform Analysis**
### **Sample Queries**
1. **Top Selling Genres**
   ```sql
   SELECT g.Name AS Genre, SUM(ii.Quantity) AS Total_Sales
   FROM Invoice_Items ii
   JOIN Tracks t ON ii.TrackID = t.TrackID
   JOIN Genres g ON t.GenreID = g.GenreID
   GROUP BY g.Name
   ORDER BY Total_Sales DESC;
   ```

2. **Top Artists by Revenue**
   ```sql
   SELECT ar.Name AS Artist, SUM(ii.UnitPrice * ii.Quantity) AS Revenue
   FROM Invoice_Items ii
   JOIN Tracks t ON ii.TrackID = t.TrackID
   JOIN Albums al ON t.AlbumID = al.AlbumID
   JOIN Artists ar ON al.ArtistID = ar.ArtistID
   GROUP BY ar.Name
   ORDER BY Revenue DESC;
   ```

3. **Monthly Revenue Trend**
   ```sql
   SELECT DATE_FORMAT(InvoiceDate, '%Y-%m') AS Month, SUM(Total) AS Monthly_Revenue
   FROM Invoices
   GROUP BY Month
   ORDER BY Month;
   ```

4. **Customer Purchase Frequency**
   ```sql
   SELECT c.FirstName, c.LastName, COUNT(i.InvoiceID) AS Purchase_Count
   FROM Customers c
   JOIN Invoices i ON c.CustomerID = i.CustomerID
   GROUP BY c.CustomerID
   ORDER BY Purchase_Count DESC;
   ```

5. **Revenue by Country**
   ```sql
   SELECT c.Country, SUM(i.Total) AS Revenue
   FROM Invoices i
   JOIN Customers c ON i.CustomerID = c.CustomerID
   GROUP BY c.Country
   ORDER BY Revenue DESC;
   ```

---

## **Step 5: Insights and Visualizations**
- Use tools like Tableau, Power BI, or Python libraries (e.g., Matplotlib, Seaborn) to visualize data.
- Example visualizations:
  - Bar charts for top genres or artists.
  - Line charts for monthly revenue trends.
  - Heatmaps for revenue by country.

---

## **Step 6: Documentation**
- Write a report detailing the objectives, methods, results, and recommendations based on analysis.
- Include SQL queries and visualizations.

Would you like help setting up specific queries, further steps, or generating a detailed report for this analysis?
