# API Contract — OpsPilot

## 1. Base URL

```txt
/api/v1
```

## 2. Standard Response

### Success

```json
{
  "success": true,
  "message": "Success",
  "data": {}
}
```

### Error

```json
{
  "success": false,
  "message": "Validation error",
  "errors": [
    {
      "field": "title",
      "message": "title is required"
    }
  ]
}
```

### Pagination

```json
{
  "success": true,
  "message": "Success",
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total_data": 100,
    "total_page": 10
  }
}
```

## 3. Auth

### POST `/auth/login`

Request:

```json
{
  "email": "support@opspilot.local",
  "password": "password"
}
```

Response:

```json
{
  "success": true,
  "message": "Login success",
  "data": {
    "access_token": "jwt-token",
    "token_type": "Bearer",
    "expires_in": 86400,
    "user": {
      "id": "uuid",
      "name": "RPA Support User",
      "email": "support@opspilot.local",
      "role": "SUPPORT"
    }
  }
}
```

### GET `/auth/me`

Response:

```json
{
  "success": true,
  "message": "Success",
  "data": {
    "id": "uuid",
    "name": "RPA Support User",
    "email": "support@opspilot.local",
    "role": "SUPPORT"
  }
}
```

### POST `/auth/logout`

Response:

```json
{
  "success": true,
  "message": "Logout success",
  "data": null
}
```

## 4. Tickets

### Ticket Object

```json
{
  "id": "uuid",
  "ticket_no": "OPS-2026-000001",
  "title": "Office 365 login failed",
  "description": "Robot cannot login due to firewall whitelist issue.",
  "type": "ISSUE",
  "priority": "HIGH",
  "status": "OPEN",
  "reporter": {
    "id": "uuid",
    "name": "Business User"
  },
  "assignee": {
    "id": "uuid",
    "name": "RPA Support User"
  },
  "robot": {
    "id": "uuid",
    "name": "POD Payment Regress"
  },
  "application": {
    "id": "uuid",
    "name": "Office 365"
  },
  "sla_due_at": "2026-06-20T10:00:00Z",
  "root_cause": null,
  "resolution": null,
  "created_at": "2026-06-19T10:00:00Z",
  "updated_at": "2026-06-19T10:00:00Z"
}
```

### GET `/tickets`

Query params:

| Param | Type | Notes |
|---|---|---|
| page | number | Default 1 |
| limit | number | Default 10 |
| search | string | Search ticket_no/title |
| status | string | OPEN, IN_PROGRESS, etc |
| type | string | ISSUE, REQUEST, etc |
| priority | string | LOW, MEDIUM, HIGH, CRITICAL |
| assignee_id | string | Optional |
| robot_id | string | Optional |
| application_id | string | Optional |
| start_date | date | Optional |
| end_date | date | Optional |
| sort | string | Default latest update |

### POST `/tickets`

Request:

```json
{
  "title": "Office 365 login failed",
  "description": "Robot cannot login to Office 365.",
  "type": "ISSUE",
  "priority": "HIGH",
  "category_id": "uuid",
  "robot_id": "uuid",
  "application_id": "uuid"
}
```

### GET `/tickets/{id}`

Returns ticket detail including comments and activities.

### PUT `/tickets/{id}`

Request:

```json
{
  "title": "Updated title",
  "description": "Updated description",
  "type": "ISSUE",
  "priority": "HIGH",
  "category_id": "uuid",
  "robot_id": "uuid",
  "application_id": "uuid",
  "root_cause": "Firewall domain not whitelisted",
  "resolution": "Whitelist required Microsoft domain"
}
```

### PATCH `/tickets/{id}/status`

Request:

```json
{
  "status": "IN_PROGRESS"
}
```

Business rule:

- Create `STATUS_CHANGED` activity.
- Set `started_at` when status changes to `IN_PROGRESS` for the first time.
- Set `resolved_at` when status changes to `RESOLVED`.
- Set `closed_at` when status changes to `CLOSED`.

### PATCH `/tickets/{id}/assign`

Request:

```json
{
  "assignee_id": "uuid"
}
```

Business rule:

- Create `ASSIGNED` activity.

### POST `/tickets/{id}/comments`

Request:

```json
{
  "message": "Escalated to network team for firewall checking.",
  "is_internal": true
}
```

Business rule:

- Create comment.
- Create `COMMENT` activity.

### GET `/tickets/{id}/activities`

Returns timeline activities.

### POST `/tickets/{id}/escalations`

Request:

```json
{
  "target_type": "NETWORK",
  "target_name": "Network Team",
  "reason": "Need firewall whitelist for Office 365 login domain"
}
```

Business rule:

