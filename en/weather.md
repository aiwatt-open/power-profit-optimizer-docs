[[_TOC_]]

# Weather Data

## GET /v1/weather/basic

- Description: Get weather data

- Request

  - Query params

    | Param | Type | Required | Description | Range | Example |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | yes | Site ID | (0, +∞) | 1 |
    | start_time | int | no | Start time (Unix timestamp, seconds) | - | 1736143200 |
    | end_time | int | no | End time (Unix timestamp, seconds) | - | 1736229599 |
    | is_forecast | bool | no | Filter by forecast: omit=all, false=historical, true=forecast | - | false |
    | page | int | no | Page number | [1, +∞) | 1 |
    | page_size | int | no | Page size | [1, 500] | 10 |

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | yes | Weather records | - |
    | total | int | yes | Total count | 100 |
    | page | int | yes | Current page | 1 |
    | page_size | int | yes | Page size | 10 |
    | total_pages | int | yes | Total pages | 10 |

    Each item in items:

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | yes | Record ID | 1 |
    | site_id | int | yes | Site ID | 1 |
    | latitude | float | yes | Latitude | 39.9042 |
    | longitude | float | yes | Longitude | 116.4074 |
    | timestamp | int | yes | Unix timestamp (seconds) | 1736143200 |
    | temperature | float | yes | Temperature (°C) | 25.5 |
    | humidity | float | yes | Humidity (%) | 60.0 |
    | wind_speed | float | yes | Wind speed (m/s) | 5.2 |
    | wind_direction | float | yes | Wind direction (degrees) | 180.0 |
    | cloud_cover | float | yes | Cloud cover (%) | 30.0 |
    | is_forecast | bool | yes | Is forecast: false=historical, true=forecast | false |

  - Example

    ```json
    {
      "items": [
        {
          "id": 1,
          "site_id": 1,
          "latitude": 39.9042,
          "longitude": 116.4074,
          "timestamp": 1736143200,
          "temperature": 25.5,
          "humidity": 60.0,
          "wind_speed": 5.2,
          "wind_direction": 180.0,
          "cloud_cover": 30.0,
          "is_forecast": false
        }
      ],
      "total": 100,
      "page": 1,
      "page_size": 10,
      "total_pages": 10
    }
    ```

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 404 | 32502 | Weather data not found |
  | 500 | 50001 | Internal server error |

## GET /v1/weather/solar

- Description: Get solar radiation data

- Request

  - Query params

    | Param | Type | Required | Description | Range | Example |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | yes | Site ID | (0, +∞) | 1 |
    | start_time | int | no | Start time (Unix timestamp, seconds) | - | 1736143200 |
    | end_time | int | no | End time (Unix timestamp, seconds) | - | 1736229599 |
    | page | int | no | Page number | [1, +∞) | 1 |
    | page_size | int | no | Page size | [1, 100] | 10 |

- Response

  - Data type: json

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | yes | Solar radiation records | - |
    | total | int | yes | Total count | 100 |
    | page | int | yes | Current page | 1 |
    | page_size | int | yes | Page size | 10 |
    | total_pages | int | yes | Total pages | 10 |

    Each item in items (Open-Meteo API fields):

    | Param | Type | Required | Description | Example |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | yes | Record ID | 1 |
    | site_id | int | yes | Site ID | 1 |
    | latitude | float | yes | Latitude | 39.9042 |
    | longitude | float | yes | Longitude | 116.4074 |
    | timestamp | int | yes | Unix timestamp (seconds) | 1736143200 |
    | shortwave_radiation | float | no | Shortwave radiation (W/m²) | 500.0 |
    | direct_radiation | float | no | Direct radiation (W/m²) | 300.0 |
    | diffuse_radiation | float | no | Diffuse radiation (W/m²) | 200.0 |
    | direct_normal_irradiance | float | no | Direct normal irradiance (W/m²) | 600.0 |
    | global_tilted_irradiance | float | no | Global tilted irradiance (W/m²) | 450.0 |
    | terrestrial_radiation | float | no | Terrestrial radiation (W/m²) | 1200.0 |
    | created_at | datetime | yes | Created at | "2025-01-06T10:00:00" |
    | updated_at | datetime | no | Updated at | "2025-01-06T10:00:00" |

  - Example

    ```json
    {
      "items": [
        {
          "id": 1,
          "site_id": 1,
          "latitude": 39.9042,
          "longitude": 116.4074,
          "timestamp": 1736143200,
          "shortwave_radiation": 500.0,
          "direct_radiation": 300.0,
          "diffuse_radiation": 200.0,
          "direct_normal_irradiance": 600.0,
          "global_tilted_irradiance": 450.0,
          "terrestrial_radiation": 1200.0,
          "created_at": "2025-01-06T10:00:00",
          "updated_at": null
        }
      ],
      "total": 100,
      "page": 1,
      "page_size": 10,
      "total_pages": 10
    }
    ```

- Error codes

  | HTTP Status | Code | Message |
  | :----: | :--: | :--: |
  | 404 | 32502 | Solar radiation data not found |
  | 500 | 50001 | Internal server error |
