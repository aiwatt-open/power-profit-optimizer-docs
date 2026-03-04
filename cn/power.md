[[_TOC_]]

# 电力数据模块

## POST /v1/power/records

- 描述：创建单个功率记录

- 请求

  - Body参数

    - 数据类型：json

      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | device_id | int | 是 | 设备ID | (0, +∞) | 1 |
      | timestamp | int | 是 | Unix 时间戳（秒） | - | 1736143200 |
      | value | float | 是 | 功率值（kW） | [0, +∞) | 50.5 |
      | abnormal | bool | 否 | 是否异常：true=不正常，false=正常 | - | false |

    - 样例

      ```json
      {
        "device_id": 1,
        "timestamp": 1736143200,
        "value": 50.5,
        "abnormal": false
      }
      ```

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 记录ID | 1 |
    | device_id | int | 是 | 设备ID | 1 |
    | timestamp | int | 是 | Unix 时间戳（秒） | 1736143200 |
    | value | float | 是 | 功率值（kW） | 50.5 |
    | abnormal | bool | 否 | 是否异常：true=不正常，false=正常 | false |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "id": 1,
      "device_id": 1,
      "timestamp": 1736143200,
      "value": 50.5,
      "abnormal": false,
      "created_at": "2025-01-06T10:00:00",
      "updated_at": null
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 20001 | Device not found | 设备ID无效 |
  | 500 | 50001 | Failed to create power record | 数据库操作失败 |

## POST /v1/power/records/batch

- 描述：批量创建功率记录

