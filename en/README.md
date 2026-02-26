# API Documentation

API documentation for the Power Profit Optimizer system. Use these docs to integrate with site management, device management, power data, and max profit optimization.

## Document Index

| File | Module | Description |
|------|--------|-------------|
| [site.md](site.md) | Site Management | CRUD for sites |
| [device.md](device.md) | Device Management | CRUD for devices |
| [power.md](power.md) | Power Data | Power records, max profit optimization |

---

## Base URL

Paths in the docs are relative (e.g. `/v1/devices/`). Prepend your service base URL, for example:

```
https://your-domain.com/api/v1/devices/
```

The `/v1/xxx` in docs maps to `/api/v1/xxx` in the actual URL.

---

## Request and Response

- **Request**: GET uses query parameters; POST/PUT use JSON body; path placeholders like `:id` must be replaced with actual values.
- **Response**: Success returns JSON; errors also return JSON (see Error Response below).

---

## Pagination

List endpoints (e.g. device list, power records) return paginated data with these fields:

| Field | Type | Description |
|-------|------|-------------|
| items | array | Data list for current page |
| total | int | Total record count |
| page | int | Current page (1-based) |
| page_size | int | Records per page |
| total_pages | int | Total page count |

Use `page` and `page_size` in the request, e.g. `?page=1&page_size=10`.

---

## Time Fields

| Field | Format | Example |
|-------|--------|---------|
| timestamp, start_time, end_time, etc. | int, Unix timestamp (seconds) | `1736143200` |
| created_at, updated_at | string, ISO 8601 datetime | `"2025-01-06T10:00:00"` |

All time-related query parameters use Unix timestamps (seconds).

---

## Error Response

On failure, the API returns:

```json
{
  "code": 20001,
  "message": "Device not found"
}
```

| Field | Description |
|-------|-------------|
| code | Error code for programmatic handling |
| message | Human-readable error message |

**Error code ranges**:

| Range | Meaning |
|-------|---------|
| 10000-19999 | Parameter or request format error |
| 20000-29999 | Resource not found (e.g. device, site) |
| 30000-39999 | Business logic error (e.g. insufficient data, type mismatch) |
| 50000-59999 | Server internal error |

Each endpoint's error table lists the codes it may return.
