# Backend Feature Requirements

## 1. User Authentication

### API Endpoints
- `POST /api/register`
- `POST /api/login`
- `GET /api/profile`
- `PUT /api/profile`

### Input Specifications
- **Registration**:
  - `first_name` (string, required)
  - `last_name` (string, required)
  - `email` (string, required, unique, email format)
  - `password` (string, required, min 8 chars)
  - `role` (enum: guest | host)

- **Login**:
  - `email` (string, required)
  - `password` (string, required)

### Output Specifications
- JWT token on successful login or registration
- User profile JSON on `GET /api/profile`

### Validation Rules
- Email must be unique and valid
- Password must be at least 8 characters long
- Role must be one of the allowed enums

### Performance Criteria
- Authentication responses within 200ms
- Token expiration: 24 hours

---

## 2. Property Management

### API Endpoints
- `POST /api/properties`
- `GET /api/properties`
- `GET /api/properties/:id`
- `PUT /api/properties/:id`
- `DELETE /api/properties/:id`

### Input Specifications
- `host_id` (UUID, required)
- `title` (string, required)
- `description` (text, required)
- `location` (string, required)
- `price_per_night` (decimal, required)
- `amenities` (array of strings, optional)
- `availability` (array of date ranges, optional)

### Output Specifications
- Full property object in JSON
- Error messages if validation fails

### Validation Rules
- `price_per_night` must be a positive number
- `title` and `description` must not be empty

### Performance Criteria
- Property search should return results within 300ms
- Support pagination and filtering (e.g., price, location)

---

## 3. Booking System

### API Endpoints
- `POST /api/bookings`
- `GET /api/bookings`
- `GET /api/bookings/:id`
- `PUT /api/bookings/:id/cancel`

### Input Specifications
- `user_id` (UUID, required)
- `property_id` (UUID, required)
- `start_date` (ISO date, required)
- `end_date` (ISO date, required)

### Output Specifications
- Booking confirmation JSON
- Error if booking conflicts or validation fails

### Validation Rules
- `start_date` must be before `end_date`
- No overlapping bookings allowed for same property
- Max booking duration: 30 days

### Performance Criteria
- Booking creation within 250ms
- Conflict detection under 100ms

---
