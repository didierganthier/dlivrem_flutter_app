# Database Structure for a D-Livrem App

This is a conceptual database structure, focusing on key entities and relationships.

## Tables:

### 1. Users

| Column Name   | Data Type      | Constraints            | Description                        |
|---------------|----------------|------------------------|------------------------------------|
| user_id       | INTEGER        | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for the user     |
| full_name     | VARCHAR(255)   | NOT NULL              | User's full name                  |
| email         | VARCHAR(255)   | UNIQUE, NOT NULL      | User's email address             |
| phone_number  | VARCHAR(20)    |                       | User's phone number                |
| password_hash | VARCHAR(255)   | NOT NULL              | Hashed user password             |
| created_at    | TIMESTAMP      | DEFAULT CURRENT_TIMESTAMP  | User account creation time        |
| last_login    | TIMESTAMP      |                       | Last time user logged in           |

### 2. Restaurants

| Column Name          | Data Type      | Constraints            | Description                               |
|----------------------|----------------|------------------------|-------------------------------------------|
| restaurant_id        | INTEGER        | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for the restaurant    |
| name                 | VARCHAR(255)   | NOT NULL              | Restaurant's name                         |
| address              | VARCHAR(255)   | NOT NULL              | Restaurant's physical address             |
| latitude             | DECIMAL(10, 8) |                       | Restaurant's latitude coordinate          |
| longitude            | DECIMAL(11, 8) |                       | Restaurant's longitude coordinate          |
| phone_number         | VARCHAR(20)    |                       | Restaurant's phone number                 |
| cuisine_type         | VARCHAR(100)   |                       | Type of cuisine served                    |
| opening_time        | TIME           |                       | Restaurant opening time             |
| closing_time        | TIME           |                       | Restaurant closing time             |
| average_rating       | DECIMAL(3, 2)  |                       | Average rating of the restaurant          |
| delivery_fee         | DECIMAL(5, 2)  |                       | Delivery fee charged                      |

### 3. Menu Items

| Column Name    | Data Type      | Constraints            | Description                         |
|----------------|----------------|------------------------|-------------------------------------|
| item_id        | INTEGER        | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for the menu item     |
| restaurant_id  | INTEGER        | FOREIGN KEY REFERENCES Restaurants(restaurant_id) | Restaurant to which the item belongs |
| name           | VARCHAR(255)   | NOT NULL              | Item's name                         |
| description    | TEXT           |                       | Item's description                   |
| price          | DECIMAL(6, 2)  | NOT NULL              | Item's price                        |
| image_url     | VARCHAR(255)   |                       | URL for the item's image           |
| category       | VARCHAR(100)   |                       | Category of the menu item      |

### 4. Orders

| Column Name   | Data Type      | Constraints                    | Description                                    |
|---------------|----------------|--------------------------------|------------------------------------------------|
| order_id      | INTEGER        | PRIMARY KEY, AUTO_INCREMENT       | Unique identifier for the order                  |
| user_id       | INTEGER        | FOREIGN KEY REFERENCES Users(user_id)   | User who placed the order                        |
| restaurant_id | INTEGER        | FOREIGN KEY REFERENCES Restaurants(restaurant_id)   | Restaurant from which the order was placed      |
| order_time    | TIMESTAMP      | DEFAULT CURRENT_TIMESTAMP       | Time when the order was placed                  |
| delivery_address | VARCHAR(255)  | NOT NULL                       | Delivery address for the order                  |
| order_status  | ENUM ('Pending','Preparing', 'Out for delivery','Delivered', 'Cancelled')| DEFAULT 'Pending'| Status of the order                              |
| total_amount | DECIMAL(8, 2)   | NOT NULL                       | Total amount of the order                        |
| delivery_fee   | DECIMAL(5, 2)   | NOT NULL                       | Delivery fee charged for the order        |

### 5. Order Items (Junction Table)

| Column Name    | Data Type      | Constraints                       | Description                               |
|----------------|----------------|-----------------------------------|-------------------------------------------|
| order_item_id  | INTEGER        | PRIMARY KEY, AUTO_INCREMENT        | Unique identifier for the order item       |
| order_id       | INTEGER        | FOREIGN KEY REFERENCES Orders(order_id)    | Order to which the item belongs             |
| item_id        | INTEGER        | FOREIGN KEY REFERENCES Menu Items(item_id) | Menu item included in the order           |
| quantity       | INTEGER        | NOT NULL                           | Quantity of the item ordered              |
| item_price     | DECIMAL(6, 2)  | NOT NULL                           | Item price at the time of order         |


### 6. Payment Methods

| Column Name         | Data Type      | Constraints                       | Description                                  |
|---------------------|----------------|-----------------------------------|----------------------------------------------|
| payment_method_id | INTEGER        | PRIMARY KEY, AUTO_INCREMENT        | Unique identifier for payment method          |
| user_id             | INTEGER        | FOREIGN KEY REFERENCES Users(user_id)   | User who owns the payment method              |
| card_type           | VARCHAR(50)    |                                   | Type of card (e.g., Visa, MasterCard)       |
| card_number         | VARCHAR(20)    | NOT NULL                       | Card number                                 |
| expiry_date         | DATE           | NOT NULL                       | Expiry date of the card                     |
| cvv                 | VARCHAR(5)    | NOT NULL                       | CVV of the card                             |
| is_default        | BOOLEAN        | DEFAULT FALSE                    | Indicates if this is the default payment method |

### 7. Reviews

| Column Name | Data Type      | Constraints            | Description                               |
|-------------|----------------|------------------------|-------------------------------------------|
| review_id   | INTEGER        | PRIMARY KEY, AUTO_INCREMENT   | Unique identifier for the review          |
| user_id     | INTEGER        | FOREIGN KEY REFERENCES Users(user_id)  | User who wrote the review                   |
| order_id    | INTEGER        | FOREIGN KEY REFERENCES Orders(order_id) | Order the review is related to             |
| rating      | INTEGER        | NOT NULL              | Rating given by the user (e.g., 1 to 5) |
| comment     | TEXT           |                       | Text of the review                        |
| review_time | TIMESTAMP      | DEFAULT CURRENT_TIMESTAMP | Time when the review was submitted           |

## Relationships:

*   A **User** can place multiple **Orders**.
*   A **Restaurant** can have multiple **Menu Items**.
*   A **Restaurant** can have multiple **Orders** from different users.
*   An **Order** can have multiple **Order Items**.
*   A **User** can have multiple **Payment Methods**.
*  A **User** can write multiple **Reviews**.
* An **Order** can have one **Review**
*  A **Restaurant** can receive multiple **Reviews** through their Orders.

## Notes:
*   This structure assumes a relational database (like MySQL, PostgreSQL, etc.).
*   Data types might vary depending on the database system being used.
*   You might need additional tables for things like delivery driver information, promotions, etc, depending on the features you need.
*   Indexes are important but not included in this outline for simplicity.

This markdown should give you a good overview of a possible database structure for a food delivery app like the one you described. Let me know if you would like a more detailed model!
samples, guidance on mobile development, and a full API reference.
