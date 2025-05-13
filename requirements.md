# Backend Requirement Specifications

## 1. User Authentication

### Functional Requirements:
- Allow users to register as guest or host


### Objective
Allow users to register, login, and maintain secure sessions using JWT.

### Endpoints
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/profile` (Authenticated)
- `PUT /api/auth/profile` (Authenticated)

### Input/Output Specifications
#### Registration (`/register`)
**Input:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "SecurePass123",
  "role": "guest" | "host"
}
```
**Output:**
```json
{
  "message": "User registered successfully",
  "token": "jwt-token-here"
}
```
### Validation Rules
- Email must be unique and properly formatted.

- Password must be at least 8 characters.

- Role must be either "guest" or "host".

### Performance
- Average response time < 300ms.
- Rate limit: 5 registration/login attempts per minute.

## 2. Property Management
### Objective
Allow hosts to create, edit, delete, and view their property listings.

### Endpoints

- ```POST /api/properties (Authenticated Host)```
- ```GET /api/properties```
- ```GET /api/properties/:id```
- ```PUT /api/properties/:id (Authenticated Host) ```
- ```DELETE /api/properties/:id (Authenticated Host) ```

### Input/Output Specifications
Create Property (/properties)

**Input:**

```json
{
  "title": "Modern Apartment",
  "description": "Great view and close to downtown.",
  "price": 100,
  "location": "Addis Ababa",
  "amenities": ["WiFi", "Kitchen"],
  "availability": ["2024-06-01", "2024-06-15"]
} 
```

**Output:**

```json
{
  "message": "Property created successfully",
  "propertyId": "abc123"
}
```
### Validation Rules
- Title must not exceed 100 characters.
- Price must be positive.
- vailability must contain valid date strings.

### Performance
- Use indexing on location and price for search optimization.

## 3. Booking System
### Objective
Enable guests to book available listings for specified dates.

### Endpoints
- ```POST /api/bookings (Authenticated Guest)```
- ```GET /api/bookings (Authenticated User)```
- ```GET /api/bookings/:id (Authenticated User)```
- ```PUT /api/bookings/:id/cancel (Authenticated User)```

### Input/Output Specifications
Make Booking (/bookings)
**Input:**

```json
{
  "propertyId": "abc123",
  "checkIn": "2024-06-05",
  "checkOut": "2024-06-10"
}
```
**Output:**

```json
{
  "message": "Booking created",
  "bookingId": "bkg789",
  "status": "pending"
}
```
### Validation Rules
- Booking must not overlap with existing bookings.
- checkIn must be before checkOut.
- Dates must be in the future.

### Performance
Optimize availability checks with caching.

