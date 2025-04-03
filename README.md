# Data definition language (DDL) Checkpoint

## Checkpoint

In this checkpoint, we have the following relational model: `https://i.imgur.com/aZeHhHe.png`

and their corresponding data types tables: `https://i.imgur.com/vx1xFvS.png`

### Instructions

* You are asked to create the above given relational model using SQL language and based on the different mentioned constraints.
* After creating tables, write SQL commands to:
  * Add a column Category (VARCHAR2(20)) to the PRODUCT table
  * Add a column OrderDate (DATE)  to the ORDERS table which have SYSDATE as a default value

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

#### Explanation

**Table Creation:**

* All tables are created with primary keys (`PRIMARY KEY`) and foreign keys (`FOREIGN KEY`) as shown in the relational model.
* Data types match the specifications (e.g., `VARCHAR2` for strings, `NUMBER` for numeric values, `DATE` for dates).

**Column Additions:**

* `Category` is added to the `PRODUCT` table as a `VARCHAR2(20)`.
* `OrderDate` is added to the `ORDERS` table with a default value of the current date (`SYSDATE`).
