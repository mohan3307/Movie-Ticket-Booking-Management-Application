# Movie Ticket Booking Management Application

## Project Overview
The **Movie Ticket Booking Management Application** is an end-to-end enterprise solution built on the **Pega Platform** (App Studio & Dev Studio) following Business Architect (BA) best practices. The application streamlines movie selection, seat availability validation, pricing computation, customer approvals, seat allocation, and automated correspondence.

---

## Project Links & Deliverables

| Deliverable | Link / Resource |
| :--- | :--- |
| **Demo Viwe / Walkthrough** | [Click Here to Watch Demo](
https://dz4qqcya.pegacea.net/prweb/app/movie-ticket-request/) |
| **Pega Blueprint File** | [View Blueprint JSON](./blueprint/) |

---

## Business Requirements & Features

### 1. Case Lifecycle Architecture
The core case type **`Movie Ticket Request`** is structured into four primary stages:
* **Request Intake:** Captures user details including `Movie Name`, `Show Date`, `Show Time`, and `Number of Tickets`.
* **Availability:** Validates show seat limits (`Available Seats Count`), triggers calculation logic (`Ticket Price * Number of Tickets = Total Cost`), and enforces seat threshold validations.
* **Booking Confirmation:** Displays an itemized booking summary and captures approval/rejection decisions from the `Customer` persona.
* **Booking Execution:** Handles system-level seat assignment (`Seat Numbers`, `Ticket ID`), work routing, and status updates.

### 2. Reusable Data Models
* **Movie:** Independent data object storing movie-specific metadata (`Movie Name`, `Genre`, etc.).
* **Show:** Data object managing schedule records (`Show Date`, `Show Time`, `Seat Capacity`).

### 3. Business Logic & Automation
* **Data Transform / Calculations:** Automatically computes `Total Cost` based on ticket price and quantity.
* **Conditional Routing:** Routes execution assignments to dedicated Work Queues based on `Show Type`:
  * **`PremiumShowQueue`:** Assigned when `Show Type == "Premium"`.
  * **`StandardShowQueue`:** Default queue for standard screenings.
* **Service Level Agreement (SLA):**
  * **Goal:** 1 Day (Flags approaching target).
  * **Deadline:** 2 Days (Increases case urgency automatically).

### 4. Correspondence & Notification
On successful resolution of the lifecycle, an automated confirmation correspondence is triggered to the customer with dynamic case values.

---

## Notification Template (Final System Output)

```text
Subject: Movie Ticket Booking Confirmed - .pyID

Dear .CustomerName,

Your movie ticket booking has been successfully confirmed.

Dynamic Breakdown:
Case ID: .pyID
Movie Name: .MovieName
Show Date & Time: .ShowDate .ShowTime
Number of Tickets: .NumberOfTickets
Seat Numbers: .SeatNumbers
Total Cost: .TotalCost

Instructions: Please arrive at the theatre before show time and present your booking details at entry.

Thank you for choosing our services. Enjoy your movie!
— CineWave Entertainment Booking Support Team
