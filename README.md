# Vehicle Rental System — SQL Queries 📋

**Project purpose:** This repository contains a set of sample SQL queries (in `queries.sql`) that demonstrate common relational operations: JOIN, EXISTS, WHERE, GROUP BY and HAVING. This README explains each query, shows the SQL, and describes the expected result.

---

## Files

- `queries.sql` — contains the SQL solutions for the four tasks.

---

## Schema assumptions

For clarity the queries assume the following table structures (names and column examples):

- `booking` (booking_id, user_id, vehicle_id, start_date, end_date, status)
- `users` (user_id, name, ...)
- `vehicles` (vehicle_id, name, type, status, ...)

> Note: Adjust column/table names to match your actual schema if they differ.

---

## Query 1 — JOIN: Retrieve booking information with customer and vehicle names ✅

**Requirement:** Retrieve booking information along with customer name and vehicle name.

```sql
SELECT b.booking_id,
       u.name AS customer_name,
       v.name AS vehicle_name,
       b.start_date,
       b.end_date,
       b.status
FROM booking b
JOIN users u ON b.user_id = u.user_id
JOIN vehicles v ON b.vehicle_id = v.vehicle_id;
```

**Explanation:** This query uses inner JOINs to combine `booking` rows with matching `users` and `vehicles` records. The result lists bookings that have valid user and vehicle references and includes human-readable names.

**Notes:** If you want bookings even when the user or vehicle record is missing, use `LEFT JOIN` instead.

---

## Query 2 — EXISTS: Find vehicles that have never been booked ❗

**Requirement:** Find all vehicles that have never been booked.

```sql
SELECT *
FROM vehicles v
WHERE NOT EXISTS (
  SELECT 1
  FROM booking b
  WHERE b.vehicle_id = v.vehicle_id
);
```

**Explanation:** The `NOT EXISTS` subquery checks for the absence of any `booking` row with the same `vehicle_id`. This returns vehicles with zero bookings. Using `NOT EXISTS` is a safe, ANSI-compatible way to express this intent.

**Alternative:** You can also use a `LEFT JOIN` with `WHERE b.vehicle_id IS NULL` to achieve the same result.

---

## Query 3 — WHERE: Retrieve available vehicles of a specific type (e.g., cars) 🚗

**Requirement:** Retrieve all available vehicles of a specific type (e.g., cars).

```sql
SELECT *
FROM vehicles v
WHERE v.status = 'available'
  AND v.type = 'car';
```

**Explanation:** This simple filter query uses `WHERE` to select vehicles whose `status` is `'available'` and whose `type` matches `'car'`. Change the literal `'car'` to any other type as needed.

---

## Query 4 — GROUP BY and HAVING: Vehicles with more than 2 bookings 📊

**Requirement:** Find the total number of bookings for each vehicle and display only those vehicles that have more than 2 bookings.

```sql
SELECT v.vehicle_id,
       v.name,
       COUNT(b.booking_id) AS total_bookings
FROM vehicles v
JOIN booking b ON v.vehicle_id = b.vehicle_id
GROUP BY v.vehicle_id, v.name
HAVING COUNT(b.booking_id) > 2;
```

**Explanation:** This query joins `vehicles` with `booking`, groups results by vehicle, counts bookings per vehicle, and filters groups with `HAVING` to return only vehicles with more than 2 bookings.




