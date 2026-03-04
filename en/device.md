[[_TOC_]]

# Device Management

## POST /v1/devices/

- Description: Create a device

- Request

  - Body params

    - Data type: json

      | Param | Type | Required | Description | Range | Example |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | name | string | yes | Device name | - | "PV Device 1" |
      | type | string | yes | Device type | PV/Load/BESS | "PV" |
      | site_id | int | yes | Site ID | (0, +∞) | 1 |
      | total_capacity | float | no | Total capacity (kWh) | (0, +∞) | 100.0 |
      | max_power | float | no | Max power (kW) | (0, +∞) | 50.0 |
      | energy_conversion_efficiency | float | no | Conversion efficiency (%) | (0, 100] | 85.5 |
      | max_soc | float | no | Max SOC (%) | (0, 100] | 90.0 |
      | min_soc | float | no | Min SOC (%) | [0, 100] | 10.0 |
      | panel_tilt | float | no | Panel tilt (°): horizontal=0, vertical=90 (PV) | [0, 90] | 30.0 |
      | panel_azimuth | float | no | Panel azimuth (°), GIS: N=0, E=90, S=180, W=270 (PV) | [0, 360) | 180.0 |

      Optional fields by type:
      - **PV**: required suggested: `total_capacity`, `max_power`; optional: `panel_tilt`, `panel_azimuth`
      - **BESS**: required suggested: `total_capacity`, `max_power`, `energy_conversion_efficiency`, `min_soc`, `max_soc`
      - **Load**: no extra required

    - Example

      ```json
      {
        "name": "PV Device 1",
        "type": "PV",
        "site_id": 1,
        "total_capacity": 100.0,
        "max_power": 50.0,
        "panel_tilt": 30.0,
        "panel_azimuth": 180.0
      }
      ```

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | yes | Device ID | 1 |
    | name | string | yes | Device name | "PV Device 1" |
    | type | string | yes | Device type | "PV" |
    | site_id | int | yes | Site ID | 1 |
    | total_capacity | float | no | Total capacity (kWh) | 100.0 |
    | max_power | float | no | Max power (kW) | 50.0 |
    | energy_conversion_efficiency | float | no | Conversion efficiency (%) | 85.5 |
    | max_soc | float | no | Max SOC (%) | 90.0 |
    | min_soc | float | no | Min SOC (%) | 10.0 |
    | panel_tilt | float | no | Panel tilt (°) | 30.0 |
    | panel_azimuth | float | no | Panel azimuth (°), GIS convention | 180.0 |
    | created_at | datetime | yes | Created at | "2025-01-06T10:00:00" |
    | updated_at | datetime | no | Updated at | "2025-01-06T10:00:00" |

  - Example

    ```json
    {
      "id": 1,
      "name": "PV Device 1",
      "type": "PV",
      "site_id": 1,
      "total_capacity": 100.0,
      "max_power": 50.0,
      "energy_conversion_efficiency": null,
      "max_soc": null,
      "min_soc": null,
      "panel_tilt": 30.0,
      "panel_azimuth": 180.0,
      "created_at": "2025-01-06T10:00:00",
      "updated_at": null
    }
    ```

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 400 | 10001 | A device with the same name already exists |
  | 404 | 20002 | Site not found |
  | 500 | 50001 | Internal server error |

## GET /v1/devices/

- Description: List devices

- Request

  - Query params

    | Param | Type | Required | Description | Range | Example |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | page | int | no | Page number | [1, +∞) | 1 |
    | page_size | int | no | Page size | [1, 500] | 100 |
    | site_id | int | no | Filter by site ID | (0, +∞) | 1 |
    | device_type | string | no | Filter by type | PV/Load/BESS | "PV" |

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | yes | Device list | - |
    | total | int | yes | Total count | 100 |
    | page | int | yes | Current page | 1 |
    | page_size | int | yes | Page size | 100 |
    | total_pages | int | yes | Total pages | 1 |

    Each item in items:

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | yes | Device ID | 1 |
    | name | string | yes | Device name | "PV Device 1" |
    | type | string | yes | Device type | "PV" |
    | site_id | int | yes | Site ID | 1 |
    | total_capacity | float | no | Total capacity (kWh) | 100.0 |
    | max_power | float | no | Max power (kW) | 50.0 |
    | energy_conversion_efficiency | float | no | Conversion efficiency (%) | 85.5 |
    | max_soc | float | no | Max SOC (%) | 90.0 |
    | min_soc | float | no | Min SOC (%) | 10.0 |
    | panel_tilt | float | no | Panel tilt (°) | 30.0 |
    | panel_azimuth | float | no | Panel azimuth (°) | 180.0 |
    | created_at | datetime | yes | Created at | "2025-01-06T10:00:00" |
    | updated_at | datetime | no | Updated at | "2025-01-06T10:00:00" |

  - Example

    ```json
    {
      "items": [
        {
          "id": 1,
          "name": "PV Device 1",
          "type": "PV",
          "site_id": 1,
          "total_capacity": 100.0,
          "max_power": 50.0,
          "energy_conversion_efficiency": null,
          "max_soc": null,
          "min_soc": null,
          "panel_tilt": 30.0,
          "panel_azimuth": 180.0,
          "created_at": "2025-01-06T10:00:00",
          "updated_at": null
        }
      ],
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

## GET /v1/devices/:device_id

- Description: Get device by ID

- Request

  - Path params

    | Param | Type | Required | Description | Range | Example |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | device_id | int | yes | Device ID | (0, +∞) | 1 |

- Response

  - Data type: json (same structure as POST response)

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 404 | 20001 | Device not found |
  | 500 | 50001 | Internal server error |

## PUT /v1/devices/:device_id

- Description: Update device

- Request

  - Path params: device_id
  - Body params: partial fields (name, type, site_id, total_capacity, max_power, energy_conversion_efficiency, max_soc, min_soc, panel_tilt, panel_azimuth)

- Response

  - Data type: json (full device object)

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 400 | 10001 | A device with the same name already exists |
  | 404 | 20001 | Device not found |
  | 500 | 50001 | Internal server error |

## DELETE /v1/devices/:device_id

- Description: Delete device

- Request

  - Path params: device_id

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | message | string | yes | Message | "Device deleted successfully" |

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 404 | 20001 | Device not found |
  | 500 | 50001 | Internal server error |
