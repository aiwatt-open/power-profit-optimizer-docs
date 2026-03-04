[[_TOC_]]

# 天气数据模块

## GET /v1/weather/basic

- 描述：获取天气数据

- 请求

  - Query参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | 是 | 站点ID | (0, +∞) | 1 |
    | start_time | int | 否 | 开始时间（Unix 时间戳，秒） | - | 1736143200 |
    | end_time | int | 否 | 结束时间（Unix 时间戳，秒） | - | 1736229599 |
    | is_forecast | bool | 否 | 是否为预测数据，不传=全部，false=历史真实数据，true=预测数据 | - | false |
    | page | int | 否 | 页码 | [1, +∞) | 1 |
    | page_size | int | 否 | 每页记录数 | [1, 500] | 10 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | 是 | 天气数据列表 | - |
    | total | int | 是 | 总记录数 | 100 |
    | page | int | 是 | 当前页码 | 1 |
    | page_size | int | 是 | 每页记录数 | 10 |
    | total_pages | int | 是 | 总页数 | 10 |

    items 数组中每个元素包含：

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 记录ID | 1 |
    | site_id | int | 是 | 站点ID | 1 |
    | latitude | float | 是 | 纬度 | 39.9042 |
    | longitude | float | 是 | 经度 | 116.4074 |
    | timestamp | int | 是 | Unix 时间戳（秒） | 1736143200 |
    | temperature | float | 是 | 温度(℃) | 25.5 |
    | humidity | float | 是 | 湿度(%) | 60.0 |
    | wind_speed | float | 是 | 风速(m/s) | 5.2 |
    | wind_direction | float | 是 | 风向(度) | 180.0 |
    | cloud_cover | float | 是 | 云量(%) | 30.0 |
    | is_forecast | bool | 是 | 是否为预测数据，false=历史真实数据，true=预测数据 | false |

  - 样例

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

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 32502 | Weather data not found | 查询结果为空 |
  | 500 | 50001 | Internal server error | 数据库查询失败 |

## GET /v1/weather/solar

- 描述：获取太阳辐射数据

- 请求

  - Query参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | 是 | 站点ID | (0, +∞) | 1 |
    | start_time | int | 否 | 开始时间（Unix 时间戳，秒） | - | 1736143200 |
    | end_time | int | 否 | 结束时间（Unix 时间戳，秒） | - | 1736229599 |
    | is_forecast | bool | 否 | 是否为预测数据，不传=全部，false=历史真实数据，true=预测数据 | - | false |
    | page | int | 否 | 页码 | [1, +∞) | 1 |
    | page_size | int | 否 | 每页记录数 | [1, 100] | 10 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | 是 | 太阳辐射数据列表 | - |
    | total | int | 是 | 总记录数 | 100 |
    | page | int | 是 | 当前页码 | 1 |
    | page_size | int | 是 | 每页记录数 | 10 |
    | total_pages | int | 是 | 总页数 | 10 |

    items 数组中每个元素包含：

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 记录ID | 1 |
    | site_id | int | 是 | 站点ID | 1 |
    | latitude | float | 是 | 纬度 | 39.9042 |
    | longitude | float | 是 | 经度 | 116.4074 |
    | timestamp | int | 是 | Unix 时间戳（秒） | 1736143200 |
    | shortwave_radiation | float | 否 | 短波辐射 (W/m²) | 500.0 |
    | direct_radiation | float | 否 | 直接辐射 (W/m²) | 300.0 |
    | diffuse_radiation | float | 否 | 散射辐射 (W/m²) | 200.0 |
    | direct_normal_irradiance | float | 否 | 直接法向辐射 (W/m²) | 600.0 |
    | global_tilted_irradiance | float | 否 | 全球倾斜辐射 (W/m²) | 450.0 |
    | terrestrial_radiation | float | 否 | 地面辐射 (W/m²) | 1200.0 |
    | is_forecast | bool | 是 | 是否为预测数据，false=历史真实数据，true=预测数据 | false |

  - 样例

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
          "is_forecast": false
        }
      ],
      "total": 100,
      "page": 1,
      "page_size": 10,
      "total_pages": 10
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 32502 | Solar radiation data not found | 查询结果为空 |
  | 500 | 50001 | Internal server error | 数据库查询失败 |
