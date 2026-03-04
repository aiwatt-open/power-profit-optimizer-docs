[[_TOC_]]

# 设备管理模块

## POST /v1/devices/

- 描述：创建设备

- 请求

  - Body参数

    - 数据类型：json


      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | name | string | 是 | 设备名称 | - | "光伏设备1" |
      | type | string | 是 | 设备类型 | PV/Load/BESS | "PV" |
      | site_id | int | 是 | 所属站点ID | (0, +∞) | 1 |
      | total_capacity | float | 否 | 总容量（单位：kWh） | (0, +∞) | 100.0 |
      | max_power | float | 否 | 最大功率（单位：kW） | (0, +∞) | 50.0 |
      | energy_conversion_efficiency | float | 否 | 能量转换效率（%） | (0, 100] | 85.5 |
      | max_soc | float | 否 | 最大SOC（%） | (0, 100] | 90.0 |
      | min_soc | float | 否 | 最小SOC（%） | [0, 100] | 10.0 |
      | panel_tilt | float | 否 | 光伏板倾角（°）<br>水平=0，垂直=90 | [0, 90] | 30.0 |
      | panel_azimuth | float | 否 | 光伏板方位角（°）<br>北=0, 东=90, 南=180,西=270 | [0, 360) | 180.0 |


      可选字段说明: 
      
      - **PV（光伏）**
        - 必填: `total_capacity`、`max_power`
        - 可选: `panel_tilt`、`panel_azimuth`
      - **BESS（储能）**：
        - 必填:  `total_capacity`、`max_power`、`energy_conversion_efficiency`、`min_soc`、`max_soc`
      - **Load（负载）**：无必填

    - 样例

      ```json
      {
        "name": "光伏设备1",
        "type": "PV",
        "site_id": 1,
        "total_capacity": 100.0,
        "max_power": 50.0,
        "panel_tilt": 30.0,
        "panel_azimuth": 180.0
      }
      ```

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 设备ID | 1 |
    | name | string | 是 | 设备名称 | "光伏设备1" |
    | type | string | 是 | 设备类型 | "PV" |
    | site_id | int | 是 | 所属站点ID | 1 |
    | total_capacity | float | 否 | 总容量（单位：kWh） | 100.0 |
    | max_power | float | 否 | 最大功率（单位：kW） | 50.0 |
    | energy_conversion_efficiency | float | 否 | 能量转换效率（%） | 85.5 |
    | max_soc | float | 否 | 最大SOC（%） | 90.0 |
    | min_soc | float | 否 | 最小SOC（%） | 10.0 |
    | panel_tilt | float | 否 | 光伏板倾角（°） | 30.0 |
    | panel_azimuth | float | 否 | 光伏板方位角（°） | 180.0 |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "id": 1,
      "name": "光伏设备1",
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

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 400 | 10001 | A device with the same name already exists | 设备名称重复 |
  | 404 | 20002 | Site not found | 站点ID无效 |
  | 500 | 50001 | Internal server error | 数据库操作失败 |

## GET /v1/devices/

- 描述：获取设备列表

