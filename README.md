# Bookify: Scheduling SaaS App

## Features

- **Calendar View & Booking Slots:** Interactive calendar for users to view and book available slots.
- **User Login & Business Admin Panel:** Secure authentication, user roles, and business management dashboard.
- **Stripe Recurring Billing:** Subscription management and payments via Stripe.
- **Email & SMS Reminders:** Automated notifications using Twilio.
- **Custom Branding (White-label):** Each client can set their own branding.

---

## Tech Stack

- **Backend:** Laravel (PHP)
- **Frontend:** Vue.js
- **Database:** MySQL
- **Payments:** Stripe
- **Notifications:** Twilio (SMS), Email
- **Containerization:** Docker

---

## Folder Structure

```
/backend (Laravel)
/frontend (Vue.js)
/docker
```

---

## API Documentation

### Authentication

#### `POST /api/login`
- **Body:** `{ email, password }`
- **Response:** `{ token }`

#### `POST /api/register`
- **Body:** `{ name, email, password }`
- **Response:** `{ user, token }`

### Bookings

#### `GET /api/bookings`
- **Headers:** `Authorization: Bearer {token}`
- **Response:** `[{ id, slot, user, status }]`

#### `POST /api/bookings`
- **Body:** `{ slot_id }`
- **Headers:** `Authorization: Bearer {token}`
- **Response:** `{ booking }`

### Calendar

#### `GET /api/calendar`
- **Headers:** `Authorization: Bearer {token}`
- **Response:** `[{ date, slots: [{ id, time, available }] }]`

### Billing

#### `POST /api/subscribe`
- **Body:** `{ plan_id, payment_method }`
- **Headers:** `Authorization: Bearer {token}`
- **Response:** `{ subscription }`

---

## Unit Test Example (Laravel)

```php
public function test_user_can_book_slot()
{
    $user = User::factory()->create();
    $slot = Slot::factory()->create();

    $response = $this->actingAs($user)->postJson('/api/bookings', [
        'slot_id' => $slot->id,
    ]);

    $response->assertStatus(201)
             ->assertJson(['booking' => true]);
}
```

---

## Docker Setup

**docker-compose.yml**
```yaml
version: '3.8'
services:
  app:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - DB_HOST=db
      - DB_DATABASE=bookify
      - DB_USERNAME=root
      - DB_PASSWORD=secret
    depends_on:
      - db
  frontend:
    build: ./frontend
    ports:
      - "8080:8080"
  db:
    image: mysql:8
    environment:
      MYSQL_DATABASE: bookify
      MYSQL_ROOT_PASSWORD: secret
    ports:
      - "3306:3306"
```

---

## Custom Branding

- Each business can upload logo, set colors, and customize email/SMS templates via the admin panel.

---

## Reminders

- Scheduled jobs send reminders via email and SMS before bookings using Twilio and Laravel queues.

---

## Next Steps

- Scaffold Laravel backend and Vue.js frontend.
- Integrate Stripe and Twilio APIs.
- Implement Docker containers.
- Write unit and integration tests.