- 请求

  - Body参数

    - 数据类型：json

      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | device_id | int | 是 | 设备ID | (0, +∞) | 1 |
      | data | array | 是 | 功率记录列表 | - | - |

      data 数组中每个元素包含：

      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | timestamp | int | 是 | Unix 时间戳（秒） | - | 1736143200 |
      | value | float | 是 | 功率值（kW） | [0, +∞) | 50.5 |
      | abnormal | bool | 否 | 是否异常：true=不正常，false=正常 | - | false |

    - 样例

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

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | inserted_count | int | 是 | 成功插入的记录数量 | 100 |

  - 样例

    ```json
    {
      "inserted_count": 100
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 20001 | Device not found | 设备ID无效 |
  | 500 | 50001 | Failed to create power records in batch | 数据库操作失败 |

## GET /v1/power/records

- 描述：获取设备的功率记录

- 请求

  - Query参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | device_id | int | 是 | 设备ID | (0, +∞) | 1 |
    | start_time | int | 否 | 开始时间，Unix 时间戳（秒） | - | 1736143200 |
    | end_time | int | 否 | 结束时间，Unix 时间戳（秒） | - | 1736229599 |
    | page | int | 否 | 页码 | [1, +∞) | 1 |
    | page_size | int | 否 | 每页记录数 | [1, 500] | 10 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | 是 | 功率记录列表 | - |
    | total | int | 是 | 总记录数 | 100 |
    | page | int | 是 | 当前页码 | 1 |
    | page_size | int | 是 | 每页记录数 | 10 |
    | total_pages | int | 是 | 总页数 | 10 |

    items 数组中每个元素包含：

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 记录ID | 1 |
    | device_id | int | 是 | 设备ID | 1 |
    | timestamp | int | 是 | Unix 时间戳（秒） | 1736143200 |
    | value | float | 是 | 功率值（kW） | 50.5 |
    | abnormal | bool | 否 | 是否异常：true=不正常，false=正常 | false |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "items": [
        {
          "id": 1,
          "device_id": 1,
          "timestamp": 1736143200,
          "value": 50.5,
          "abnormal": false,
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

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 20001 | Device not found | 设备ID无效或查询结果为空 |
  | 400 | 32501 | No power records | 无功率记录 |
  | 500 | 50001 | Internal server error | 数据库查询失败 |


## POST /v1/power/max_profit

- 描述：最大利润优化

- 请求

  - Query参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | 是 | 站点ID | (0, +∞) | 1 |
    | current_cap | float | 是 | 储能当前总电量（kWh） | [0, +∞) | 50.0 |
    | start_time | int | 是 | 开始时间，Unix 时间戳（秒） | - | 1736143200 |
    | gap_minutes | int | 是 | 时间间隔（分钟） | (0, +∞) | 15 |
    | mode | int | 否 | 优化模式 | 1/2/3 | 1 |
    | export_limit_kw | float | 否 | 防逆流/外送限制（kW），不传或 null=不启用<br>正数表示从电网取电，负数表示放电到电网 | (-∞, +∞) | 0.0 |
    | demand_limit_kw | float | 否 | 固定软需量上限（kW），不传或 null=不启用 | [0, +∞) | - |
    | demand_penalty | float | 否 | 超需量惩罚（货币单位/kWh） | [0, +∞) | 200.0 |
    | c_cycle | float | 否 | 循环惩罚系数（货币单位/kWh 吞吐量） | [0, +∞) | 0.02 |

  - Body参数

    - 数据类型：json

      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | pv_to_grid | array | 是 | 光伏向电网售电价格列表 | - | [0.5, 0.6] |
      | pv_lcost | array | 是 | 光伏本地消纳价格列表 | - | [0.4, 0.5] |
      | grid_sel | array | 是 | 电网电价列表 | - | [0.8, 0.9] |
      | ems_from_pv | array | 是 | 储能从光伏购电价格列表 | - | [0.3, 0.4] |
      | ems_from_grid | array | 是 | 储能从电网购电价格列表 | - | [0.7, 0.8] |
      | ems_lcost | array | 是 | 储能本地消纳价格列表 | - | [0.5, 0.6] |
      | ems_to_grid | array | 是 | 储能向电网售电价格列表 | - | [0.6, 0.7] |

    - 样例

      ```json
      {
        "pv_to_grid": [0.5, 0.6, 0.7],
        "pv_lcost": [0.4, 0.5, 0.6],
        "grid_sel": [0.8, 0.9, 1.0],
        "ems_from_pv": [0.3, 0.4, 0.5],
        "ems_from_grid": [0.7, 0.8, 0.9],
        "ems_lcost": [0.5, 0.6, 0.7],
        "ems_to_grid": [0.6, 0.7, 0.8]
      }
      ```

    - mode 参数说明：
      - 1: 储能收益最大
      - 2: 站点收益最大
      - 3: 绿色收益最大

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | result | array | 是 | 优化结果列表（每个时间段的功率值） | [10.5, 20.3] |
    | details | object | 是 | 详细信息 | - |
    | pv_supply | array | 是 | 光伏供应列表 | [50.0, 60.0] |
    | user_demand | array | 是 | 用户需求列表 | [30.0, 40.0] |
    | prices | object | 是 | 价格字典 | - |

  - 样例

    ```json
    {
      "result": [10.5, 20.3, 15.2],
      "details": {
        "charge": [10.0, 20.0, 15.0],
        "discharge": [0.5, 0.3, 0.2]
      },
      "pv_supply": [50.0, 60.0, 55.0],
      "user_demand": [30.0, 40.0, 35.0],
      "prices": {
        "pv_to_grid": [0.5, 0.6, 0.7],
        "pv_lcost": [0.4, 0.5, 0.6],
        "grid_sel": [0.8, 0.9, 1.0],
        "ems_from_pv": [0.3, 0.4, 0.5],
        "ems_from_grid": [0.7, 0.8, 0.9],
        "ems_lcost": [0.5, 0.6, 0.7],
        "ems_to_grid": [0.6, 0.7, 0.8]
      }
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 400 | 32001 | Device is not BESS | 设备不是储能设备 |
  | 400 | 32002 | Invalid price data | 价格数据异常 |
  | 404 | 20001 | Device not found | 设备ID无效 |
  | 500 | 50002 | Max profit optimization failed | 优化算法执行失败 |