- 请求

  - Query参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | page | int | 否 | 页码 | [1, +∞) | 1 |
    | page_size | int | 否 | 每页记录数 | [1, 500] | 100 |
    | site_id | int | 否 | 站点ID（过滤条件） | (0, +∞) | 1 |
    | device_type | string | 否 | 设备类型（过滤条件） | PV/Load/BESS | "PV" |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | 是 | 设备列表 | - |
    | total | int | 是 | 总记录数 | 100 |
    | page | int | 是 | 当前页码 | 1 |
    | page_size | int | 是 | 每页记录数 | 100 |
    | total_pages | int | 是 | 总页数 | 1 |

    items 数组中每个元素包含：

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 设备ID | 1 |
    | name | string | 是 | 设备名称 | "光伏设备1" |
    | type | string | 是 | 设备类型 | "PV" |
    | site_id | int | 是 | 所属站点ID | 1 |
    | total_capacity | float | 否 | 总容量（单位：kWh） | 100.0 |
    | max_power | float | 否 | 最大功率（单位：kW） | 50.0 |
    | energy_conversion_efficiency | float | 否 | 能量转换效率（%） | 85.5 |
    | max_soc | float | 否 | 最大SOC（%） | 90.0 |
    | min_soc | float | 否 | 最小SOC（%） | 10.0 |
    | panel_tilt | float | 否 | 光伏板倾角（°） | 30.0 |
    | panel_azimuth | float | 否 | 光伏板方位角（°） | 180.0 |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "items": [
        {
          "id": 1,
          "name": "光伏设备1",
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

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 500 | 50001 | Internal server error | 数据库查询失败 |

## GET /v1/devices/:device_id

- 描述：获取设备详情

- 请求

  - Uri参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | device_id | int | 是 | 设备ID | (0, +∞) | 1 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 设备ID | 1 |
    | name | string | 是 | 设备名称 | "光伏设备1" |
    | type | string | 是 | 设备类型 | "PV" |
    | site_id | int | 是 | 所属站点ID | 1 |
    | total_capacity | float | 否 | 总容量（单位：kWh） | 100.0 |
    | max_power | float | 否 | 最大功率（单位：kW） | 50.0 |
    | energy_conversion_efficiency | float | 否 | 能量转换效率（%） | 85.5 |
    | max_soc | float | 否 | 最大SOC（%） | 90.0 |
    | min_soc | float | 否 | 最小SOC（%） | 10.0 |
    | panel_tilt | float | 否 | 光伏板倾角（°） | 30.0 |
    | panel_azimuth | float | 否 | 光伏板方位角（°） | 180.0 |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "id": 1,
      "name": "光伏设备1",
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

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 20001 | Device not found | 设备ID无效 |
  | 500 | 50001 | Internal server error | 数据库查询失败 |

## PUT /v1/devices/:device_id

- 描述：更新设备

- 请求

  - Uri参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | device_id | int | 是 | 设备ID | (0, +∞) | 1 |

  - Body参数

    - 数据类型：json

      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | name | string | 否 | 设备名称 | - | "光伏设备1更新" |
      | type | string | 否 | 设备类型 | PV/Load/BESS | "PV" |
      | site_id | int | 否 | 所属站点ID | (0, +∞) | 1 |
      | total_capacity | float | 否 | 总容量（单位：kWh） | (0, +∞) | 100.0 |
      | max_power | float | 否 | 最大功率（单位：kW） | (0, +∞) | 50.0 |
      | energy_conversion_efficiency | float | 否 | 能量转换效率（%） | (0, 100] | 85.5 |
      | max_soc | float | 否 | 最大SOC（%） | (0, 100] | 90.0 |
      | min_soc | float | 否 | 最小SOC（%） | [0, 100] | 10.0 |
      | panel_tilt | float | 否 | 光伏板倾角（°）<br>水平=0，垂直=90 | [0, 90] | 30.0 |
      | panel_azimuth | float | 否 | 光伏板方位角（°）<br>北=0, 东=90, 南=180, 西=270 | [0, 360) | 180.0 |

    - 说明：Body 字段与 [POST 创建设备](#post-v1devices) 一致，均为可选（按需更新）；各类型必填/可选见上文「可选字段说明」。

    - 样例

      ```json
      {
        "name": "光伏设备1更新",
        "max_power": 60.0
      }
      ```

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 设备ID | 1 |
    | name | string | 是 | 设备名称 | "光伏设备1更新" |
    | type | string | 是 | 设备类型 | "PV" |
    | site_id | int | 是 | 所属站点ID | 1 |
    | total_capacity | float | 否 | 总容量（单位：kWh） | 100.0 |
    | max_power | float | 否 | 最大功率（单位：kW） | 60.0 |
    | energy_conversion_efficiency | float | 否 | 能量转换效率（%） | 85.5 |
    | max_soc | float | 否 | 最大SOC（%） | 90.0 |
    | min_soc | float | 否 | 最小SOC（%） | 10.0 |
    | panel_tilt | float | 否 | 光伏板倾角（°） | 30.0 |
    | panel_azimuth | float | 否 | 光伏板方位角（°） | 180.0 |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T11:00:00" |

  - 样例

    ```json
    {
      "id": 1,
      "name": "光伏设备1更新",
      "type": "PV",
      "site_id": 1,
      "total_capacity": 100.0,
      "max_power": 60.0,
      "energy_conversion_efficiency": null,
      "max_soc": null,
      "min_soc": null,
      "panel_tilt": 30.0,
      "panel_azimuth": 180.0,
      "created_at": "2025-01-06T10:00:00",
      "updated_at": "2025-01-06T11:00:00"
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 400 | 10001 | A device with the same name already exists | 设备名称重复 |
  | 404 | 20001 | Device not found | 设备ID无效 |
  | 500 | 50001 | Internal server error | 数据库操作失败 |

## DELETE /v1/devices/:device_id

- 描述：删除设备

- 请求

  - Uri参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | device_id | int | 是 | 设备ID | (0, +∞) | 1 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | message | string | 是 | 响应消息 | "Device deleted" |

  - 样例

    ```json
    {
      "message": "Device deleted"
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 20001 | Device not found | 设备ID无效 |
  | 500 | 50001 | Internal server error | 数据库操作失败 |

