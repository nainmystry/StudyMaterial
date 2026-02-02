Parking Lot HLD for Interview
High-Level Overview:
A Vehicle Parking Management System that efficiently allocates parking spots, manages entry/exit, calculates fees, and handles different vehicle types using strategy-based allocation and factory-based object creation.

1. Core Architecture
Entry/Exit Gates: Physical/automated gates with sensors and cameras

Central Management System: Coordinates allocation, tracking, and billing

User Interfaces: Mobile app, kiosks, display boards

Database: Stores parking sessions, user accounts, payment records

2. Key Components & Entities
Core Entities (with SRP):
ParkingLotSystem - Main orchestrator, coordinates all operations

ParkingFloor - Manages spots on individual floors

ParkingSpot - Represents a single parking space

Attributes: spotId, type (Compact/Large/EV/Handicapped), floor, isOccupied

Vehicle Factory - Creates vehicle objects

Vehicle base class → Car, Bike, Truck, EVVehicle

AllocationStrategy Interface - Base for parking algorithms

NearestEntryStrategy - Closest to entry gate

EVFirstStrategy - Prioritizes EV spots

CompactFirstStrategy - Maximizes space utilization

ReservedStrategy - For pre-booked spots

Ticket - Entry ticket with timestamp, vehicle, spot details

PricingStrategy - Calculates parking fees

HourlyPricing, DailyPricing, EventPricing, MembershipDiscount

PaymentProcessor - Handles cash, card, mobile payments

GateController - Manages gate operations (open/close)

Supporting Entities:
User/Driver - Driver information and preferences

ReservationSystem - Pre-booking management

DisplayBoard - Shows available spots per floor

NotificationService - Alerts for time expiration, reminders

MaintenanceModule - Tracks spot maintenance status

AnalyticsEngine - Usage patterns, revenue reports

3. Component Diagram (PlantUML)



4. Data Flow
Entry Flow:
Vehicle Detection: Camera/license plate recognition at entry gate

Vehicle Creation: Factory creates appropriate vehicle object

Spot Allocation:

System selects AllocationStrategy based on time/vehicle type

Searches available spots through ParkingFloor hierarchy

Ticket Generation: Creates Ticket with entry time, spot assignment

Gate Opening: GateController opens gate, updates display boards

Exit Flow:
Ticket Validation: Scan ticket or license plate

Fee Calculation: PricingStrategy computes fee based on duration

Payment Processing: PaymentProcessor handles transaction

Spot Release: Mark spot as available, update inventory

Analytics Logging: Record session data for reporting

5. Scalability & Extensibility
Multi-location Support: Manage multiple parking lots

Dynamic Pricing: Surge pricing during peak hours/events

Smart Features:

License plate recognition for automatic entry/exit

Mobile app integration for spot reservation

Guidance systems with LED indicators

Access Control: Different access levels (staff, VIP, public)

Integration: Payment gateways, city traffic systems

6. Why These Patterns?
Strategy Pattern: Allows switching allocation/pricing algorithms without modifying core logic. New strategies can be added for special events or vehicle types.

Factory Pattern: Centralizes vehicle creation, making it easy to add new vehicle types (e.g., autonomous vehicles, scooters) with minimal code changes.

7. Interview Talking Points
"The Strategy Pattern lets us experiment with different allocation algorithms based on real-time occupancy data"

"Factory Pattern ensures vehicle-specific logic (like size validation) is encapsulated"

"We can implement A/B testing for pricing strategies to maximize revenue"

"The system scales horizontally by adding more parking floors with consistent interfaces"

"Component diagram shows clear separation of concerns - allocation, pricing, and vehicle management are independent"

Interview Delivery Tip:
Show the component diagram first to establish the big picture, then walk through the entry/exit flows. Emphasize how the strategy and factory patterns enable flexibility. Draw parallels to real systems like airport parking or hotel valet.
