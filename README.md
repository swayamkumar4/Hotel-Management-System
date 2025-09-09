## Project Summary

A desktop hotel management application built with Python Tkinter and MySQL. It provides modules for customer management, room inventory setup, and room booking, with CRUD operations and basic billing calculation within the UI.

## Tech Stack

- Language/UI: Python, Tkinter (desktop GUI)
- Images: Pillow (PIL)
- Database: MySQL via mysql-connector-python

## Environment & Configuration

- MySQL database management on localhost with username root and password 1234 (used directly in DB calls).
- Tables used (inferred): customer, details (room catalog), room (bookings).
- Images are referenced by absolute Windows paths under an images directory; adjust paths as needed.

## System Architecture

- Entry point: hotel.py → HotelManagementSystem
  - Main window with buttons to open child windows via Toplevel:
    - Customer: customer.Cust_Win
    - Room Booking: room.Roombooking
    - Room Details: details.DetailsRoom
- Each module encapsulates its own UI layout and DB operations.

## Modules & Workflows

- Customer Management (customer.py → Cust_Win)

  - Fields: reference (auto random), name, mother, gender, postcode, mobile, email, nationality, ID proof and number, address.
  - Actions: Add, Update, Delete, Reset; View/Search by Mobile or Ref.
  - Data grid using ttk.Treeview with scrollbars; selection populates form for edit.
  - DB tables: customer with columns matching the UI fields (case-sensitive as used in queries).

- Room Catalog (details.py → DetailsRoom)

  - Define rooms: floor, room number, room type.
  - Actions: Add, Update, Delete, Reset; view all rooms in grid.
  - DB table: details(Floor, RoomNo, RoomType).

- Room Booking (room.py → Roombooking)
  - Inputs: customer contact (mobile), check-in/out dates (dd/mm/YYYY), room type (from details), available room (from details), meal, computed days, taxes, totals.
  - Actions: Bill (compute totals), Add, Update, Delete, Reset; View/Search by Contact or Room.
  - Fetch contact pulls customer info snippet for preview.
  - Pricing logic examples:
    - Breakfast + Luxury: specific base rates, 10% tax; similar branches for other combinations.
  - DB table: room(Contact, check_in, check_out, roomtype, roomavailable, meal, noOfdays).

## Data Flow

- The main window launches separate Toplevel windows; each module directly connects to MySQL for CRUD.
- Room booking pulls reference data from details and customer info from customer via the provided contact.

## Security & Robustness Considerations

- SQL statements are string-formatted in some places (e.g., search); prefer parameterized queries to prevent injection.
- Credentials are hardcoded; move to environment variables or a config file.
- Add input validation for dates, numeric fields, and selection defaults.
- Centralize DB connection handling; ensure proper closing/commit/rollback and error handling.
- Replace absolute image paths with relative paths or a config-driven assets folder.

## Setup & Run

1. Install dependencies: pip install pillow mysql-connector-python
2. Create MySQL database management with tables customer, details, and room matching field usage in code.
3. Adjust DB credentials in the code (or refactor to use a config file).
4. Run: python hotel.py.
