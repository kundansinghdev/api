# ELogistics-Tendering API Reference

## **1. Introduction**

 **COMPLETE** API Reference for the ELogistics Tendering Platform.
**Base URL**: `http://localhost:9090/api/v1`
**Content-Type**: `application/json`

---

## **2. Authentication & Public Endpoints**

**Controller**: `AuthController`
**Base Path**: `/auth`

### **2.1. Server Health Check**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/auth/ping`
- **Auth**: None
- **Response**:

```json
{
  "status": "ELogisticsTendering is up and running."
}
```

### **2.2. Register User**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/register`
- **Auth**: None
- **Body**:

```json
{
  "email": "transport@fastlogistics.com",
  "companyName": "FastLogistics",
  "mobileNumber": "9876543210",
  "location": "Mumbai",
  "password": "Password@123",
  "role": "LSP",
  "serviceZone": "NORTH"
}
```

_Note: `role` can be `LSP` or `3PL`. If `LSP`, `serviceZone` is required._

### **2.3. Verify Registration OTP**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/register/verify`
- **Auth**: None
- **Body**:

```json
{
  "mobileNumber": "9876543210",
  "otp": "123456"
}
```

### **2.4. Login (Email)**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/login`
- **Auth**: None
- **Body**:

```json
{
  "email": "transport@fastlogistics.com",
  "password": "Password@123"
}
```

- **Response**: Use the `jwtToken` for all subsequent requests.

### **2.5. Login (Mobile)**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/login/mobile`
- **Auth**: None
- **Body**:

```json
{
  "mobileNumber": "9876543210",
  "password": "Password@123"
}
```

### **2.6. Request Login OTP**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/otp/request`
- **Auth**: None
- **Body**:

```json
{
  "mobileNumber": "9876543210"
}
```

### **2.7. Verify Login OTP**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/otp/verify`
- **Auth**: None
- **Body**:

```json
{
  "mobileNumber": "9876543210",
  "otp": "123456"
}
```

---

## **3. Password Reset**

**Controller**: `PasswordResetController`
**Base Path**: `/auth/password-reset`

### **3.1. Request Reset**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/password-reset/request`
- **Auth**: None
- **Body**:

```json
{
  "email": "transport@fastlogistics.com"
}
```

### **3.2. Verify Reset OTP**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/password-reset/verify`
- **Auth**: None
- **Body**:

```json
{
  "email": "transport@fastlogistics.com",
  "otp": "654321"
}
```

### **3.3. Confirm New Password**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/auth/password-reset/confirm`
- **Auth**: None
- **Body**:

```json
{
  "email": "transport@fastlogistics.com",
  "newPassword": "NewPassword@123"
}
```

---

## **4. Tender Management (3PL Only)**

**Controller**: `TenderController`
**Base Path**: `/tenders`
**Auth**: Bearer Token (Role: **3PL**)

### **4.1. Create Tender**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/tenders?companyName={YOUR_COMPANY_NAME}`
- **Body**:

```json
{
  "sourceLocation": "Mumbai",
  "destinationLocation": "Delhi",
  "pickupDate": "2026-02-15",
  "dropDate": "2026-02-20",
  "deadline": "2026-02-14",
  "weight": 1200.0,
  "tenderPrice": 50000.0,
  "specialInstructions": "Fragile goods.",
  "tenderZone": "NORTH",
  "broadcastToAllZones": false
}
```

### **4.2. Update Tender**

- **Method**: `PUT`
- **URL**: `http://localhost:9090/api/v1/tenders/{tenderNo}?companyName={YOUR_COMPANY_NAME}`
- **Body**: Same as Create Tender.

### **4.3. Publish Tender**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/tenders/{tenderNo}/publish`

### **4.4. Search Tenders (Status)**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/tenders/search`
- **Body**:

```json
{
  "companyName": "My3PLCompany",
  "status": "ACTIVE"
}
```

---

## **5. Bidding (LSP Only)**

