
---

# `part-b/MATRIX.md`

```md
# Impact vs Effort Matrix

## The Matrix

|                   | Low Effort | High Effort |
|-------------------|------------|--------------|
| **High Impact** | Smart Session Timeout Warning, Persistent Smart Search Filters | Tatkal Virtual Queue System, Real-Time Payment Tracking |
| **Low Impact** | Mobile Optimized Booking Interface | Locked Seat Selection Confirmation |

---

# How I Scored Each Dimension

## Impact Scoring (1–5)

I scored Impact based on:
- Number of users affected
- Frequency of occurrence
- Whether the issue affects core booking flow
- Severity of user frustration and booking failure

## Effort Scoring (1–5)

I scored Effort based on:
- Infrastructure requirements
- Backend complexity
- API dependency
- Frontend redesign complexity
- Risk of breaking existing IRCTC systems

---

# Placement Justifications

## Tatkal Virtual Queue System — High Impact / High Effort

Tatkal booking crashes affect millions of users daily during one of the most critical booking windows. Implementing a real-time queue system requires major backend infrastructure changes, WebSocket communication, and load balancing improvements. Despite high implementation effort, this should remain a top long-term priority because it solves one of IRCTC's most visible failures.

---

## Persistent Smart Search Filters — High Impact / Low Effort

Search filters are used by nearly all train search users. The issue mainly involves frontend state management and filter persistence rather than major infrastructure redesign. Because the impact is large and implementation effort is moderate, this is a strong quick-win improvement.

---

## Locked Seat Selection Confirmation — Low Impact / High Effort

Seat reset issues affect a smaller percentage of booking users compared to Tatkal or payment failures. However, implementing temporary seat locking and synchronization across devices requires backend coordination and concurrency handling. This feature is important but lower priority than booking stability improvements.

---

## Real-Time Payment Tracking — High Impact / High Effort

Payment uncertainty creates panic, duplicate transactions, and refund complaints. Implementing live payment tracking requires coordination with banking systems and payment gateways. Since payment reliability directly affects user trust and revenue, this feature deserves high priority despite technical complexity.

---

## Smart Session Timeout Warning — High Impact / Low Effort

Unexpected session expiry frustrates users and causes repeated booking attempts. The solution mainly requires frontend timers and lightweight backend session refresh APIs. Because implementation is relatively simple and user benefit is immediate, this is one of the best sprint candidates.

---

## Mobile Optimized Booking Interface — Low Impact / Low Effort

The mobile UI creates usability issues but usually does not completely block booking. Responsive UI redesign requires frontend work but minimal backend changes. This makes it a good iterative improvement after critical booking and payment problems are solved.

---

# Recommended Sprint Order

1. Smart Session Timeout Warning — quick implementation with immediate user benefit
2. Persistent Smart Search Filters — improves search usability for most users
3. Real-Time Payment Tracking — reduces transaction confusion and refund complaints
4. Tatkal Virtual Queue System — major infrastructure improvement for booking stability
5. Mobile Optimized Booking Interface — improves accessibility and mobile usability
6. Locked Seat Selection Confirmation — important but lower priority compared to booking/payment reliability