- Create escalation.
- Create `ESCALATED` activity.

## 5. Dashboard

### GET `/dashboard/summary`

Response:

```json
{
  "success": true,
  "message": "Success",
  "data": {
    "total_tickets_today": 4,
    "total_open_tickets": 12,
    "total_in_progress_tickets": 5,
    "total_waiting_user_tickets": 3,
    "resolved_this_month": 21,
    "sla_breached_tickets": 2,
    "robot_success_rate": 96.5,
    "failed_robot_runs_today": 1,
    "license_expiring_soon": 2
  }
}
```

### GET `/dashboard/ticket-chart`

Query params:

- `start_date`
- `end_date`
- `group_by`: `status`, `priority`, `type`, `category`, `month`

### GET `/dashboard/robot-chart`

Query params:

- `start_date`
- `end_date`

## 6. Robots

### GET `/robots`

Supports pagination and search.

### POST `/robots`

Request:

```json
{
  "name": "POD Payment Regress",
  "code": "POD_PAYMENT_REGRESS",
  "description": "POD payment regression process",
  "environment": "PROD",
  "status": "ACTIVE",
  "server_name": "RPA-PROD-01",
  "server_ip": "172.18.55.113",
  "schedule_info": "Daily 08:00"
}
```

### GET `/robots/{id}`

Returns robot detail.

### PUT `/robots/{id}`

Updates robot master data.

### DELETE `/robots/{id}`

Soft delete robot if possible.

### GET `/robots/{id}/runs`

Returns run history.

### POST `/robots/{id}/runs`

Request:

```json
{
  "run_no": "RUN-2026-000001",
  "status": "FAILED",
  "started_at": "2026-06-19T08:00:00Z",
  "finished_at": "2026-06-19T08:10:00Z",
  "total_transaction": 100,
  "success_transaction": 80,
  "failed_transaction": 20,
  "error_message": "Login failed",
  "log_summary": "Office 365 login failed due to firewall",
  "related_ticket_id": "uuid"
}
```

## 7. Assets

### GET `/assets`

Supports pagination, search, type, status, and expiring filter.

### POST `/assets`

Request:

```json
{
  "name": "Office 365 Robot Account",
  "type": "OFFICE_LICENSE",
  "owner": "RPA Team",
  "vendor": "Microsoft",
  "related_robot_id": "uuid",
  "start_date": "2026-01-01",
  "expired_date": "2026-12-31",
  "reminder_days": 30,
  "status": "ACTIVE",
  "notes": "Used for robot email login"
}
```

### GET `/assets/expiring-soon`

Returns assets where `expired_date <= today + reminder_days`.

## 8. Reports

### GET `/reports/daily`

Query params:

- `date`

### GET `/reports/monthly`

Query params:

- `year`
- `month`

### GET `/reports/yearly`

Query params:

- `year`

### GET `/reports/export/csv`

Query params:

- `report_type`
- `start_date`
- `end_date`

## 9. Knowledge Base

### GET `/knowledge-base`

Supports search and tag filter.

### POST `/knowledge-base`

Request:

```json
{
  "title": "Office 365 login failed due to firewall",
  "issue_pattern": "Robot cannot login to Office 365",
  "root_cause": "Required Microsoft domain not whitelisted",
  "solution": "Request firewall whitelist to network team",
  "workaround": "Manual process by user while waiting whitelist",
  "related_application_id": "uuid",
  "related_robot_id": "uuid",
  "tags": ["office365", "firewall", "login"]
}
```

## 10. Enum Reference

### Roles

- ADMIN
- SUPPORT
- REQUESTER
- MANAGER

### Ticket Type

- ISSUE
- REQUEST
- ENHANCEMENT
- CHANGE_REQUEST
- INCIDENT
- SUPPORT
- LICENSE

### Ticket Status

- OPEN
- IN_PROGRESS
- WAITING_USER
- WAITING_INTERNAL
- WAITING_VENDOR
- RESOLVED
- CLOSED
- CANCELLED

### Priority

- LOW
- MEDIUM
- HIGH
- CRITICAL

### Robot Environment

- DEV
- UAT
- PROD

### Robot Status

- ACTIVE
- INACTIVE
- MAINTENANCE

### Robot Run Status

- RUNNING
- SUCCESS
- FAILED
- PARTIAL_SUCCESS
- CANCELLED
- SKIPPED

### Asset Type

- RPA_LICENSE
- OFFICE_LICENSE
- SERVER
- BOT_ACCOUNT
- VPN
- CERTIFICATE
- SCHEDULER
- APPLICATION_ACCESS

### Asset Status

- ACTIVE
- EXPIRING_SOON
- EXPIRED
- INACTIVE
