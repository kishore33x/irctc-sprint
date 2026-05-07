# IRCTC Feature Specifications — Part B

---

# Feature Spec 1: Tatkal Virtual Queue System

## Problem Statement

IRCTC servers become overloaded during Tatkal booking hours at 10:00 AM, causing crashes, session timeouts, and failed bookings. Millions of users attempt booking simultaneously without any queue visibility or booking progress information.

## Current State (from Part A)

Currently, users log in before 10:00 AM, fill passenger details, and click Book Now exactly at 10:00 AM. The page freezes, shows loading spinners, or crashes completely. Users do not know whether their request succeeded, failed, or is still processing.

## Proposed Solution

Introduce a virtual waiting room system for Tatkal bookings. Users entering Tatkal booking are automatically assigned a live queue position before the booking process begins. The interface displays estimated wait time and booking progress updates.

## Proposed User Flow — Step by Step

1. User logs into IRCTC before 10:00 AM.
2. User selects Tatkal quota and clicks Join Queue.
3. System assigns queue position.
4. User sees estimated wait time and live queue updates.
5. System automatically redirects user to booking page when turn arrives.
6. User completes booking without refreshing or repeated clicking.
7. Booking confirmation or failure status is shown clearly.

## Technical Implementation Plan

### System components affected
- Booking gateway
- Queue management service
- Frontend booking flow
- Session management system

### New data requirements
- Queue token
- Queue position
- Estimated wait time
- Request timestamp

### API changes
- POST /tatkal/queue/join
- GET /tatkal/queue/status

### Frontend changes
- Queue waiting screen
- Real-time progress bar
- Automatic booking redirect

### Third-party services
- Redis queue management
- WebSocket service for live updates

## Success Metrics

- Reduce Tatkal crashes by 70%
- Reduce repeated booking retries
- Improve successful booking completion rate
- Reduce average server load spikes

## Edge Cases and Constraints

- Queue server failure
- Users disconnecting during queue wait
- Government infrastructure limitations
- Slow internet users
- Graceful fallback to manual retry system

---

# Feature Spec 2: Persistent Smart Search Filters

## Problem Statement

IRCTC filters frequently reset or show incorrect results after navigation. Users must repeatedly apply filters and manually scan large train lists.

## Current State (from Part A)

Users apply Sleeper Class or Available Seat filters, but the system either resets filters after navigation or shows waitlisted trains despite availability-only filtering.

## Proposed Solution

Create persistent filter chips that remain active during navigation and synchronize filters with live availability updates.

## Proposed User Flow — Step by Step

1. User searches trains.
2. User selects filters.
3. Active filters appear as chips above results.
4. User clicks train details.
5. User returns to search results.
6. Previously selected filters remain active.
7. Results refresh dynamically without filter reset.

## Technical Implementation Plan

### System components affected
- Search service
- Frontend filter state management
- Availability API

### New data requirements
- User filter preferences
- Cached search session state

### API changes
- GET /search/results with filter state

### Frontend changes
- Filter chip UI
- Persistent filter state
- Cached search restoration

### Third-party services
- Browser local storage

## Success Metrics

- Reduce search abandonment rate
- Reduce average train search time
- Increase filter usage success rate

## Edge Cases and Constraints

- Cached results becoming outdated
- Mobile browser refresh behavior
- Slow API response handling

---

# Feature Spec 3: Locked Seat Selection Confirmation

## Problem Statement

Users lose selected berth preferences during booking flow transitions, especially on mobile devices.

## Current State (from Part A)

Users select lower berths, but after proceeding, the passenger details page resets the preference to Auto or another seat.

## Proposed Solution

Introduce temporary seat locking and confirmation syncing between the seat map and passenger details page.

## Proposed User Flow — Step by Step

1. User opens seat map.
2. User selects preferred seat.
3. Seat becomes temporarily locked for 2 minutes.
4. Passenger details page shows locked berth number.
5. User confirms booking.
6. Seat preference remains preserved throughout checkout.

## Technical Implementation Plan

