# Data definition language (DDL) Checkpoint

## Checkpoint

In this checkpoint, we have the following relational model: `https://i.imgur.com/aZeHhHe.png`

and their corresponding data types tables: `https://i.imgur.com/vx1xFvS.png`

### Instructions

- You are asked to create the above given relational model using SQL language and based on the different mentioned constraints.
- After creating tables, write SQL commands to:
  - Add a column Category (VARCHAR2(20)) to the PRODUCT table
  - Add a column OrderDate (DATE) to the ORDERS table which have SYSDATE as a default value

## Solution

```sql
CREATE TABLE CUSTOMER (
    CustomerID NUMBER PRIMARY KEY,
    Name VARCHAR2(50) NOT NULL,
    ContactInfo VARCHAR2(100),
    Address VARCHAR2(200)
);

CREATE TABLE PRODUCT (
    ProductID NUMBER PRIMARY KEY,
    Name VARCHAR2(50) NOT NULL,
    Price NUMBER(10,2)
);

CREATE TABLE ORDERS (
    OrderID NUMBER PRIMARY KEY,
    CustomerID NUMBER NOT NULL,
    CONSTRAINT fk_customer
        FOREIGN KEY (CustomerID) REFERENCES CUSTOMER(CustomerID)
);

CREATE TABLE ORDER_DETAIL (
    OrderID NUMBER,
    ProductID NUMBER,
    Quantity NUMBER NOT NULL,
    UnitPrice NUMBER(10,2),
    PRIMARY KEY (OrderID, ProductID),
    CONSTRAINT fk_order
        FOREIGN KEY (OrderID) REFERENCES ORDERS(OrderID),
    CONSTRAINT fk_product
        FOREIGN KEY (ProductID) REFERENCES PRODUCT(ProductID)
);

CREATE TABLE PAYMENT (
    PaymentID NUMBER PRIMARY KEY,
    OrderID NUMBER NOT NULL,
    Amount NUMBER(10,2),
    PaymentDate DATE,
    CONSTRAINT fk_order_payment
        FOREIGN KEY (OrderID) REFERENCES ORDERS(OrderID)
);
```

### Adding New Columns

```sql
-- Add New Columns
ALTER TABLE PRODUCT
ADD Category VARCHAR2(20);

ALTER TABLE ORDERS
ADD OrderDate DATE DEFAULT SYSDATE;
```

### Inserting Data

```sql
-- Insert into CUSTOMER
INSERT INTO CUSTOMER (CustomerID, Name, ContactInfo, Address)
VALUES
(1, 'John Doe', 'john@example.com', '123 Street'),
(2, 'Jane Smith', 'jane@example.com', '456 Avenue'),
(3, 'Bob Johnson', 'bob@example.com', '789 Road');

-- Insert into PRODUCT
INSERT INTO PRODUCT (ProductID, Name, Price, Category)
VALUES
(1, 'Laptop', 999.99, 'Electronics'),
(2, 'Phone', 699.99, 'Electronics'),
(3, 'Headphones', 149.99, 'Accessories');

-- Insert into ORDERS
INSERT INTO ORDERS (OrderID, CustomerID, OrderDate)
VALUES
(101, 1, TO_DATE('2023-10-01', 'YYYY-MM-DD')),
(102, 2, TO_DATE('2023-10-02', 'YYYY-MM-DD'));

-- Insert into ORDER_DETAIL
INSERT INTO ORDER_DETAIL (OrderID, ProductID, Quantity, UnitPrice)
VALUES
(101, 1, 1, 999.99),
(101, 2, 2, 699.99),
(102, 3, 1, 699.99);  -- Note: UnitPrice differs from PRODUCT table (historical pricing)

-- Insert into PAYMENT
INSERT INTO PAYMENT (PaymentID, OrderID, Amount, PaymentDate)
VALUES
(1001, 101, 2399.97, TO_DATE('2023-10-01', 'YYYY-MM-DD')),
(1002, 102, 699.99, TO_DATE('2023-10-02', 'YYYY-MM-DD'));
```

#### Explanation

**Table Creation:**

- All tables are created with primary keys (`PRIMARY KEY`) and foreign keys (`FOREIGN KEY`) as shown in the relational model.
- Data types match the specifications (e.g., `VARCHAR2` for strings, `NUMBER` for numeric values, `DATE` for dates).

**Column Additions:**

- `Category` is added to the `PRODUCT` table as a `VARCHAR2(20)`.
- `OrderDate` is added to the `ORDERS` table with a default value of the current date (`SYSDATE`).

#### Key Notes

**Date Format:** Uses TO_DATE to explicitly convert strings to DATE type.
**Order Detail Pricing:**

- The UnitPrice in ORDER_DETAIL for ProductID 3 (Headphones) is 699.99 instead of the current 149.99 in PRODUCT. This reflects historical pricing (price at the time of purchase).

**Payment Totals:**

- Order 101: `(1 * 999.99) + (2 * 699.99) = 2399.97`
- Order 102: `1 * 699.99 = 699.99`

**Foreign Key Constraints:**

- All `CustomerID` values in `ORDERS` exist in the `CUSTOMER` table.
- All `ProductID` and `OrderID` values in `ORDER_DETAIL` exist in their respective tables.
- All `OrderID` values in `PAYMENT` exist in `ORDERS`.