**Controller**: `BidController`
**Base Path**: `/bids`
**Auth**: Bearer Token (Role: **LSP**)

### **5.1. View My Bids**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/bids?companyName={YOUR_COMPANY_NAME}`

### **5.2. Place/Update Bid**

- **Method**: `PUT`
- **URL**: `http://localhost:9090/api/v1/bids/{tenderNo}/response?companyName={YOUR_COMPANY_NAME}`
- **Body**:

```json
{
  "estimatedArrivalDate": "2026-02-19",
  "bidPrice": 48000.0,
  "lspMessage": "We confirm availability."
}
```

### **5.3. Search Bids (Status)**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/bids/search`
- **Body**:

```json
{
  "companyName": "FastLSP",
  "status": "PENDING"
}
```

### **5.4. Find Specific Bid**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/bids/search/tender`
- **Body**:

```json
{
  "companyName": "FastLSP",
  "tender_no": "TDR-170123"
}
```

---

## **6. 3PL Operations (3PL Only)**

**Controller**: `ThreePlController`
**Base Path**: `/3pl`
**Auth**: Bearer Token (Role: **3PL**)

### **6.1. Confirm/Award Bid**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/3pl/bids/confirm`
- **Body**:

```json
{
  "tenderNo": "TDR-170123",
  "lspCompanyName": "FastLSP",
  "selectionMessage": "Lowest bid selected."
}
```

### **6.2. Get All Bids for Tender**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/3pl/tenders/{tenderNo}/bids`

### **6.3. Get Winning Bid**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/3pl/tenders/{tenderNo}/bids/confirmed`

### **6.4. Get Assignment for Tender**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/3pl/tenders/{tenderNo}/assignment`

### **6.5. Get All Assignments**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/3pl/assignments?companyName={YOUR_COMPANY_NAME}`

---

## **7. Network Management (3PL Only)**

**Controller**: `ThreePlNetworkController`
**Base Path**: `/networks`
**Auth**: Bearer Token (Role: **3PL**)

### **7.1. Add LSP to Network**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/networks/{companyName}/members`
- **Body**:

```json
{
  "lspPhone": "9876543210"
}
```

### **7.2. Get Network Members**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/networks/{companyName}/members`

### **7.3. Remove LSP**

- **Method**: `DELETE`
- **URL**: `http://localhost:9090/api/v1/networks/{companyName}/members/{lspPhone}`

---

## **8. Vehicle Assignments**

**Controller**: `LspAssignmentController`
**Base Path**: `/assignments`

### **8.1. Submit Vehicle (Via Bid ID)**

- **Role**: **LSP**
- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/assignments/bid/{bidId}`
- **Body**:

```json
{
  "vehicleNumber": "MH-04-AB-1234",
  "driverName": "Ramesh Kumar",
  "driverPhone": "9000012345"
}
```

### **8.2. Submit Vehicle (Via Tender No)**

- **Role**: **LSP**
- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/assignments/tender/{tenderNo}`
- **Body**: Same as above.

### **8.3. Get Assignment (Via Tender No)**

- **Role**: **Any**
- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/assignments/tender/{tenderNo}`

### **8.4. Get Assignment (Via Bid ID)**

- **Role**: **Any**
- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/assignments/bid/{bidId}`

### **8.5. Get My Assignments (LSP)**

- **Role**: **LSP**
- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/assignments?companyName={YOUR_COMPANY_NAME}`

---

## **9. User Directory**

**Controller**: `UserListController`
**Base Path**: `/users`
**Auth**: Bearer Token (Any Role)

### **9.1. List All Users**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/users`

### **9.2. List LSPs**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/users/lsp`

### **9.3. List 3PLs**

- **Method**: `GET`
- **URL**: `http://localhost:9090/api/v1/users/3pl`

### **9.4. Search Profile**

- **Method**: `POST`
- **URL**: `http://localhost:9090/api/v1/users/search/profile`
- **Body**:

```json
{
  "email": "transport@fastlogistics.com"
}
```
