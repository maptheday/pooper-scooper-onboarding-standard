# CODS Certification

Get your software officially certified as CODS-compatible and listed in our directory.

## Why Get Certified?

✅ **Marketing Badge** - Display "CODS Certified" on your website  
✅ **Directory Listing** - Featured on cods.dev  
✅ **Technical Credibility** - Proven interoperability  
✅ **Community Recognition** - Early adopter status  
✅ **Integration Priority** - First to get new integrations  

## Certification Levels

### Level 1: Basic Compliance

**Requirements:**
- ✅ Accept CODS-formatted data at standard endpoint
- ✅ Validate required fields
- ✅ Return standard success/error responses
- ✅ Pass basic test suite (10 tests)

**Badge:** CODS Level 1 Certified  
**Time to Complete:** 4-8 hours

### Level 2: Full Compliance

**Requirements:**
- ✅ Everything in Level 1
- ✅ Support all optional CODS fields
- ✅ Comprehensive error handling
- ✅ Partial save for leads
- ✅ Idempotency support
- ✅ Pass full test suite (30 tests)

**Badge:** CODS Certified  
**Time to Complete:** 1-2 days

### Level 3: Extended Compliance

**Requirements:**
- ✅ Everything in Level 2
- ✅ Export client data as CODS format
- ✅ Webhook notifications
- ✅ Bulk import support
- ✅ Advanced validation
- ✅ Pass extended test suite (50 tests)

**Badge:** CODS Extended Certified  
**Time to Complete:** 2-3 days

## Certification Process

### Step 1: Self-Assessment

Test your implementation against the specification:

```bash
# Install CODS validator
npm install -g @cods/validator

# Test your endpoint
cods-test --endpoint https://yourapp.com/api/v1/cods/onboarding \
          --api-key your_test_key \
          --level 1
```

### Step 2: Documentation

Provide:

1. **API Documentation**
   - Endpoint URL
   - Authentication method
   - Any special requirements

2. **Implementation Details**
   - Which fields you support
   - Custom fields you've added
   - Known limitations

3. **Test Credentials**
   - Test API key
   - Test environment URL

### Step 3: Automated Testing

We'll run our test suite against your endpoint:

- Valid onboarding (complete data)
- Valid onboarding (minimal data)
- Missing required fields
- Invalid data types
- Outside service area
- Duplicate client
- Idempotency test

### Step 4: Manual Review

A CODS maintainer will:

1. Review your documentation
2. Test edge cases
3. Verify error handling
4. Check response format
5. Validate security practices

### Step 5: Certification Issued

Once approved:

1. You receive certification badge files
2. Listed on cods.dev
3. Listed in GitHub README
4. Announced in community

## Test Suite

### Basic Tests (Level 1)

```yaml
tests:
  - name: "Valid complete onboarding"
    file: examples/complete-onboarding.json
    expect: 201
    
  - name: "Valid minimal onboarding"
    file: examples/minimal-onboarding.json
    expect: 201
    
  - name: "Missing required field - email"
    expect: 422
    error_code: "MISSING_REQUIRED_FIELD"
    
  - name: "Invalid email format"
    expect: 422
    error_code: "INVALID_FORMAT"
    
  - name: "Invalid CODS version"
    expect: 400
    error_code: "UNSUPPORTED_VERSION"
```

### Full Tests (Level 2)

All basic tests plus:

```yaml
  - name: "Outside service area partial save"
    expect: 422
    require:
      - partial_save: true
      - lead_id
      
  - name: "Duplicate client detection"
    expect: 409
    error_code: "DUPLICATE_CLIENT"
    
  - name: "Idempotency check"
    headers:
      X-Idempotency-Key: "test-123"
    repeat: 2
    expect: Same response both times
```

### Extended Tests (Level 3)

All previous tests plus:

```yaml
  - name: "Export client as CODS"
    endpoint: GET /api/v1/cods/clients/{id}
    expect: Valid CODS format
    
  - name: "Bulk import"
    file: examples/bulk-onboarding.json
    expect: Array of results
    
  - name: "Webhook delivery"
    expect: Webhook sent within 60 seconds
```

## Testing Your Implementation

### Manual Testing

Use cURL to test your endpoint:

```bash
curl -X POST https://yourapp.com/api/v1/cods/onboarding \
  -H "Content-Type: application/json" \
  -H "X-API-Key: your_key" \
  -H "X-CODS-Version: 1.0" \
  -d @examples/complete-onboarding.json
```

Expected response:

```json
{
  "status": "success",
  "client_id": "CLT-12345",
  "onboarding_id": "OB-2024-00123",
  "created_at": "2024-02-06T10:30:00Z"
}
```

