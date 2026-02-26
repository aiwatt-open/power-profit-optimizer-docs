[[_TOC_]]

# 站点管理模块

## GET /v1/sites/

- 描述：获取站点列表

- 请求

  - Query参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | page | int | 否 | 页码 | [1, +∞) | 1 |
    | page_size | int | 否 | 每页记录数 | [1, 500] | 100 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | items | array | 是 | 站点列表 | - |
    | total | int | 是 | 总记录数 | 100 |
    | page | int | 是 | 当前页码 | 1 |
    | page_size | int | 是 | 每页记录数 | 100 |
    | total_pages | int | 是 | 总页数 | 1 |

    items 数组中每个元素包含：

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 站点ID | 1 |
    | name | string | 是 | 站点名称 | "站点1" |
    | latitude | float | 是 | 纬度 | 39.9042 |
    | longitude | float | 是 | 经度 | 116.4074 |
    | country | string | 是 | 国家 | "中国" |
    | city | string | 是 | 城市 | "北京" |
    | address | string | 否 | 站点地址 | "北京市朝阳区" |
    | description | string | 否 | 站点描述 | "测试站点" |
    | timezone | string | 是 | 站点时区 | "Asia/Shanghai" |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "items": [
        {
          "id": 1,
          "name": "站点1",
          "latitude": 39.9042,
          "longitude": 116.4074,
          "country": "中国",
          "city": "北京",
          "address": "北京市朝阳区",
          "description": "测试站点",
          "timezone": "Asia/Shanghai",
          "created_at": "2025-01-06T10:00:00",
          "updated_at": "2025-01-06T10:00:00"
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

## POST /v1/sites/

- 描述：创建站点

- 请求

  - Body参数

    - 数据类型：json

      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | name | string | 是 | 站点名称 | - | "站点1" |
      | latitude | float | 是 | 纬度 | [-90.0, 90.0] | 39.9042 |
      | longitude | float | 是 | 经度 | [-180.0, 180.0] | 116.4074 |
      | country | string | 是 | 国家 | - | "中国" |
      | city | string | 是 | 城市 | - | "北京" |
      | address | string | 否 | 站点地址 | - | "北京市朝阳区" |
      | description | string | 否 | 站点描述 | - | "测试站点" |
      | timezone | string | 是 | 站点时区 | IANA 时区标识符 | "Asia/Shanghai" |

    - 样例

      ```json
      {
        "name": "站点1",
        "latitude": 39.9042,
        "longitude": 116.4074,
        "country": "中国",
        "city": "北京",
        "address": "北京市朝阳区",
        "description": "测试站点",
        "timezone": "Asia/Shanghai"
      }
      ```

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 站点ID | 1 |
    | name | string | 是 | 站点名称 | "站点1" |
    | latitude | float | 是 | 纬度 | 39.9042 |
    | longitude | float | 是 | 经度 | 116.4074 |
    | country | string | 是 | 国家 | "中国" |
    | city | string | 是 | 城市 | "北京" |
    | address | string | 否 | 站点地址 | "北京市朝阳区" |
    | description | string | 否 | 站点描述 | "测试站点" |
    | timezone | string | 是 | 站点时区 | "Asia/Shanghai" |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "id": 1,
      "name": "站点1",
      "latitude": 39.9042,
      "longitude": 116.4074,
      "country": "中国",
      "city": "北京",
      "address": "北京市朝阳区",
      "description": "测试站点",
      "timezone": "Asia/Shanghai",
      "created_at": "2025-01-06T10:00:00",
      "updated_at": null
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 400 | 10001 | Invalid parameter | 时区字符串无效 |
  | 500 | 50001 | Internal server error | 数据库操作失败 |

## GET /v1/sites/:site_id

- 描述：获取站点详情

- 请求

  - Uri参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | 是 | 站点ID | (0, +∞) | 1 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 站点ID | 1 |
    | name | string | 是 | 站点名称 | "站点1" |
    | latitude | float | 是 | 纬度 | 39.9042 |
    | longitude | float | 是 | 经度 | 116.4074 |
    | country | string | 是 | 国家 | "中国" |
    | city | string | 是 | 城市 | "北京" |
    | address | string | 否 | 站点地址 | "北京市朝阳区" |
    | description | string | 否 | 站点描述 | "测试站点" |
    | timezone | string | 是 | 站点时区 | "Asia/Shanghai" |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T10:00:00" |

  - 样例

    ```json
    {
      "id": 1,
      "name": "站点1",
      "latitude": 39.9042,
      "longitude": 116.4074,
      "country": "中国",
      "city": "北京",
      "address": "北京市朝阳区",
      "description": "测试站点",
      "timezone": "Asia/Shanghai",
      "created_at": "2025-01-06T10:00:00",
      "updated_at": "2025-01-06T10:00:00"
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 20002 | Site not found | 站点不存在 |
  | 500 | 50001 | Internal server error | 数据库查询失败 |

## PUT /v1/sites/:site_id

- 描述：更新站点

- 请求

  - Uri参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | 是 | 站点ID | (0, +∞) | 1 |

  - Body参数

    - 数据类型：json

      | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
      | :---: | :---: | :------: | :---: | :---: | :---: |
      | name | string | 否 | 站点名称 | - | "站点1" |
      | latitude | float | 否 | 纬度 | [-90.0, 90.0] | 39.9042 |
      | longitude | float | 否 | 经度 | [-180.0, 180.0] | 116.4074 |
      | country | string | 否 | 国家 | - | "中国" |
      | city | string | 否 | 城市 | - | "北京" |
      | address | string | 否 | 站点地址 | - | "北京市朝阳区" |
      | description | string | 否 | 站点描述 | - | "测试站点" |
      | timezone | string | 否 | 站点时区 | IANA 时区标识符 | "Asia/Shanghai" |

    - 样例

      ```json
      {
        "name": "站点1更新",
        "city": "上海"
      }
      ```

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | id | int | 是 | 站点ID | 1 |
    | name | string | 是 | 站点名称 | "站点1更新" |
    | latitude | float | 是 | 纬度 | 39.9042 |
    | longitude | float | 是 | 经度 | 116.4074 |
    | country | string | 是 | 国家 | "中国" |
    | city | string | 是 | 城市 | "上海" |
    | address | string | 否 | 站点地址 | "北京市朝阳区" |
    | description | string | 否 | 站点描述 | "测试站点" |
    | timezone | string | 是 | 站点时区 | "Asia/Shanghai" |
    | created_at | datetime | 是 | 创建时间 | "2025-01-06T10:00:00" |
    | updated_at | datetime | 否 | 更新时间 | "2025-01-06T11:00:00" |

  - 样例

    ```json
    {
      "id": 1,
      "name": "站点1更新",
      "latitude": 39.9042,
      "longitude": 116.4074,
      "country": "中国",
      "city": "上海",
      "address": "北京市朝阳区",
      "description": "测试站点",
      "timezone": "Asia/Shanghai",
      "created_at": "2025-01-06T10:00:00",
      "updated_at": "2025-01-06T11:00:00"
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 400 | 10001 | Invalid parameter | 时区字符串无效 |
  | 404 | 20002 | Site not found | 站点不存在 |
  | 500 | 50001 | Internal server error | 数据库操作失败 |

## DELETE /v1/sites/:site_id

- 描述：删除站点

- 请求

  - Uri参数

    | 参数 | 类型 | 是否必填 | 描述 | 范围 | 样例 |
    | :---: | :---: | :------: | :---: | :---: | :---: |
    | site_id | int | 是 | 站点ID | (0, +∞) | 1 |

- 响应

  - 数据类型：json

    | 参数 | 类型 | 是否必填 | 描述 | 样例 |
    | :---: | :---: | :------: | :---: | :---: |
    | message | string | 是 | 响应消息 | "Site deleted successfully" |

  - 样例

    ```json
    {
      "message": "Site deleted successfully"
    }
    ```

- 错误码

  | HTTP状态码 | 错误码 | 描述 | 说明 |
  | :----: | :--: | :--: | ---- |
  | 404 | 20002 | Site not found | 站点不存在 |
  | 500 | 50001 | Internal server error | 数据库操作失败 |

