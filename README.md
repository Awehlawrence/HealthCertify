
# HealthCertify  Smart Contract

## Overview

The **HealthCertify Smart Contract** is designed to enable transparent, tamper-proof tracking of medical device lifecycles and certifications on a blockchain platform. By leveraging smart contracts, manufacturers, regulatory bodies, and service providers can collaboratively and securely record the status, ownership, and certifications of medical devices, ensuring compliance and traceability throughout the device’s lifecycle.

---

## Features

* **Device Registration:** Allows authorized entities (typically the contract owner or device manufacturers) to register new medical devices with an initial status.
* **Lifecycle Status Tracking:** Tracks the device status across key stages—Manufactured, Testing, Deployed, and Maintained.
* **Certification Management:** Allows approved regulatory bodies to issue certifications such as FDA, CE, ISO, and Safety certifications to devices.
* **Transparent History:** Maintains a timestamped history of status changes to support audits and compliance verification.
* **Access Control:** Enforces strict permissions so that only authorized parties can update device status or add certifications.
* **Regulatory Body Management:** Contract owner can add regulatory bodies authorized to certify devices.

---

## Constants

| Constant                     | Description                          |
| ---------------------------- | ------------------------------------ |
| `DEVICE_STATUS_MANUFACTURED` | Device has been manufactured         |
| `DEVICE_STATUS_TESTING`      | Device is undergoing testing         |
| `DEVICE_STATUS_DEPLOYED`     | Device is deployed in the field      |
| `DEVICE_STATUS_MAINTAINED`   | Device is maintained post-deployment |

| Certification Type | Description          |
| ------------------ | -------------------- |
| `CERT_TYPE_FDA`    | FDA certification    |
| `CERT_TYPE_CE`     | CE certification     |
| `CERT_TYPE_ISO`    | ISO certification    |
| `CERT_TYPE_SAFETY` | Safety certification |

---

## Error Codes

| Error Code                  | Description                             |
| --------------------------- | --------------------------------------- |
| `ERR_UNAUTHORIZED`          | Unauthorized action attempted           |
| `ERR_INVALID_DEVICE`        | Invalid device ID provided              |
| `ERR_STATUS_UPDATE_FAILED`  | Failed to update device status          |
| `ERR_INVALID_STATUS`        | Provided device status is invalid       |
| `ERR_INVALID_CERTIFICATION` | Invalid certification type              |
| `ERR_CERTIFICATION_EXISTS`  | Certification already exists for device |

---

## Data Structures

* **Device Details:** Stores the device owner, current status, and a history (up to 10 entries) of status changes with timestamps.
* **Device Certifications:** Tracks certifications issued to devices by regulatory bodies with timestamp and validity status.
* **Regulatory Bodies:** Maintains a list of approved authorities allowed to issue certifications for specific certification types.

---

## Usage

### Register a Device

```clarity
(register-device device-id initial-status)
```

* `device-id`: Unique identifier for the device (uint)
* `initial-status`: Initial status of the device (`DEVICE_STATUS_MANUFACTURED` recommended)
* Only contract owner or devices starting with `DEVICE_STATUS_MANUFACTURED` can register.

### Update Device Status

```clarity
(update-device-status device-id new-status)
```

* Updates the lifecycle status of the device.
* Can only be called by the device owner or contract owner.
* Appends the new status with a timestamp to the device’s history.

### Add Regulatory Body

```clarity
(add-regulatory-body authority cert-type)
```

* Only contract owner can call.
* Adds an approved regulatory authority for a specific certification type.

### Add Certification

```clarity
(add-certification device-id cert-type)
```

* Called by approved regulatory bodies.
* Issues a certification to the device.
* Fails if the certification already exists.

### Verify Certification

```clarity
(verify-certification device-id cert-type)
```

* Read-only function to check if a device holds a valid certification.

---

## Access Control

* **Contract Owner:** The deployer of the contract has the highest privileges including adding regulatory bodies.
* **Device Owner:** The entity that registered the device can update its status.
* **Regulatory Bodies:** Approved entities for specific certifications can add certifications.

---

## Security & Validations

* Device IDs are validated within a reasonable range.
* Statuses and certification types are strictly validated against defined constants.
* Regulatory authorities cannot be the contract owner or zero address.
* Certification cannot be duplicated.

---
