
# **E-Hance API Documentation**

Welcome to the E-Hance API documentation. This guide details how to integrate and interact with our API, including endpoints for retrieving resources, managing shipments, and tracking data.

---

## **Table of Contents**
- [Authentication](#authentication)
- [Endpoints](#endpoints)
  - [Retrieve Client Resources](#1-retrieve-client-resources)
  - [Retrieve Shipment Details](#2-retrieve-shipment-details)
  - [Create Shipment](#3-create-shipment)
- [Data Types and Constants](#data-types-and-constants)
- [Error Handling](#error-handling)

---

### **Authentication**

All API requests require an authorization token in the request headers.

```http
Authorization: Bearer {user_token}
```

---

## **Endpoints**

### **1. Retrieve Client Resources**
**GET** `/get-client-resources`

Fetches specific data required for creating a shipment.

- **Query Parameters**:
  - `?data=states` (optional): Retrieve only states.
  - `?data=areas` (optional): Retrieve only areas.
  - `?data=addresses` (optional): Retrieve only addresses.
  - `?data=packages` (optional): Retrieve only packages.
  - `?data=carrier` (optional): Retrieve only carriers.
  - *Multiple values can be requested simultaneously*, e.g., `?data=states,areas,carrier`.

**Sample Response**:
```json
{
  "states": [{ "id": 1, "name": "State Name", "country_id": 1, "country_code": "EG", "iso2": "KAF" }],
  "areas": [{ "id": 1, "state_id": 1, "name": {"ar": "عربي", "en": "English"} }],
  "addresses": [{ "id": 1, "client_id": 1, "address": "Address", "country_id": 1, "state_id": 1, "area_id": 1 }],
  "packages": [{ "id": 1, "client_id": 1, "package_id": 1, "cost": 0, "name": {"en": "English", "ar": "عربي"} }],
  "carrier": [{ "id": 1, "name": "Carrier", "code": 1 }]
}
```

---

### **2. Retrieve Shipment Details**
**GET** `/get-shipment/{id}`

Fetches shipment data, including tracking information.

- **Path Parameter**:
  - `id` (required): Unique ID of the shipment.

**Sample Response**:
```json
{
  "id": 1,
  "code": "1",
  "type": "Pickup",
  "status_id": 1,
  "shipping_cost": 1,
  "total_weight": 1,
  "pickup_id": 1,
  "courier": "carrier",
  "amount_to_be_collected": 1,
  "receiver_name": "name",
  "receiver_phone": "01000000000",
  "tracking": {
    "Supplied": "2025-01-01 00:00:00",
    "Received": "2025-01-01 00:00:00",
    "Assigned": "2025-01-01 00:00:00",
    "Delivered": "2025-01-01 00:00:00"
  }
}
```

---

### **3. Create Shipment**
**POST** `/create-shipment`

Creates a new shipment, saving the details to the database and carrier.

- **Body Parameters**:
  - `type`: Type of shipment (1 for Pickup, 2 for Drop off).
  - `shipping_date`: Date of shipment in `YYYY-MM-DD`.
  - `collection_time`: Time of collection.
  - `client_address`: Client's address ID.
  - `receiver_name`: Name of the receiver.
  - `receiver_phone`: Receiver's phone number.
  - `receiver_address`: Address details.
  - `to_state_id`: State ID for the delivery location.
  - `payment_type`: 1 for Postpaid, 2 for Prepaid.
  - `payment_method_id`: `cash_payment` or `invoice_payment`.
  - Other parameters include `total_weight`, `amount_to_be_collected`, `order_id`, `captain_id`.

**Sample Response**:
```json
{
  "type": "Pickup",
  "branch_id": 1,
  "shipping_date": "2025-01-01",
  "collection_time": "00:00:00",
  "client_id": 1,
  "receiver_name": "name",
  "receiver_phone": "01000000000",
  "amount_to_be_collected": 1,
  "total_weight": 1,
  "status": "Pending",
  "created_at": "2025-01-01T00:00:00Z"
}
```

---

### **Data Types and Constants**
- **Shipment Types**:
  - `1`: Pickup
  - `2`: Drop off
- **Payment Types**:
  - `1`: Postpaid
  - `2`: Prepaid
- **Payment Methods**:
  - `cash_payment`
  - `invoice_payment`
  
--- 

