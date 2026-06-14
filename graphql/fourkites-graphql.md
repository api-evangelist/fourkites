# FourKites GraphQL Schema

## Overview

FourKites is a real-time supply chain visibility platform that tracks shipments and assets across road, rail, ocean, and yard modes worldwide. The GraphQL schema below is a conceptual representation of the FourKites domain model derived from the publicly documented REST API capabilities, including shipment tracking, ETA prediction, carrier management, exception handling, and supply chain analytics.

FourKites provides partner-access APIs for shipment creation, status updates, ETA computation, and carrier integrations across truckload (TL), less-than-truckload (LTL), ocean, rail, and parcel modes.

## Schema Source

This schema is conceptual, derived from:
- FourKites Developer Documentation: https://developers.fourkites.com/
- FourKites Platform Capabilities: https://www.fourkites.com/

The REST API is available to partners at https://tracking-api.fourkites.com.

## Core Domain Types

### Shipment Domain
- **Shipment** — Top-level entity representing a freight movement from origin to destination
- **ShipmentDetails** — Extended metadata about a shipment including references and attributes
- **ShipmentStatus** — Current lifecycle state of a shipment
- **ShipmentLocation** — Real-time geographic position of a shipment
- **ShipmentETA** — Predicted arrival time at destination or next stop
- **ShipmentMetrics** — Performance and operational statistics for a shipment

### ETA and Prediction Domain
- **ETAConfidence** — Confidence score and interval for an ETA prediction
- **ETAHistory** — Historical record of ETA changes over a shipment's lifecycle
- **ETAEvent** — A discrete ETA recalculation event
- **ETAUpdate** — Payload representing an ETA revision
- **PredictedEvent** — A machine-learning forecasted milestone or event

### Carrier Domain
- **Carrier** — A freight carrier entity
- **CarrierDetails** — Extended carrier information including modes and certifications
- **CarrierCode** — Standardized carrier identifier
- **SCAC** — Standard Carrier Alpha Code identifier
- **CarrierScore** — Carrier performance rating and on-time metrics

### Driver and Vehicle Domain
- **Driver** — A driver entity associated with a load
- **DriverDetails** — Extended driver information
- **DriverLocation** — Real-time GPS position of a driver
- **DriverETA** — Driver-reported or system-computed ETA
- **Vehicle** — A vehicle asset used for freight transport
- **VehicleDetails** — Extended vehicle metadata
- **VehicleType** — Classification of vehicle (e.g., dry van, flatbed, reefer)
- **VehicleID** — Standardized vehicle identifier
- **TruckID** — Tractor/power unit identifier
- **TrailerID** — Trailer unit identifier

### Customer and Location Domain
- **Customer** — A shipper or consignee entity
- **CustomerDetails** — Extended customer profile
- **Origin** — Starting location of a shipment
- **Destination** — Ending location of a shipment
- **Waypoint** — An intermediate geographic point along a route
- **Stop** — A planned or actual stop on a shipment's route
- **StopType** — Classification of stop (pickup, delivery, fuel, etc.)
- **StopStatus** — Current state of a stop

### Milestone Domain
- **Milestone** — A significant event in a shipment's lifecycle
- **MilestoneType** — Classification of milestone

### Load and Equipment Domain
- **Load** — A freight load assignment
- **LoadDetails** — Extended load information
- **LoadOrder** — An order or purchase order attached to a load
- **LoadItem** — An individual item or commodity within a load
- **Equipment** — Physical freight equipment
- **EquipmentType** — Classification of equipment

### Alert and Exception Domain
- **Alert** — A notification triggered by a shipment event or threshold breach
- **AlertType** — Classification of alert
- **AlertPriority** — Urgency level of an alert
- **Exception** — A deviation from expected shipment behavior
- **ExceptionType** — Classification of exception

### Integration and Event Domain
- **IntegrationEvent** — An event exchanged with an external TMS, ERP, or WMS system
- **Webhook** — A webhook subscription for real-time event delivery

### Supply Chain Domain
- **SupplyChain** — An aggregated view of a supply chain network
- **SupplyChainNetwork** — The full graph of nodes and lanes in a supply chain

### Risk and Disruption Domain
- **WeatherImpact** — Weather-related risk affecting a shipment route
- **CapacityIssue** — A capacity constraint or equipment shortage alert
- **DisruptionAlert** — A broader supply chain disruption notification
- **RiskAssessment** — Composite risk score and contributing factors

### Performance Domain
- **PerformanceData** — Aggregated operational performance statistics
- **OTP** — On-time performance metric

### Access Control Domain
- **APIKey** — An API credential for programmatic access
- **Token** — An authentication token

## Key Queries

- `shipment(id: ID!)` — Fetch a single shipment by ID
- `shipments(filter: ShipmentFilter, page: Int, pageSize: Int)` — List shipments with filtering
- `carrier(id: ID!)` — Fetch a carrier by ID
- `carriers(filter: CarrierFilter)` — List carriers
- `load(id: ID!)` — Fetch a load by ID
- `driver(id: ID!)` — Fetch a driver by ID
- `alerts(shipmentId: ID)` — List alerts for a shipment
- `exceptions(filter: ExceptionFilter)` — List exceptions
- `performanceData(carrierId: ID, dateRange: DateRangeInput)` — Query carrier performance
- `supplyChain(id: ID!)` — Fetch supply chain network view

## Key Mutations

- `createShipment(input: CreateShipmentInput!)` — Create a new shipment
- `updateShipmentStatus(id: ID!, status: ShipmentStatusEnum!)` — Update shipment status
- `updateShipmentLocation(id: ID!, location: LocationInput!)` — Push a location update
- `acknowledgeAlert(id: ID!)` — Acknowledge an alert
- `resolveException(id: ID!)` — Mark an exception resolved
- `createWebhook(input: WebhookInput!)` — Register a webhook
- `deleteWebhook(id: ID!)` — Remove a webhook

## Key Subscriptions

- `shipmentLocationUpdated(shipmentId: ID!)` — Real-time location stream
- `shipmentETAUpdated(shipmentId: ID!)` — Real-time ETA change stream
- `alertCreated(customerId: ID)` — Stream of new alerts
- `exceptionCreated(customerId: ID)` — Stream of new exceptions
