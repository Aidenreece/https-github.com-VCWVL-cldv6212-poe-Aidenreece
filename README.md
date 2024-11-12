-- Create the 'Customers' table to store customer information
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY IDENTITY(1,1),
    FirstName NVARCHAR(100) NOT NULL,
    LastName NVARCHAR(100) NOT NULL,
    Email NVARCHAR(255) UNIQUE NOT NULL,
    Phone NVARCHAR(50),
    Address NVARCHAR(255),
    City NVARCHAR(100),
    PostalCode NVARCHAR(20),
    Country NVARCHAR(100),
    CreatedAt DATETIME DEFAULT GETDATE()
); 
-- Insert into 'Customers' table
INSERT INTO Customers (FirstName, LastName, Email, Phone, Address, City, PostalCode, Country)
VALUES
('John', 'Doe', 'john.doe@example.com', '123-456-7890', '123 Main St', 'New York', '10001', 'USA'),
('Jane', 'Smith', 'jane.smith@example.com', '098-765-4321', '456 Elm St', 'Los Angeles', '90001', 'USA');

-- Create the 'Products' table to store product information
CREATE TABLE Products (
    ProductID INT PRIMARY KEY IDENTITY(1,1),
    ProductName NVARCHAR(200) NOT NULL,
    Description NVARCHAR(1000),
    Price DECIMAL(18, 2) NOT NULL,
    StockQuantity INT NOT NULL,
    CreatedAt DATETIME DEFAULT GETDATE()
);
-- Insert into 'Products' table
INSERT INTO Products (ProductName, Description, Price, StockQuantity)
VALUES
('Laptop', '15-inch laptop with 8GB RAM', 999.99, 50),
('Smartphone', 'Latest model with 128GB storage', 699.99, 100);
-- Create the 'Orders' table to store order information
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY IDENTITY(1,1),
    CustomerID INT FOREIGN KEY REFERENCES Customers(CustomerID),
    OrderDate DATETIME DEFAULT GETDATE(),
    TotalAmount DECIMAL(18, 2) NOT NULL,
    Status NVARCHAR(50) DEFAULT 'Pending',
    ShippingAddress NVARCHAR(255),
    ShippingCity NVARCHAR(100),
    ShippingPostalCode NVARCHAR(20),
    ShippingCountry NVARCHAR(100)
);
-- Insert into 'Orders' table
INSERT INTO Orders (CustomerID, TotalAmount, Status, ShippingAddress, ShippingCity, ShippingPostalCode, ShippingCountry)
VALUES
(1, 999.99, 'Pending', '123 Main St', 'New York', '10001', 'USA'),
(2, 699.99, 'Shipped', '456 Elm St', 'Los Angeles', '90001', 'USA');
-- Create the 'OrderItems' table to store items in each order
CREATE TABLE OrderItems (
    OrderItemID INT PRIMARY KEY IDENTITY(1,1),
    OrderID INT FOREIGN KEY REFERENCES Orders(OrderID),
    ProductID INT FOREIGN KEY REFERENCES Products(ProductID),
    Quantity INT NOT NULL,
    Price DECIMAL(18, 2) NOT NULL,
    Total AS (Quantity * Price) PERSISTED
);
-- Insert into 'OrderItems' table
INSERT INTO OrderItems (OrderID, ProductID, Quantity, Price)
VALUES
(1, 1, 1, 999.99),
(2, 2, 1, 699.99);
-- Create the 'Payments' table to store payment information for orders
CREATE TABLE Payments (
    PaymentID INT PRIMARY KEY IDENTITY(1,1),
    OrderID INT FOREIGN KEY REFERENCES Orders(OrderID),
    PaymentDate DATETIME DEFAULT GETDATE(),
    PaymentMethod NVARCHAR(100),
    PaymentAmount DECIMAL(18, 2) NOT NULL,
    PaymentStatus NVARCHAR(50) DEFAULT 'Pending'
);
-- Insert into 'Payments' table
INSERT INTO Payments (OrderID, PaymentMethod, PaymentAmount, PaymentStatus)
VALUES
(1, 'Credit Card', 999.99, 'Completed'),
(2, 'PayPal', 699.99, 'Pending');
