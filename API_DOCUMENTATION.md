# Voc2Go API Documentation

## Overview
The Voc2Go API provides RESTful endpoints for content management, team social media integration, and administrative functions. All endpoints return JSON responses and use standard HTTP status codes.

## Base URL
```
Development: http://localhost:5000
Production: https://your-domain.replit.app
```

## Authentication
Admin endpoints require JWT authentication. Include the token in the Authorization header:
```http
Authorization: Bearer <jwt_token>
```

## Content Management API

### Get All Content
Retrieve all content sections for the application.

**Endpoint:** `GET /api/content`

**Response:**
```json
{
  "sections": [
    {
      "id": 1,
      "sectionId": "hero-title",
      "contentEn": "Welcome to Voc2Go",
      "contentHu": "Üdvözöljük a Voc2Go-ban",
      "pageId": "home",
      "createdAt": "2025-08-09T08:15:28.655Z",
      "updatedAt": "2025-08-09T08:15:28.655Z"
    }
  ]
}
```

**Status Codes:**
- `200` - Success
- `500` - Internal server error

### Update Content Section
Update a specific content section (Admin only).

**Endpoint:** `PUT /api/content/:id`

**Headers:**
```http
Authorization: Bearer <admin_jwt_token>
Content-Type: application/json
```

**Request Body:**
```json
{
  "contentEn": "Updated English content",
  "contentHu": "Frissített magyar tartalom",
  "pageId": "home"
}
```

**Response:**
```json
{
  "section": {
    "id": 1,
    "sectionId": "hero-title",
    "contentEn": "Updated English content",
    "contentHu": "Frissített magyar tartalom",
    "pageId": "home",
    "updatedAt": "2025-08-09T09:30:15.123Z"
  }
}
```

**Status Codes:**
- `200` - Success
- `400` - Invalid request data
- `401` - Unauthorized
- `404` - Content section not found
- `500` - Internal server error

## Team Social Media API

### Get Team Member Social Links
Retrieve social media links for a specific team member.

**Endpoint:** `GET /api/team-socials/:memberId`

**Parameters:**
- `memberId` (string) - Team member identifier (e.g., "member-1")

**Response:**
```json
{
  "socials": {
    "id": 1,
    "memberId": "member-1",
    "instagram": "https://www.instagram.com/voc2go24/",
    "facebook": "https://www.facebook.com/voc2go",
    "linkedin": "https://linkedin.com/in/kata-marjai",
    "behance": null,
    "kofi": null,
    "pinterest": null,
    "twitter": null,
    "website": "https://www.voc2go.com",
    "createdAt": "2025-08-09T08:15:28.655Z",
    "updatedAt": "2025-08-09T09:49:59.078Z"
  }
}
```

**Response when no links found:**
```json
{
  "socials": null
}
```

**Status Codes:**
- `200` - Success
- `400` - Missing member ID
- `500` - Internal server error

### Update Team Member Social Links
Update or create social media links for a team member (Admin only).

**Endpoint:** `PUT /api/team-socials/:memberId`

**Headers:**
```http
Authorization: Bearer <admin_jwt_token>
Content-Type: application/json
```

**Request Body:**
```json
{
  "instagram": "https://www.instagram.com/username",
  "facebook": "https://www.facebook.com/username",
  "linkedin": "https://linkedin.com/in/username",
  "behance": "https://www.behance.net/username",
  "kofi": "https://ko-fi.com/username",
  "pinterest": "https://pinterest.com/username",
  "twitter": "https://x.com/username",
  "website": "https://yourwebsite.com"
}
```

**Response:**
```json
{
  "socials": {
    "id": 1,
    "memberId": "member-1",
    "instagram": "https://www.instagram.com/username",
    "facebook": "https://www.facebook.com/username",
    "linkedin": "https://linkedin.com/in/username",
    "behance": "https://www.behance.net/username",
    "kofi": "https://ko-fi.com/username",
    "pinterest": "https://pinterest.com/username",
    "twitter": "https://x.com/username",
    "website": "https://yourwebsite.com",
    "createdAt": "2025-08-09T08:15:28.655Z",
    "updatedAt": "2025-08-09T10:30:45.123Z"
  }
}
```

**Status Codes:**
- `200` - Success
- `400` - Invalid request data or missing member ID
- `401` - Unauthorized
- `500` - Internal server error

### Get All Team Social Links
Retrieve social media links for all team members (Admin only).

**Endpoint:** `GET /api/team-socials`