### Automated Testing

Use our test script:

```python
# test_cods.py
import requests
import json

def test_onboarding(endpoint, api_key):
    with open('examples/complete-onboarding.json') as f:
        data = json.load(f)
    
    response = requests.post(
        endpoint,
        json=data,
        headers={
            'X-API-Key': api_key,
            'X-CODS-Version': '1.0'
        }
    )
    
    assert response.status_code == 201
    assert response.json()['status'] == 'success'
    print("✅ Basic onboarding test passed")

if __name__ == '__main__':
    test_onboarding(
        'https://yourapp.com/api/v1/cods/onboarding',
        'your_test_key'
    )
```

## Common Certification Issues

### Issue: Missing Required Fields in Response

**Problem:** Success response doesn't include all required fields

**Solution:** Ensure your success response includes:
- `status`
- `client_id`
- `created_at`

### Issue: Incorrect Error Format

**Problem:** Errors don't match CODS specification

**Solution:** Use standardized error format:
```json
{
  "status": "error",
  "error_code": "VALIDATION_ERROR",
  "errors": [
    {
      "field": "client.contact.email",
      "code": "INVALID_FORMAT",
      "message": "Invalid email address"
    }
  ]
}
```

### Issue: Not Handling Unknown Fields

**Problem:** Endpoint rejects valid CODS data with extra fields

**Solution:** Ignore unknown fields gracefully. Don't reject data just because it has fields you don't use.

### Issue: Validation Too Strict

**Problem:** Rejecting valid optional fields

**Solution:** Only validate:
- Required fields exist
- Field types are correct
- Values are in valid ranges

Don't reject data for missing optional fields.

## Certification Checklist

Before applying for certification:

### Technical Requirements

- [ ] Endpoint at `/api/v1/cods/onboarding` or documented alternative
- [ ] Accepts POST requests with JSON body
- [ ] Returns proper HTTP status codes
- [ ] Validates required fields
- [ ] Returns CODS-compliant responses
- [ ] Handles errors gracefully
- [ ] Supports HTTPS (required for production)
- [ ] API key authentication working

### Documentation Requirements

- [ ] API documentation published
- [ ] List of supported fields documented
- [ ] Custom fields documented (if any)
- [ ] Authentication method documented
- [ ] Error codes documented
- [ ] Example requests/responses provided

### Testing Requirements

- [ ] Test endpoint available
- [ ] Test API key provided
- [ ] All automated tests passing
- [ ] Manual testing completed
- [ ] Edge cases handled

## Applying for Certification

1. **Fill out the form:** https://cods.dev/certify
2. **Provide test credentials**
3. **Submit documentation**
4. **Wait for review** (usually 3-5 business days)
5. **Address any feedback**
6. **Receive certification**

Or email hello@maptheday.com with:

```
Subject: CODS Certification Request - [Your Company]

Company: Your Company Name
Software: Your Software Name
Contact Email: your@email.com
Certification Level: 1, 2, or 3

Test Endpoint: https://...
Test API Key: ...

Documentation URL: https://...

Additional Notes: ...
```

## Maintaining Certification

### Annual Recertification

Certifications are valid for 12 months. To maintain:

1. Test against new CODS versions as released
2. Update if spec changes significantly
3. Re-test annually

### Version Compatibility

When new CODS versions are released:

- **Minor versions (1.1, 1.2):** You stay certified, new features optional
- **Major versions (2.0):** You have 6 months to upgrade or lose certification

### Revocation

Certification can be revoked if:

- Implementation breaks specification
- Security vulnerabilities not addressed
- Misleading claims about CODS support
- No longer maintaining endpoint

## Marketing Your Certification

Once certified, you can:

### Use the Badge

Download badge files:
- PNG (for websites)
- SVG (scalable)
- PDF (for print)

### Update Your Site

```html
<img src="cods-certified-badge.svg" alt="CODS Certified" />
<p>Fully compatible with the Client Onboarding Data Standard</p>
```

### Announce It

- Blog post
- Press release
- Social media
- Email to customers
- Sales materials

### Sample Language

> "[Your Software] is officially CODS-certified, ensuring seamless data exchange with marketing agencies, lead generation services, and other CODS-compatible tools. Import client data with zero manual entry."

## Questions?

- **Technical questions:** hello@maptheday.com
- **Certification status:** hello@maptheday.com
- **Badge files:** Download from cods.dev after certification

---

Ready to get certified? [Apply now](https://cods.dev/certify) or email hello@maptheday.com
