[[_TOC_]]

# Power Data

## POST /v1/power/records

- Description: Create a single power record

- Request

  - Body params

    - Data type: json

      | Param | Type | Required | Description | Range | Example |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | device_id | int | yes | Device ID | (0, +∞) | 1 |
      | timestamp | int | yes | Unix timestamp (seconds) | - | 1736143200 |
      | value | float | yes | Power (kW) | [0, +∞) | 50.5 |
      | abnormal | bool | no | true=abnormal, false=normal | - | false |

  - Example

    ```json
    {
      "device_id": 1,
      "timestamp": 1736143200,
      "value": 50.5,
      "abnormal": false
    }
    ```

- Response

  - Data type: json (record with id, device_id, timestamp, value, abnormal, created_at, updated_at)

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 404 | 20001 | Device not found |
  | 500 | 50001 | Failed to create power record |

## POST /v1/power/records/batch

- Description: Create power records in batch

- Request

  - Body params

    - Data type: json

      | Param | Type | Required | Description | Range | Example |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | device_id | int | yes | Device ID | (0, +∞) | 1 |
      | data | array | yes | List of records | - | - |

      Each record: timestamp (int), value (float), abnormal (bool, optional)

  - Example

    ```json
    {
      "device_id": 1,
      "data": [
        {
          "timestamp": 1736143200,
          "value": 50.5,
          "abnormal": false
        }
      ]
    }
    ```

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | inserted_count | int | yes | Number of inserted records | 100 |

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 404 | 20001 | Device not found |
  | 500 | 50001 | Failed to create power records in batch |

## GET /v1/power/records

- Description: Get power records for a device

- Request

  - Query params

    | Param | Type | Required | Description | Range | Example |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | device_id | int | yes | Device ID | (0, +∞) | 1 |
    | start_time | int | no | Start time (Unix seconds) | - | 1736143200 |
    | end_time | int | no | End time (Unix seconds) | - | 1736229599 |
    | page | int | no | Page number | [1, +∞) | 1 |
    | page_size | int | no | Page size | [1, 500] | 10 |

- Response

  - Data type: json (paginated: items, total, page, page_size, total_pages)

  - Each item: id, device_id, timestamp, value, abnormal, created_at, updated_at

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 404 | 20001 | Device not found or empty result |
  | 400 | 32501 | No power records |
  | 500 | 50001 | Internal server error |

## POST /v1/power/max_profit

- Description: Max profit optimization

- Request

  - Query params

    | Param | Type | Required | Description | Range | Example |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | device_id | int | yes | Device ID (must be BESS) | (0, +∞) | 1 |
    | current_cap | float | yes | Current capacity (kWh) | [0, +∞) | 50.0 |
    | start_time | int | yes | Start time (Unix seconds) | - | 1736143200 |
    | gap_minutes | int | yes | Time gap (minutes) | (0, +∞) | 15 |
    | mode | int | no | Optimization mode | 1/2/3 | 1 |
    | export_limit_kw | float | no | Export limit (kW), 0=no export | [0, +∞) | 0.0 |
    | demand_limit_kw | float | no | Soft demand cap (kW), omit or null=disabled | [0, +∞) | - |
    | demand_penalty | float | no | Overage penalty (per kWh) | [0, +∞) | 200.0 |
    | c_cycle | float | no | Cycle penalty (per kWh throughput) | [0, +∞) | 0.02 |

  - Body params

    - Data type: json

      | Param | Type | Required | Description | Example |
      | :---: | :---: | :------: | :---: | :---: |
      | pv_to_grid | array | mode=1/2 | PV sell-to-grid prices | - | [0.5, 0.6] |
      | pv_lcost | array | mode=1/2 | Local PV consumption prices | - | [0.4, 0.5] |
      | grid_sel | array | mode=1/2 | Grid prices | - | [0.8, 0.9] |
      | ems_from_pv | array | mode=1/2 | BESS purchase from PV prices | - | [0.3, 0.4] |
      | ems_from_grid | array | mode=1/2 | BESS purchase from grid prices | - | [0.7, 0.8] |
      | ems_lcost | array | mode=1/2 | BESS local consumption prices | - | [0.5, 0.6] |
      | ems_to_grid | array | mode=1/2 | BESS sell-to-grid prices | - | [0.6, 0.7] |

  - Mode (int): 1 = max BESS profit, 2 = max site profit, 3 = max green profit

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | result | array | yes | Optimized power per period | [10.5, 20.3] |
    | details | object | yes | Power flow details; each array has the same length as result | - |
    | pv_supply | array | yes | PV supply list | [50.0, 60.0] |
    | user_demand | array | yes | User demand list | [30.0, 40.0] |
    | prices | object | yes | Price dict | - |

    details object fields:

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | charge_from_grid | array | yes | BESS charging power from grid | [8.0, 0.0] |
    | charge_from_pv | array | yes | BESS charging power from PV; all 0 when there is no PV data | [2.0, 20.0] |
    | discharge_to_grid | array | yes | BESS discharging power to grid | [0.0, 0.1] |
    | discharge_to_load | array | yes | BESS discharging power to load | [0.5, 0.2] |

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 400 | 10001 | Invalid parameter | e.g. gap_minutes ≤ 0, invalid SOC bounds, demand_limit_profile length mismatch |
  | 400 | 32001 | Device is not a BESS device | The device type is not BESS |
  | 400 | 32002 | Invalid price data | Price series missing or lengths inconsistent |
  | 400 | 32004 | Missing load prediction data | Load device has no prediction data; run load prediction first |
  | 400 | 32005 | No PV device found | mode=3 requires at least one PV device on the site |
  | 400 | 32006 | No PV prediction data | mode=3 requires PV prediction data; run PV prediction first |
  | 404 | 20001 | Device not found | Invalid device ID |
  | 500 | 50002 | Max profit optimization failed | Solver returned no solution |
