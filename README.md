# 🍕 FoodOrder: Online Food Ordering App

A full-stack web application where customers can browse a menu, add items to a cart, and place orders, while an admin manages foods, categories, and order status.

Built with **React** (frontend) and **Spring Boot** (backend REST API) with **MySQL** and **JWT authentication**.

##  Features

### Customer
- Register and login with JWT authentication
- Browse the menu with search and category filter
- Add to cart, change quantity, remove items (cart saved in the browser)
- Checkout with a delivery address
- View order history and live order status

### Admin
- Add, edit, and delete food items
- Mark foods as available or hidden
- Add and delete categories
- View all customers' orders
- Update order status: `Placed → Preparing → Out for delivery → Delivered`

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Vite, React Router, Axios, Context API, CSS |
| Backend | Java 21, Spring Boot, Spring Security, Spring Data JPA (Hibernate) |
| Database | MySQL |
| Auth | JWT, BCrypt password hashing |
| Tools | Maven, Git, Postman, IntelliJ IDEA |

##  Architecture

```
React  →  Controller  →  Service  →  Repository  →  MySQL
(UI)      (REST API)     (business     (JPA)
                          logic)
```

- **Layered architecture:** Controller, Service, Repository
- **DTOs** for all requests and responses (entities and password hashes are never exposed)
- **Bean Validation** and a global exception handler for clean error messages
- **Server-side pricing:** order totals are calculated from database prices, not from the client
- **Role-based access:** `/api/admin/**` is allowed only for `ADMIN`

## Database Design

| Table | Description |
|---|---|
| `users` | id, name, email, password (hashed), role |
| `categories` | id, name |
| `food_items` | id, name, price, image, description, available, category_id |
| `orders` | id, user_id, total, status, address, created_at |
| `order_items` | id, order_id, food_id, quantity, price |

Relationships: Category 1—* FoodItem, User 1—* Order, Order 1—* OrderItem, FoodItem 1—* OrderItem.

##  API Endpoints

### Auth (public)
| Method | URL | Description |
|---|---|---|
| POST | `/api/auth/register` | Create an account |
| POST | `/api/auth/login` | Login and receive a JWT |

### Foods (public)
| Method | URL | Description |
|---|---|---|
| GET | `/api/foods` | List foods (`?categoryId=` or `?search=`) |
| GET | `/api/foods/{id}` | Get one food |

### Orders (login required)
| Method | URL | Description |
|---|---|---|
| POST | `/api/orders` | Place an order |
| GET | `/api/orders/my` | My order history |
| GET | `/api/users/me` | Current user info |

### Admin (ADMIN role only)
| Method | URL | Description |
|---|---|---|
| GET / POST | `/api/admin/foods` | List or create foods |
| PUT / DELETE | `/api/admin/foods/{id}` | Update or delete a food |
| GET / POST | `/api/admin/categories` | List or create categories |
| DELETE | `/api/admin/categories/{id}` | Delete a category |
| GET | `/api/admin/orders` | List all orders |
| PUT | `/api/admin/orders/{id}/status` | Move an order to the next status |

Protected requests need the header: `Authorization: Bearer <token>`

##  Getting Started

### Prerequisites
- Java 21
- Node.js 18+
- MySQL 8+

### 1. Clone the repo
```bash
git clone https://github.com/YOUR_USERNAME/food-order-app.git
cd food-order-app
```

### 2. Create the database
```sql
CREATE DATABASE food_order_db;
```

### 3. Configure and run the backend
Set these environment variables (or edit `backend/src/main/resources/application.properties`):

```
DB_USERNAME=root
DB_PASSWORD=your_mysql_password
JWT_SECRET=a-long-random-secret-at-least-32-characters
```

Then run:
```bash
cd backend
./mvnw spring-boot:run
```
The API starts at `http://localhost:8080`. Tables and sample data are created automatically on first run.

### 4. Run the frontend
```bash
cd frontend
npm install
npm run dev
```
Open `http://localhost:5173`.

##  Demo Accounts

| Role | Email | Password |
|---|---|---|
| Admin | `admin@food.com` | `admin123` |
| Customer | Register a new account | |

>  Demo credentials are for local development only. Change them before any real deployment.

##  Screenshots

✏️ Add 3 to 5 screenshots here (menu page, cart, checkout, my orders, admin panel):

```
![Menu](screenshots/menu.png)
![Cart](screenshots/cart.png)
![Admin Panel](screenshots/admin.png)
```

##  Future Improvements

- Image upload for food items
- Email notifications on order updates
- Live order tracking
- Ratings and reviews
- Deployment (Render / Vercel)

## 👤 Author

** Neeraj Gupta **
GitHub: [neerajgupta6](https://github.com/neerajgupta6) 