**Headers:**
```http
Authorization: Bearer <admin_jwt_token>
```

**Response:**
```json
{
  "socials": [
    {
      "id": 1,
      "memberId": "member-1",
      "instagram": "https://www.instagram.com/voc2go24/",
      "facebook": "https://www.facebook.com/voc2go",
      "linkedin": "https://linkedin.com/in/kata-marjai",
      "website": "https://www.voc2go.com",
      "createdAt": "2025-08-09T08:15:28.655Z",
      "updatedAt": "2025-08-09T09:49:59.078Z"
    }
  ]
}
```

**Status Codes:**
- `200` - Success
- `401` - Unauthorized
- `500` - Internal server error

## Admin Authentication API

### Admin Login
Authenticate admin user and receive JWT token.

**Endpoint:** `POST /api/admin/login`

**Request Body:**
```json
{
  "loginName": "admin_username",
  "password": "admin_password"
}
```

**Response:**
```json
{
  "success": true,
  "admin": {
    "id": 2,
    "loginName": "admin_username",
    "lastLogin": "2025-08-09T09:43:02.123Z"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Error Response:**
```json
{
  "error": "Invalid credentials"
}
```

**Status Codes:**
- `200` - Success
- `401` - Invalid credentials
- `400` - Missing credentials
- `500` - Internal server error

### Get Current Admin User
Get information about the currently authenticated admin.

**Endpoint:** `GET /api/admin/me`

**Headers:**
```http
Authorization: Bearer <admin_jwt_token>
```

**Response:**
```json
{
  "admin": {
    "id": 2,
    "loginName": "admin_username",
    "lastLogin": "2025-08-09T09:43:02.123Z",
    "createdAt": "2025-08-01T12:00:00.000Z"
  }
}
```

**Status Codes:**
- `200` - Success
- `401` - Unauthorized or invalid token
- `500` - Internal server error

## Error Handling

### Standard Error Response Format
```json
{
  "error": "Error message description",
  "details": "Additional error details (optional)"
}
```

### HTTP Status Codes
- `200` - OK: Request successful
- `400` - Bad Request: Invalid request data
- `401` - Unauthorized: Authentication required or failed
- `403` - Forbidden: Insufficient permissions
- `404` - Not Found: Resource not found
- `500` - Internal Server Error: Server-side error

### Common Error Scenarios

#### Authentication Errors
```json
{
  "error": "Unauthorized access. Please login."
}
```

#### Validation Errors
```json
{
  "error": "Invalid social media data",
  "details": [
    {
      "field": "instagram",
      "message": "Must be a valid URL"
    }
  ]
}
```

#### Server Errors
```json
{
  "error": "Internal server error"
}
```

## Rate Limiting
Currently, no rate limiting is implemented. For production use, consider implementing rate limiting based on IP address or user authentication.

## CORS Configuration
The API supports cross-origin requests from the frontend application. CORS is configured to accept requests from the same domain.

## Data Validation

### Social Media Links
- All social media fields are optional
- URLs are automatically prefixed with `https://` if no protocol is provided
- Empty strings are treated as null values
- Maximum length: 500 characters per field

### Content Sections
- `contentEn` and `contentHu` support HTML content
- `pageId` is limited to 100 characters
- `sectionId` is required and limited to 255 characters

## Security Considerations

### JWT Tokens
- Tokens expire after 24 hours
- Tokens should be stored securely (httpOnly cookies recommended)
- Include tokens in Authorization header for protected routes

### Input Sanitization
- All input data is validated using Zod schemas
- HTML content is not sanitized server-side (handle client-side if needed)
- SQL injection protection via parameterized queries

### Password Security
- Passwords are hashed using bcrypt
- Minimum password requirements should be enforced client-side

## Development & Testing

### Testing Endpoints
Use tools like curl, Postman, or Insomnia to test API endpoints:

```bash
# Get content
curl http://localhost:5000/api/content

# Login (get token)
curl -X POST http://localhost:5000/api/admin/login \
  -H "Content-Type: application/json" \
  -d '{"loginName":"admin","password":"password"}'

# Update social links (with token)
curl -X PUT http://localhost:5000/api/team-socials/member-1 \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"instagram":"https://instagram.com/newhandle"}'
```

### Database Schema
The API interacts with PostgreSQL database tables:
- `content_sections` - Application content
- `team_member_socials` - Team social media links  
- `admin_users` - Administrator accounts

Refer to `shared/schema.ts` for complete database schema definitions.