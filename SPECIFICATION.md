# CODS Technical Specification v1.0

**Client Onboarding Data Standard**  
**Version:** 1.0.0  
**Status:** Stable  
**Last Updated:** February 6, 2026

## Table of Contents

1. [Introduction](#introduction)
2. [Core Principles](#core-principles)
3. [Data Schema](#data-schema)
4. [API Specification](#api-specification)
5. [Authentication](#authentication)
6. [Error Handling](#error-handling)
7. [Validation Rules](#validation-rules)
8. [Extensibility](#extensibility)
9. [Versioning](#versioning)

## Introduction

The Client Onboarding Data Standard (CODS) defines a universal format for exchanging client onboarding information in the pet waste management industry.

### Goals

- **Interoperability** - Enable data exchange between any systems
- **Simplicity** - Easy to implement and understand
- **Extensibility** - Support future enhancements without breaking changes
- **Security** - Protect sensitive client information
- **Validation** - Ensure data quality and completeness

### Scope

This specification covers:
- Client contact and address information
- Pet details and special requirements
- Service requests and scheduling preferences
- Pricing and payment information
- Legal agreements and consent tracking
- Marketing attribution and metadata

## Core Principles

### 1. JSON Format

All CODS data MUST be valid JSON (RFC 8259).

### 2. UTF-8 Encoding

All text MUST be UTF-8 encoded.

### 3. ISO Standards

- Dates: ISO 8601 (YYYY-MM-DD)
- Times: ISO 8601 with timezone (2024-02-06T10:30:00Z)
- Phone numbers: E.164 format recommended
- Country codes: ISO 3166-1 alpha-2

### 4. Required vs Optional

- **MUST** - Required field
- **SHOULD** - Recommended field
- **MAY** - Optional field

### 5. Null Handling

- Use `null` for unknown values
- Omit optional fields entirely if not applicable
- Empty strings `""` are discouraged; use `null` instead

## Data Schema

### Root Object

```json
{
  "standard": "CODS",
  "version": "1.0",
  "timestamp": "2024-02-06T10:30:00Z",
  "source": "string",
  "client": { },
  "service_request": { },
  "pricing": { },
  "payment": { },
  "preferences": { },
  "agreements": { },
  "metadata": { }
}
```

#### Root Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `standard` | string | MUST | Always "CODS" |
| `version` | string | MUST | Spec version (e.g., "1.0") |
| `timestamp` | string | MUST | ISO 8601 timestamp when data was created |
| `source` | string | SHOULD | Origin of data (e.g., "website-form", "phone-intake") |

### Client Object

```json
{
  "client": {
    "contact": {
      "first_name": "Sarah",
      "last_name": "Johnson",
      "email": "sarah@example.com",
      "phone": "+19195550123",
      "phone_type": "mobile",
      "preferred_contact_method": "sms",
      "language": "en",
      "timezone": "America/New_York"
    },
    
    "service_address": {
      "street": "123 Oak Street",
      "unit": null,
      "city": "Raleigh",
      "state": "NC",
      "zip": "27601",
      "country": "US",
      "coordinates": {
        "latitude": 35.7796,
        "longitude": -78.6382
      },
      "property_type": "single_family",
      "lot_size_sqft": 8000,
      "fenced": true,
      "fence_type": "privacy",
      "gate_code": "1234#",
      "gate_location": "backyard-left",
      "parking_instructions": "Driveway available",
      "access_instructions": "Gate on left side of house"
    },
    
    "billing_address": {
      "same_as_service": true,
      "street": null,
      "unit": null,
      "city": null,
      "state": null,
      "zip": null,
      "country": null
    },
    
    "pets": [
      {
        "name": "Max",
        "species": "dog",
        "breed": "Golden Retriever",
        "weight_lbs": 65,
        "age_years": 3,
        "age_months": 2,
        "gender": "male",
        "spayed_neutered": true,
        "temperament": "friendly",
        "aggression_level": "none",
        "special_needs": null,
        "medical_conditions": [],
        "dietary_restrictions": null,
        "photo_url": null
      }
    ],
    
    "emergency_contact": {
      "name": "John Johnson",
      "relationship": "spouse",
      "phone": "+19195550124",
      "email": "john@example.com"
    }
  }
}
```

#### Client.Contact Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `first_name` | string | MUST | Client's first name |
| `last_name` | string | MUST | Client's last name |
| `email` | string | MUST | Valid email address |
| `phone` | string | MUST | Phone number (E.164 recommended) |
| `phone_type` | enum | SHOULD | "mobile", "home", "work" |
| `preferred_contact_method` | enum | SHOULD | "email", "sms", "phone", "app" |
| `language` | string | MAY | ISO 639-1 language code |
| `timezone` | string | MAY | IANA timezone identifier |

#### Client.Service_Address Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `street` | string | MUST | Street address |
| `unit` | string | MAY | Apartment/unit number |
| `city` | string | MUST | City name |
| `state` | string | MUST | State/province code |
| `zip` | string | MUST | Postal code |
| `country` | string | MUST | ISO 3166-1 alpha-2 |
| `coordinates` | object | SHOULD | GPS coordinates |
| `coordinates.latitude` | number | - | Decimal degrees |
| `coordinates.longitude` | number | - | Decimal degrees |
| `property_type` | enum | SHOULD | "single_family", "townhouse", "condo", "apartment", "commercial" |
| `lot_size_sqft` | integer | MAY | Square footage |
| `fenced` | boolean | SHOULD | Is yard fenced? |
| `fence_type` | enum | MAY | "privacy", "chain_link", "split_rail", "invisible", "partial" |
| `gate_code` | string | MAY | Access code for gate |
| `gate_location` | string | MAY | Where gate is located |
| `parking_instructions` | string | MAY | Where to park |
| `access_instructions` | string | MAY | How to access property |

#### Client.Pets[] Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | MUST | Pet's name |
| `species` | enum | MUST | "dog", "cat", "other" |
| `breed` | string | SHOULD | Breed name or "mixed" |
| `weight_lbs` | number | SHOULD | Weight in pounds |
| `age_years` | integer | SHOULD | Age in years |
| `age_months` | integer | MAY | Additional months |
| `gender` | enum | MAY | "male", "female", "unknown" |
| `spayed_neutered` | boolean | MAY | Fixed status |
| `temperament` | enum | SHOULD | "friendly", "shy", "protective", "aggressive" |
| `aggression_level` | enum | MAY | "none", "low", "medium", "high" |
| `special_needs` | string | MAY | Any special requirements |
| `medical_conditions` | array | MAY | Array of condition strings |
| `dietary_restrictions` | string | MAY | Diet information |
| `photo_url` | string | MAY | URL to pet photo |

### Service_Request Object

```json
{
  "service_request": {
    "service_type": "recurring",
    "frequency": "weekly",
    "preferred_day": "tuesday",
    "preferred_time": "morning",
    "alternate_day": "thursday",
    "start_date": "2024-02-13",
    "end_date": null,
    "area_to_service": "entire_yard",
    "estimated_waste_volume": "medium",
    "special_instructions": "Please close gate after service",
    "additional_services": [
      "deodorizing",
      "waste_removal"
    ],
    "skip_dates": [
      "2024-12-25",
      "2024-12-26"
    ]
  }
}
```

#### Service_Request Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `service_type` | enum | MUST | "recurring", "one_time", "initial_only" |
| `frequency` | enum | MUST* | "weekly", "biweekly", "monthly", "twice_weekly", "custom" |
| `preferred_day` | enum | SHOULD | "monday", "tuesday", etc. |
| `preferred_time` | enum | SHOULD | "morning", "afternoon", "evening", "anytime" |
| `alternate_day` | enum | MAY | Backup day preference |
| `start_date` | string | MUST | ISO 8601 date |
| `end_date` | string | MAY | ISO 8601 date (null for ongoing) |
| `area_to_service` | enum | SHOULD | "entire_yard", "front_only", "back_only", "custom" |
| `estimated_waste_volume` | enum | MAY | "low", "medium", "high" |
| `special_instructions` | string | MAY | Free-form instructions |
| `additional_services` | array | MAY | Array of service add-ons |
| `skip_dates` | array | MAY | Array of ISO 8601 dates to skip |

*Required if service_type is "recurring"

### Pricing Object

```json
{
  "pricing": {
    "quoted_price": 29.99,
    "currency": "USD",
    "billing_frequency": "monthly",
    "initial_cleanup_price": 75.00,
    "one_time_price": null,
    "promotion_code": "SPRING2024",
    "discount_applied": 10.00,
    "discount_type": "percentage",
    "tax_rate": 0.0725,
    "price_locked": true,
    "price_lock_duration_months": 12
  }
}
```

#### Pricing Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `quoted_price` | number | MUST | Regular service price |
| `currency` | string | MUST | ISO 4217 currency code |
| `billing_frequency` | enum | MUST | "per_service", "weekly", "monthly", "quarterly", "annually" |
| `initial_cleanup_price` | number | MAY | One-time initial cleanup fee |
| `one_time_price` | number | MAY | For one-time service requests |
| `promotion_code` | string | MAY | Code applied |
| `discount_applied` | number | MAY | Discount amount |
| `discount_type` | enum | MAY | "fixed", "percentage" |
| `tax_rate` | number | MAY | Sales tax rate as decimal |
| `price_locked` | boolean | MAY | Is price guaranteed? |
| `price_lock_duration_months` | integer | MAY | How long price is locked |

### Payment Object

```json
{
  "payment": {
    "method": "credit_card",
    "processor": "stripe",
    "payment_token": "tok_visa_4242",
    "card_last_four": "4242",
    "card_brand": "visa",
    "billing_day": 1,
    "auto_pay": true,
    "require_initial_payment": true,
    "initial_payment_amount": 75.00,
    "payment_terms": "net_15"
  }
}
```

#### Payment Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `method` | enum | MUST | "credit_card", "ach", "check", "cash", "invoice" |
| `processor` | enum | SHOULD | "stripe", "square", "paypal", "fiserv", "other" |
| `payment_token` | string | MAY | Tokenized payment method (NEVER raw card data) |
| `card_last_four` | string | MAY | Last 4 digits for display |
| `card_brand` | enum | MAY | "visa", "mastercard", "amex", "discover" |
| `billing_day` | integer | MAY | Day of month for recurring billing (1-28) |
| `auto_pay` | boolean | SHOULD | Automatic payment enabled? |
| `require_initial_payment` | boolean | MAY | Charge before first service? |
| `initial_payment_amount` | number | MAY | Amount to charge upfront |
| `payment_terms` | enum | MAY | "due_on_service", "net_15", "net_30" |

**Security Note:** NEVER include raw credit card numbers, CVV, or full account numbers. Always use payment processor tokens.

### Preferences Object

```json
{
  "preferences": {
    "notifications": {
      "service_reminders": true,
      "arrival_alerts": true,
      "completion_notifications": true,
      "invoice_emails": true,
      "receipt_emails": true,
      "marketing_emails": false,
      "sms_notifications": true
    },
    "service_preferences": {
      "service_photos": true,
      "service_reports": true,
      "waste_removal": true,
      "deodorizing": false,
      "leave_door_hanger": false
    },
    "communication_preferences": {
      "preferred_language": "en",
      "text_ok": true,
      "call_ok": true,
      "email_ok": true
    }
  }
}
```

### Agreements Object

```json
{
  "agreements": {
    "terms_accepted": true,
    "terms_version": "2024-01-15",
    "terms_url": "https://example.com/terms",
    "privacy_policy_accepted": true,
    "privacy_policy_version": "2024-01-15",
    "privacy_policy_url": "https://example.com/privacy",
    "signature": "Sarah Johnson",
    "signature_type": "typed",
    "ip_address": "192.168.1.1",
    "user_agent": "Mozilla/5.0...",
    "accepted_at": "2024-02-06T10:30:00Z"
  }
}
```

#### Agreements Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `terms_accepted` | boolean | MUST | Terms of service accepted |
| `terms_version` | string | MUST | Version/date of terms |
| `terms_url` | string | SHOULD | URL to terms document |
| `signature` | string | SHOULD | Name as typed/signed |
| `signature_type` | enum | MAY | "typed", "drawn", "click" |
| `ip_address` | string | SHOULD | IP address of acceptance |
| `accepted_at` | string | MUST | ISO 8601 timestamp |

### Metadata Object

```json
{
  "metadata": {
    "referral_source": "google_ads",
    "referral_source_detail": "Spring Campaign 2024",
    "campaign_id": "spring_2024_raleigh",
    "utm_source": "google",
    "utm_medium": "cpc",
    "utm_campaign": "spring_2024",
    "utm_term": "pooper scooper raleigh",
    "utm_content": "ad_variant_a",
    "referring_customer_id": null,
    "referring_customer_name": null,
    "sales_rep": "John Smith",
    "sales_rep_id": "SR-001",
    "lead_source": "website",
    "lead_score": 85,
    "notes": "Mentioned neighbor uses service. Wants to start ASAP.",
    "tags": ["hot_lead", "premium_area", "multiple_dogs"],
    "custom_fields": {
      "heard_about_us": "Neighborhood Facebook group",
      "main_concern": "Kids play in yard"
    }
  }
}
```

## API Specification

### Endpoint

All CODS-compatible systems MUST implement:

```
POST /api/v1/cods/onboarding
```

Vendors MAY implement additional endpoints but this is the required standard endpoint.

### Request Headers

```
Content-Type: application/json
X-CODS-Version: 1.0
X-API-Key: {api_key}
```

Optional but recommended:
```
X-Idempotency-Key: {unique_id}
X-Request-ID: {tracking_id}
```

### Request Body

Complete CODS JSON object as defined in the Data Schema section.

### Success Response

**Status Code:** `200 OK` or `201 Created`

```json
{
  "status": "success",
  "client_id": "CLT-12345",
  "onboarding_id": "OB-2024-00123",
  "created_at": "2024-02-06T10:30:00Z",
  "next_steps": [
    {
      "action": "schedule_initial_cleanup",
      "url": "https://app.example.com/schedule/CLT-12345",
      "required": true,
      "deadline": "2024-02-13T00:00:00Z"
    },
    {
      "action": "upload_pet_photo",
      "url": "https://app.example.com/pets/photo",
      "required": false
    }
  ],
  "client_portal": {
    "url": "https://portal.example.com",
    "username": "sarah@example.com",
    "login_token": "eyJhbGc...",
    "expires_at": "2024-02-06T11:30:00Z"
  },
  "notifications_sent": [
    {
      "type": "welcome_email",
      "recipient": "sarah@example.com",
      "sent_at": "2024-02-06T10:30:15Z",
      "status": "delivered"
    },
    {
      "type": "welcome_sms",
      "recipient": "+19195550123",
      "sent_at": "2024-02-06T10:30:16Z",
      "status": "delivered"
    }
  ],
  "messages": [
    "Account created successfully",
    "Welcome email sent",
    "Initial cleanup scheduled for Feb 13, 2024"
  ]
}
```

### Error Response

**Status Codes:**
- `400 Bad Request` - Invalid data
- `401 Unauthorized` - Missing/invalid API key
- `422 Unprocessable Entity` - Validation errors
- `500 Internal Server Error` - Server error

```json
{
  "status": "error",
  "error_code": "VALIDATION_ERROR",
  "message": "One or more fields failed validation",
  "errors": [
    {
      "field": "service_address.zip",
      "code": "outside_service_area",
      "message": "We don't currently service zip code 27601",
      "severity": "error"
    },
    {
      "field": "client.contact.phone",
      "code": "invalid_format",
      "message": "Phone number must be in valid format",
      "severity": "error"
    },
    {
      "field": "pets.0.weight_lbs",
      "code": "missing_recommended",
      "message": "Pet weight helps us provide better service",
      "severity": "warning"
    }
  ],
  "partial_save": true,
  "lead_id": "LEAD-2024-00456",
  "recovery_url": "https://app.example.com/leads/LEAD-2024-00456"
}
```

#### Error Codes

| Code | Description |
|------|-------------|
| `VALIDATION_ERROR` | One or more fields failed validation |
| `OUTSIDE_SERVICE_AREA` | Service address not in coverage area |
| `DUPLICATE_CLIENT` | Client already exists in system |
| `INVALID_PAYMENT` | Payment information invalid or declined |
| `MISSING_REQUIRED_FIELD` | Required field not provided |
| `INVALID_FORMAT` | Field format is invalid |
| `RATE_LIMIT_EXCEEDED` | Too many requests |

## Authentication

Implementations MUST support at least one of:

### API Key Authentication

```
X-API-Key: your_api_key_here
```

### OAuth 2.0

Standard OAuth 2.0 Bearer token:

```
Authorization: Bearer {access_token}
```

### Basic Authentication

For testing only (not recommended for production):

```
Authorization: Basic {base64_encoded_credentials}
```

## Validation Rules

### Required Field Validation

Implementations MUST validate that all required fields are present and non-null.

### Format Validation

- **Email:** RFC 5322 compliant
- **Phone:** Must be valid phone number (E.164 recommended)
- **URL:** RFC 3986 compliant
- **Dates:** ISO 8601 format
- **Coordinates:** Valid lat/long ranges

### Business Logic Validation

- Start date cannot be in the past (configurable tolerance)
- Pricing values must be positive numbers
- Required fields based on service_type
- Payment token required if method is "credit_card"

### Partial Save Support

If `partial_save: true` in error response, the system has saved what it could (typically as a lead). This allows:

- Capturing information even when outside service area
- Saving incomplete applications for follow-up
- Reducing data loss on validation failures

## Extensibility

### Custom Fields

Use the `metadata.custom_fields` object for implementation-specific data:

```json
{
  "metadata": {
    "custom_fields": {
      "internal_source_id": "WEB-001",
      "franchise_location": "raleigh_north",
      "territory": "zone_3"
    }
  }
}
```

### Vendor-Specific Extensions

Vendors MAY add root-level fields prefixed with `x_`:

```json
{
  "standard": "CODS",
  "version": "1.0",
  "x_vendor_data": {
    "internal_id": 12345,
    "routing_priority": "high"
  }
}
```

Implementations MUST ignore unknown fields gracefully.

## Versioning

### Version Format

Versions follow Semantic Versioning (SemVer):

- **Major:** Breaking changes (2.0, 3.0)
- **Minor:** New features, backward compatible (1.1, 1.2)
- **Patch:** Bug fixes (1.0.1, 1.0.2)

### Version Compatibility

- Implementations MUST support at least the current major version
- Implementations SHOULD support the previous major version
- Clients MUST specify version in `version` field
- Servers MUST validate version and reject unsupported versions

### Deprecation Policy

- Features deprecated in minor releases
- Removed in next major release
- Minimum 6 months notice for breaking changes
- Migration guide provided for major versions

## Compliance

### CODS Compliance Levels

**Level 1: Basic Compliance**
- Accepts CODS format
- Validates required fields
- Returns standard responses

**Level 2: Full Compliance**
- Level 1 +
- Supports all optional fields
- Proper error handling
- Idempotency support

**Level 3: Extended Compliance**
- Level 2 +
- Webhook notifications
- Bulk import support
- Advanced validation

### Testing

Reference test suite available at `/tests/` in this repository.

To claim CODS compliance, implementations should pass:
- All required field tests
- Format validation tests
- Error handling tests
- API specification tests

## Security Considerations

1. **PII Protection:** Encrypt data in transit (HTTPS required) and at rest
2. **Token Security:** Never expose payment tokens or credentials
3. **API Keys:** Rotate regularly, use per-client keys
4. **Rate Limiting:** Implement to prevent abuse
5. **Input Validation:** Sanitize all input to prevent injection
6. **Audit Logging:** Log all onboarding events

## Support

- **Documentation:** https://cods.dev
- **Issues:** https://github.com/yourusername/cods/issues
- **Discussions:** https://github.com/yourusername/cods/discussions
- **Email:** hello@maptheday.com

---

**Specification Version:** 1.0.0  
**Last Updated:** February 6, 2026  
**Status:** Stable  
**License:** MIT
