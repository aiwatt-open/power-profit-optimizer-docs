[[_TOC_]]

# Site Management

## GET /v1/sites/

- Description: List sites

- Request

  - Query params

    | Param | Type | Required | Description | Range | Example |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | page | int | no | Page number | [1, +∞) | 1 |
    | page_size | int | no | Page size | [1, 500] | 100 |

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | yes | Site list | - |
    | total | int | yes | Total count | 100 |
    | page | int | yes | Current page | 1 |
    | page_size | int | yes | Page size | 100 |
    | total_pages | int | yes | Total pages | 1 |

    Each item: id, name, latitude, longitude, country, city, address, description, timezone, created_at, updated_at

  - Example

    ```json
    {
      "items": [{"id": 1, "name": "Site 1", "latitude": 39.9042, "longitude": 116.4074, "country": "China", "city": "Beijing", "address": "Chaoyang District", "description": "Test site", "timezone": "Asia/Shanghai", "created_at": "2025-01-06T10:00:00", "updated_at": "2025-01-06T10:00:00"}],
      "total": 100,
      "page": 1,
      "page_size": 100,
      "total_pages": 1
    }
    ```

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 500 | 50001 | Internal server error |

## POST /v1/sites/

- Description: Create site

- Request

  - Body params: name, latitude, longitude, country, city, address (optional), description (optional), timezone (IANA)

- Response: Full site object with id, created_at, updated_at

- Error codes: 400 (10001 Invalid parameter), 500 (50001)

## GET /v1/sites/:site_id

- Description: Get site by ID

- Request: Path param site_id (int)

- Response: Full site object

- Error codes: 404 (20002 Site not found), 500 (50001)

## PUT /v1/sites/:site_id

- Description: Update site

- Request: Path param site_id, body with partial fields (name, latitude, longitude, country, city, address, description, timezone)

- Response: Full site object

- Error codes: 400 (10001), 404 (20002), 500 (50001)

## DELETE /v1/sites/:site_id

- Description: Delete site

- Request: Path param site_id

- Response: { "message": "Site deleted successfully" }

- Error codes: 404 (20002), 500 (50001)