### System components affected
- Seat management service
- Booking flow state management
- Mobile frontend rendering

### New data requirements
- Temporary seat lock token
- Lock expiry timestamp

### API changes
- POST /seat/lock
- POST /seat/confirm

### Frontend changes
- Seat confirmation banner
- Lock countdown timer

### Third-party services
- Redis temporary locking

## Success Metrics

- Reduce seat reset complaints
- Increase booking confidence
- Reduce repeated seat reselection attempts

## Edge Cases and Constraints

- Seat lock expiry
- Multiple users selecting same berth
- Mobile refresh behavior

---

# Feature Spec 4: Real-Time Payment Tracking

## Problem Statement

Users face uncertainty during UPI payments because the payment page shows endless loading without real-time status updates.

## Current State (from Part A)

Users initiate payment and see only a loading spinner. Many refresh or close the page due to lack of progress visibility.

## Proposed Solution

Introduce a live payment tracker with transaction stages and real-time updates.

## Proposed User Flow — Step by Step

1. User selects UPI payment.
2. User enters UPI ID.
3. User clicks Pay.
4. Payment tracker screen opens.
5. User sees stages:
   - Payment initiated
   - Bank verification
   - Railway confirmation
6. Final success or failure status appears clearly.

## Technical Implementation Plan

### System components affected
- Payment gateway integration
- Booking confirmation system

### New data requirements
- Transaction status logs
- Payment event timestamps

### API changes
- GET /payment/status

### Frontend changes
- Payment tracker component
- Animated status timeline

### Third-party services
- UPI gateway APIs

## Success Metrics

- Reduce payment-related support tickets
- Reduce accidental refreshes
- Increase successful payment completion rate

## Edge Cases and Constraints

- Payment gateway downtime
- Duplicate payment requests
- Delayed bank confirmations

---

# Feature Spec 5: Smart Session Timeout Warning

## Problem Statement

User sessions expire during long booking flows without warning, causing data loss and booking frustration.

## Current State (from Part A)

Users spend time entering passenger details, but the session expires silently and redirects them back to login.

## Proposed Solution

Add session expiry countdown warnings with automatic session refresh options.

## Proposed User Flow — Step by Step

1. User starts booking process.
2. Session timer runs in background.
3. One minute before expiry, popup warning appears.
4. User clicks Extend Session.
5. Session refreshes without data loss.
6. Booking continues normally.

## Technical Implementation Plan

### System components affected
- Authentication system
- Session management service

### New data requirements
- Session expiry timestamps

### API changes
- POST /session/refresh

### Frontend changes
- Session warning modal
- Countdown timer

### Third-party services
- None

## Success Metrics

- Reduce forced re-logins
- Reduce booking abandonment
- Improve session continuity

## Edge Cases and Constraints

- Session refresh failure
- User inactivity
- Network interruptions

---

# Feature Spec 6: Mobile Optimized Booking Interface

## Problem Statement

The IRCTC mobile website is cluttered and difficult to use on smaller screens.

## Current State (from Part A)

Users experience overlapping UI elements, excessive scrolling, and difficult navigation on mobile browsers.

## Proposed Solution

Design a mobile-first responsive booking interface optimized for low-end devices and smaller screens.

## Proposed User Flow — Step by Step

1. User opens IRCTC mobile website.
2. Simplified homepage loads.
3. User searches trains using larger input fields.
4. Results display in card layout.
5. Booking flow uses step-by-step mobile screens.
6. Payment process becomes touch-friendly and responsive.

## Technical Implementation Plan

### System components affected
- Frontend responsive UI
- Mobile rendering system

### New data requirements
- Device screen metadata

### API changes
- None

### Frontend changes
- Responsive layouts
- Mobile navigation
- Optimized buttons and forms

### Third-party services
- None

## Success Metrics

- Reduce mobile bounce rate
- Improve mobile booking completion rate
- Reduce mobile UI complaints

## Edge Cases and Constraints

- Low-end Android devices
- Slow mobile networks
- Browser compatibility issues

---