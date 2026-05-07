# IRCTC Problem Discovery — Part A

## Summary
- Total problems documented: 6 (3 given + 3 self-discovered)
- Platform explored: irctc.co.in
- Devices used: Desktop Chrome and Mobile Chrome

---

# Problem 1: Tatkal Booking Crashes at 10:00 AM [Given]

## What is broken
The IRCTC server becomes extremely slow or crashes during Tatkal booking opening time at 10:00 AM. Users face loading freezes, session timeouts, payment failures, and booking loss during the peak rush window.

## Affected users
Daily Tatkal users, especially working professionals, students, and passengers from Tier 2 and Tier 3 cities who depend heavily on train travel.

Estimated affected users:
20–40 lakh users during the Tatkal opening window.

## Frequency
Occurs every day between 9:58 AM and 10:05 AM during Tatkal booking hours.

## Current flow — step by step

1. User opens IRCTC around 9:50 AM.
2. User logs in and searches for trains.
3. User selects Tatkal quota and sees available seats.
4. User fills passenger details before 10:00 AM.
5. User clicks "Book Now" exactly at 10:00 AM.
6. Website freezes and loading spinner continues endlessly.
7. User receives HTTP 502 error or session timeout.
8. User refreshes page and is logged out.
9. Tatkal seats become waitlisted or unavailable.

## Where exactly it breaks
The failure occurs at steps 5–7 when millions of concurrent requests hit the booking server simultaneously. The system provides no queue status or progress feedback, causing repeated clicks and additional server overload.

---

# Problem 2: Search Filters Do Not Work Reliably [Given]

## What is broken
Search filters such as Sleeper Class, Available Seats, and departure timing often reset automatically or display incorrect train results.

## Affected users
All train search users, especially senior citizens and first-time users who depend on filters for easier train selection.

Estimated affected users:
Most active IRCTC users during train search sessions.

## Frequency
Occurs intermittently, especially during high traffic periods.

## Current flow — step by step

1. User searches trains between two cities.
2. Results display multiple trains and classes.
3. User applies filters like Sleeper Class and Available seats.
4. Filtered results appear temporarily.
5. User clicks a train to check details.
6. User goes back to results page.
7. Filters reset automatically.
8. User sees waitlisted trains despite selecting "Available" filter.
9. User manually searches through trains again.

## Where exactly it breaks
The failure occurs at steps 5–7 because filter state is not preserved during page refreshes and cached results may not sync with live availability data.

---

# Problem 3: Seat Selection Resets Randomly [Given]

## What is broken
Selected seat preferences are sometimes lost after proceeding to passenger details, especially on mobile devices.

## Affected users
Families, elderly passengers, disabled passengers, and users requiring specific berth preferences.

Estimated affected users:
30–40% of booking users.

## Frequency
Occurs in approximately 15–25% of booking sessions and more frequently on mobile devices.

## Current flow — step by step

1. User searches and selects train.
2. User proceeds to seat map.
3. User selects preferred berth.
4. Selected berth turns highlighted.
5. User clicks Proceed.
6. Passenger details page loads.
7. Preferred berth changes to Auto or another seat.
8. User returns to seat map.
9. Original seat becomes unavailable.

## Where exactly it breaks
The issue occurs at steps 5–7 because the selected seat state is not consistently preserved between UI components during navigation and mobile page rendering.

---

# Problem 4: Payment Page Freezes During UPI Transactions [Self-Discovered]

## What is broken
During UPI payments, the payment page shows an endless loading spinner without real-time payment confirmation or status updates.

## How I found it
I attempted a booking flow up to the payment page using UPI payment mode on desktop Chrome.

## Screenshot or description
Screenshot: assets/screenshots/payment-spinner.png

## Affected users
Users paying through UPI, especially mobile users using slower internet connections.

Estimated affected users:
A large percentage of users because UPI is one of the most common payment methods in India.

## Frequency
Occurs frequently during peak booking hours and high traffic periods.

## Current flow — step by step

1. User completes passenger details.
2. User reaches payment page.
3. User selects UPI payment option.
4. User enters UPI ID and clicks Pay.
5. Spinner appears with no status updates.
6. User waits without knowing payment status.
7. User refreshes page or closes tab.
8. User later notices payment deduction without ticket confirmation.
9. User must wait for refund process.

## Where exactly it breaks
The failure occurs at steps 5–7 because there is no real-time payment tracking or transaction progress communication to the user.

---

# Problem 5: Session Timeout During Long Booking Process [Self-Discovered]

## What is broken
The user session expires automatically during long booking flows without warning.

## How I found it
I stayed idle for a few minutes while filling passenger details and reviewing train information.

## Screenshot or description
Screenshot: assets/screenshots/session-timeout.png

## Affected users
Slow internet users, elderly users, and first-time users who require extra time while booking.

Estimated affected users:
A significant number of mobile and low-bandwidth users.

## Frequency
Occurs often when users remain inactive for several minutes.

## Current flow — step by step

1. User logs into IRCTC.
2. User searches trains and selects booking.
3. User slowly fills passenger details.
4. User reviews train and payment details.
5. Session silently expires in background.
6. User clicks Continue or Pay.
7. Website redirects user to login page.
8. Entered passenger information is lost.
9. User restarts entire booking flow.

## Where exactly it breaks
The failure occurs at steps 5–7 because the system expires sessions without countdown warnings or automatic session refresh handling.

---

# Problem 6: Mobile Website Layout Breaks on Smaller Screens [Self-Discovered]

## What is broken
The mobile website layout becomes cluttered and difficult to use on smaller screen devices.

## How I found it
I opened the IRCTC website using mobile Chrome and attempted to search and book trains.

## Screenshot or description
Screenshot: assets/screenshots/mobile-layout.png

## Affected users
Mobile users, especially users with low-end Android devices and smaller screens.

Estimated affected users:
Millions of mobile-first users across India.

## Frequency
Occurs consistently on smaller mobile screens.

## Current flow — step by step

1. User opens IRCTC website on mobile browser.
2. Homepage loads with multiple banners and menus.
3. User attempts to search trains.
4. Search fields overlap or require excessive scrolling.
5. Buttons become difficult to tap accurately.
6. User navigates between pages.
7. Layout shifts unexpectedly.
8. Some text becomes difficult to read.
9. User experiences frustration and slower booking process.

## Where exactly it breaks
The issue occurs at steps 4–7 because the responsive design is inconsistent and not optimized properly for smaller mobile screen sizes.

---