## Creación de las tablas

### creación de la secuencia

En esta siguiente consulta vamos a crear una secuencia que nos va a servir para generar valores unicos, esta va de uno en uno

  *Si ya existe, va a ser eliminada*

```sql
USE TK463DW;
GO
IF OBJECT_ID('dbo.SeqCustomerDwKey','SO') IS NOT NULL
DROP SEQUENCE dbo.SeqCustomerDwKey;
GO
CREATE SEQUENCE dbo.SeqCustomerDwKey AS INT
START WITH 1
INCREMENT BY 1;
GO
```
***

### creación de la tabla customers

```sql
CREATE TABLE dbo.Customers
(
CustomerDwKey INT NOT NULL,
CustomerKey INT NOT NULL,
FullName NVARCHAR(150) NULL,
EmailAddress NVARCHAR(50) NULL,
BirthDate DATE NULL,
MaritalStatus NCHAR(1) NULL,
Gender NCHAR(1) NULL,
Education NVARCHAR(40) NULL,
Occupation NVARCHAR(100) NULL,
City NVARCHAR(30) NULL,
StateProvince NVARCHAR(50) NULL,
CountryRegion NVARCHAR(50) NULL,
Age AS
CASE
WHEN DATEDIFF(yy, BirthDate, CURRENT_TIMESTAMP) <= 40
THEN 'Younger'
WHEN DATEDIFF(yy, BirthDate, CURRENT_TIMESTAMP) > 50
THEN 'Older'
ELSE 'Middle Age'
END,

CurrentFlag BIT NOT NULL DEFAULT 1,
CONSTRAINT PK_Customers PRIMARY KEY (CustomerDwKey)
);
GO

```
***

### creación de la tabla products

```sql
CREATE TABLE dbo.Products(
ProductKey INT NOT NULL,
ProductName NVARCHAR (50) NULL,
Color NVARCHAR (15) NULL,
Size NVARCHAR (50) NULL,
SubcategoryName NVARCHAR(50) NULL,
CategoryName NVARCHAR (50) NULL,
CONSTRAINT PK_Products PRIMARY KEY (ProductKey));
GO

```
***

### creación de la tabla dates

```sql
CREATE TABLE dbo.Dates
(
DateKey INT NOT NULL,
FullDate DATE NOT NULL,
MonthNumberName NVARCHAR(15) NULL,
CalendarQuarter TINYINT NULL,
CalendarYear SMALLINT NULL,
CONSTRAINT PK_Dates PRIMARY KEY (DateKey)
);

GO

```
***


### creación de la tabla InternetSales

```sql
CREATE TABLE dbo.InternetSales
(
InternetSalesKey INT NOT NULL IDENTITY(1,1),
CustomerDwKey INT NOT NULL,
ProductKey INT NOT NULL,
DateKey INT NOT NULL,
OrderQuantity SMALLINT NOT NULL DEFAULT 0,
SalesAmount MONEY NOT NULL DEFAULT 0,
UnitPrice MONEY NOT NULL DEFAULT 0,
DiscountAmount FLOAT NOT NULL DEFAULT 0,

CONSTRAINT PK_InternetSales
PRIMARY KEY (InternetSalesKey)
);
GO

```
***

### modificar la tabla InternetSales

```sql
ALTER TABLE dbo.InternetSales ADD CONSTRAINT
FK_InternetSales_Customers FOREIGN KEY(CustomerDwKey)
REFERENCES dbo.Customers (CustomerDwKey);
ALTER TABLE dbo.InternetSales ADD CONSTRAINT
FK_InternetSales_Products FOREIGN KEY(ProductKey)
REFERENCES dbo.Products (ProductKey);
ALTER TABLE dbo.InternetSales ADD CONSTRAINT
FK_InternetSales_Dates FOREIGN KEY(DateKey)
REFERENCES dbo.Dates (DateKey);
GO

```
***


