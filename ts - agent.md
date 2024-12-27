## TypeScript, JS, Ecmascript

### INSTRUCTION

> *insert your instruction here*

You are a professional code translator specializing in translating TypeScript source code, documentation, and comments.

Your task is to translate all Chinese comments or text within the provided TypeScript code into English.
Ensure the file structure, imports, and comments are preserved exactly as they are.
Keep all comments in their original format, including single-line or block comments.
Include line numbers and preserve alignment for readability.
Return the result as a plain text JSON object containing:

"translatedCode": The complete TypeScript code translated into English, maintaining its formatting.
"details": A JSON array where each object contains:
"lineNumber": The exact line number of the translation.
"lineType": Whether the line is a comment, annotation, or other.
"jobType": The type of transformation, e.g., "Text Translation".
"originalText": The text before translation.
"translatedText": The translated text.

Ensure the line numbers in "details" align exactly with the original input file (do not add new line). Do not include markdown formatting (e.g., ```json) in the response. Return the JSON as plain text.

### DEMOS1

#### Input:

```typescript
import request from '@/plugins/axios-last'

// 根据数据中心获取位置信息
export function getLocationTree(dcId) {
  return request.get('/api/v3/infra-database-backend/geo/batch/dc_location', { params: { dcId } })
}

/**
 * 查询实例某个等级的规则详情（含翻译）
 * @returns
 */
export function translationDetail({ event_id, event_level_id }) {
  return request.get('/jpi/dcim/event/inst/translation_detail', {
    params: {
      event_id, event_level_id
    }
  })
}

/**
 * 查询告警历程
 * @returns
 */
export function processDetail(alarm_id) {
  return request.get('/jpi/dcim/alarm/process_detail', {
    params: {
      alarm_id
    }
  })
}

/**
 * 获取设备的运行参数实例列表
 * @returns
 */
export function instAttrs(device_id) {
  return request.get('/jpi/dcim/device/attr/inst_attrs', {
    params: {
      device_id
    }
  })
}

/**
 * 查询机柜指标实例列表
 * @returns
 */
export function instIndis(cabinet_id) {
  return request.get('/jpi/dcim/cabinet/indi/indis', {
    params: {
      cabinet_id
    }
  })
}

/**
 * 获取告警设备最新运行情况
 * @returns
 */
export function dataLatest(alarmIds) {
  return request.get('/jpi/dcim/alarm/detail/data_latest', {
    params: {
      alarmIds
    }
  })
}

/**
 * 获取Event模板排障指引
 * @returns
 */
export function debugGuide(temp_event_id) {
  return request.get('/jpi/dcim/temp/event/debug_guide', {
    params: {
      temp_event_id
    }
  })
}

// 关注告警
export function concern(alarm_id) {
  return request.post(`/jpi/dcim/alarm/handler/concern?alarm_id=${alarm_id}`)
}

// 关注告警
export function unConcern(alarm_id) {
  return request.post(`/jpi/dcim/alarm/handler/unconcern?alarm_id=${alarm_id}`)
}

// 确认告警
export function confirm(data) {
  return request.post('/jpi/dcim/alarm/handler/confirm', data)
}

// 根据主体确认告警
export function targetConfirm(data) {
  return request.post('/jpi/dcim/alarm/handler/target/confirm', data)
}

// 原因类型字典
export function reasonDict() {
  return request.get('/jpi/dcim/alarm/handler/reason/type')
}

// 查询原因
export function reasonAlarm(alarm_id) {
  return request.get(`/jpi/dcim/alarm/handler/reason?alarm_id=${alarm_id}`)
}

// 更新原因
export function updateReason(data) {
  return request.post('/jpi/dcim/alarm/handler/update/reason', data)
}

// 更新告警主体原因
export function updateTargetReason(data) {
  return request.post('/jpi/dcim/alarm/handler/target/update/reason', data)
}

// 根据数据中心和keyword模糊查询
export function unionSearch(params) {
  return request.get('/jpi/dcim/device/union', { params })
}

export function unionSearchCabinets(params) {
  params = Object.assign(params, { search_type: 1 })
  return request.get('/jpi/dcim/cabinet/union', { params })
}

// 获取设备列表
export function getDevices(params) {
  return request.get('/jpi/dcim/device/deviceList', { params })
}

// 获取机柜列表
export function getCabinets(params) {
  return request.get('/jpi/dcim/cabinet/cabinets', { params })
}

// 获取设备详情
export function getDeviceDetail(id) {
  return request.get(`/jpi/dcim/device/${id}`)
}
// 获取机柜详情
export function getCabinetDetail(id) {
  return request.get(`/jpi/dcim/cabinet/${id}`)
}

// 获取机柜配电
export function getCabinetAttr(id) {
  return request.get(`/jpi/dcim/cabinet/attr/data?limit=0&offset=0&cabinet_id=${id}`)
}

// 触发应急
export function handlerEmer(alarm_id) {
  return request.post(`/jpi/dcim/alarm/handler/emergency?alarm_id=${alarm_id}`)
}

// 获取影响客户列表
export function influenceCustomer(alarm_id) {
  return request.get(`/jpi/dcim/alarm/influence/customer?alarm_id=${alarm_id}`)
}

// 获取计划类型
export function getPlanType() {
  return request.get('/jpi/dcim/alarm/plan/type')
}

// 一键开单
export function handleIncident(alarm_id) {
  return request.post(`/jpi/dcim/alarm/handler/incident?alarmId=${alarm_id}`)
}

// 批量关闭已恢复的告警
export function batchCloseRecovered(data) {
  return request.post('/jpi/dcim/alarm/handler/confirm/recovered/batch', data)
}

// 批量关闭未恢复告警
export function batchCloseUnRecovered(data) {
  return request.post('/jpi/dcim/alarm/handler/confirm/unrecovered/batch', data)
}

// 批量关闭消息告警
export function batchCloseMessage(data) {
  return request.post('/jpi/dcim/alarm/handler/confirm/message/batch', data)
}

// 获取应急列表
export function emergencyAlarm() {
  return request.get('/jpi/dcim/alarm/handler/emergency/alarm')
}

// 模拟告警恢复
export function simulateRecover(alarm_id) {
  return request.post(`/jpi/dcim/alarm/handler/simulate/recover?alarm_id=${alarm_id}`)
}

// 获取告警操作log
export function getLog(alarm_id) {
  return request.get(`/jpi/dcim/alarm/log?alarm_id=${alarm_id}`)
}

// 获取应急list
export function getEmerList() {
  return request.get('/jpi/dcim/alarm/handler/emergency/list')
}

// 查询当前ai控制状态
export function aiStatus() { // 生产常熟二号为141
  return request.get(`/jpi/dcim/ai_control/status?dc_id=${141}`)
}

// 变更AI控制模式
export function changeAiStatus(data) {
  return request.post('/jpi/dcim/ai_control/change', data)
}

// 查询旧告警数量
export function oldAlarmCount() {
  return request.get('/jpi/dcim/alarm/handler/old/alarm_count')
}

// 获取未确认的告警数量
export function oldUnconfirmCount() {
  return request.get('/jpi/dcim/alarm/handler/old/unConfirm_count')
}

// 普通通知
export function sendNotice(data) {
  return request.post('/jpi/dcim/alarm/handler/normal/notice', data)
}
```

#### Output:

```json
{
  "translatedCode": "import request from '@/plugins/axios-last'\n\n// Get location information based on data center\nexport function getLocationTree(dcId) {\n  return request.get('/api/v3/infra-database-backend/geo/batch/dc_location', { params: { dcId } })\n}\n\n/**\n * Query rule details for a specific level of an instance (including translations)\n * @returns\n */\nexport function translationDetail({ event_id, event_level_id }) {\n  return request.get('/jpi/dcim/event/inst/translation_detail', {\n    params: {\n      event_id, event_level_id\n    }\n  })\n}\n\n/**\n * Query alarm history\n * @returns\n */\nexport function processDetail(alarm_id) {\n  return request.get('/jpi/dcim/alarm/process_detail', {\n    params: {\n      alarm_id\n    }\n  })\n}\n\n/**\n * Get the runtime parameter instance list of the device\n * @returns\n */\nexport function instAttrs(device_id) {\n  return request.get('/jpi/dcim/device/attr/inst_attrs', {\n    params: {\n      device_id\n    }\n  })\n}\n\n/**\n * Query the indicator instance list of the cabinet\n * @returns\n */\nexport function instIndis(cabinet_id) {\n  return request.get('/jpi/dcim/cabinet/indi/indis', {\n    params: {\n      cabinet_id\n    }\n  })\n}\n\n/**\n * Get the latest running status of the alarm device\n * @returns\n */\nexport function dataLatest(alarmIds) {\n  return request.get('/jpi/dcim/alarm/detail/data_latest', {\n    params: {\n      alarmIds\n    }\n  })\n}\n\n/**\n * Get Event template troubleshooting guide\n * @returns\n */\nexport function debugGuide(temp_event_id) {\n  return request.get('/jpi/dcim/temp/event/debug_guide', {\n    params: {\n      temp_event_id\n    }\n  })\n}\n\n// Focus alarm\nexport function concern(alarm_id) {\n  return request.post(`/jpi/dcim/alarm/handler/concern?alarm_id=${alarm_id}`)\n}\n\n// Unfocus alarm\nexport function unConcern(alarm_id) {\n  return request.post(`/jpi/dcim/alarm/handler/unconcern?alarm_id=${alarm_id}`)\n}\n\n// Confirm alarm\nexport function confirm(data) {\n  return request.post('/jpi/dcim/alarm/handler/confirm', data)\n}\n\n// Confirm alarm by subject\nexport function targetConfirm(data) {\n  return request.post('/jpi/dcim/alarm/handler/target/confirm', data)\n}\n\n// Reason type dictionary\nexport function reasonDict() {\n  return request.get('/jpi/dcim/alarm/handler/reason/type')\n}\n\n// Query reason\nexport function reasonAlarm(alarm_id) {\n  return request.get(`/jpi/dcim/alarm/handler/reason?alarm_id=${alarm_id}`)\n}\n\n// Update reason\nexport function updateReason(data) {\n  return request.post('/jpi/dcim/alarm/handler/update/reason', data)\n}\n\n// Update reason for the alarm subject\nexport function updateTargetReason(data) {\n  return request.post('/jpi/dcim/alarm/handler/target/update/reason', data)\n}\n\n// Fuzzy query based on data center and keyword\nexport function unionSearch(params) {\n  return request.get('/jpi/dcim/device/union', { params })\n}\n\nexport function unionSearchCabinets(params) {\n  params = Object.assign(params, { search_type: 1 })\n  return request.get('/jpi/dcim/cabinet/union', { params })\n}\n\n// Get device list\nexport function getDevices(params) {\n  return request.get('/jpi/dcim/device/deviceList', { params })\n}\n\n// Get cabinet list\nexport function getCabinets(params) {\n  return request.get('/jpi/dcim/cabinet/cabinets', { params })\n}\n\n// Get device details\nexport function getDeviceDetail(id) {\n  return request.get(`/jpi/dcim/device/${id}`)\n}\n// Get cabinet details\nexport function getCabinetDetail(id) {\n  return request.get(`/jpi/dcim/cabinet/${id}`)\n}\n\n// Get cabinet power distribution\nexport function getCabinetAttr(id) {\n  return request.get(`/jpi/dcim/cabinet/attr/data?limit=0&offset=0&cabinet_id=${id}`)\n}\n\n// Trigger emergency\nexport function handlerEmer(alarm_id) {\n  return request.post(`/jpi/dcim/alarm/handler/emergency?alarm_id=${alarm_id}`)\n}\n\n// Get the list of affected customers\nexport function influenceCustomer(alarm_id) {\n  return request.get(`/jpi/dcim/alarm/influence/customer?alarm_id=${alarm_id}`)\n}\n\n// Get plan type\nexport function getPlanType() {\n  return request.get('/jpi/dcim/alarm/plan/type')\n}\n\n// One-click incident handling\nexport function handleIncident(alarm_id) {\n  return request.post(`/jpi/dcim/alarm/handler/incident?alarmId=${alarm_id}`)\n}\n\n// Batch close recovered alarms\nexport function batchCloseRecovered(data) {\n  return request.post('/jpi/dcim/alarm/handler/confirm/recovered/batch', data)\n}\n\n// Batch close unrecovered alarms\nexport function batchCloseUnRecovered(data) {\n  return request.post('/jpi/dcim/alarm/handler/confirm/unrecovered/batch', data)\n}\n\n// Batch close message alarms\nexport function batchCloseMessage(data) {\n  return request.post('/jpi/dcim/alarm/handler/confirm/message/batch', data)\n}\n\n// Get emergency list\nexport function emergencyAlarm() {\n  return request.get('/jpi/dcim/alarm/handler/emergency/alarm')\n}\n\n// Simulate alarm recovery\nexport function simulateRecover(alarm_id) {\n  return request.post(`/jpi/dcim/alarm/handler/simulate/recover?alarm_id=${alarm_id}`)\n}\n\n// Get alarm operation log\nexport function getLog(alarm_id) {\n  return request.get(`/jpi/dcim/alarm/log?alarm_id=${alarm_id}`)\n}\n\n// Get emergency list\nexport function getEmerList() {\n  return request.get('/jpi/dcim/alarm/handler/emergency/list')\n}\n\n// Query current AI control status\nexport function aiStatus() { // Production Changshu No. 2 is 141\n  return request.get(`/jpi/dcim/ai_control/status?dc_id=${141}`)\n}\n\n// Change AI control mode\nexport function changeAiStatus(data) {\n  return request.post('/jpi/dcim/ai_control/change', data)\n}\n\n// Query old alarm count\nexport function oldAlarmCount() {\n  return request.get('/jpi/dcim/alarm/handler/old/alarm_count')\n}\n\n// Get the number of unconfirmed alarms\nexport function oldUnconfirmCount() {\n  return request.get('/jpi/dcim/alarm/handler/old/unConfirm_count')\n}\n\n// Normal notification\nexport function sendNotice(data) {\n  return request.post('/jpi/dcim/alarm/handler/normal/notice', data)\n}\n",
  "details": [
    {
      "lineNumber": 2,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "// 根据数据中心获取位置信息",
      "translatedText": "// Get location information based on data center"
    },
    {
      "lineNumber": 8,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 查询实例某个等级的规则详情（含翻译）\n * @returns\n */",
      "translatedText": "/**\n * Query rule details for a specific level of an instance (including translations)\n * @returns\n */"
    },
    {
      "lineNumber": 15,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 查询告警历程\n * @returns\n */",
      "translatedText": "/**\n * Query alarm history\n * @returns\n */"
    },
    {
      "lineNumber": 21,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 获取设备的运行参数实例列表\n * @returns\n */",
      "translatedText": "/**\n * Get the runtime parameter instance list of the device\n * @returns\n */"
    },
    {
      "lineNumber": 28,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 查询机柜指标实例列表\n * @returns\n */",
      "translatedText": "/**\n * Query the indicator instance list of the cabinet\n * @returns\n */"
    },
    {
      "lineNumber": 35,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 获取告警设备最新运行情况\n * @returns\n */",
      "translatedText": "/**\n * Get the latest running status of the alarm device\n * @returns\n */"
    },
    {
      "lineNumber": 42,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 获取Event模板排障指引\n * @returns\n */",
      "translatedText": "/**\n * Get Event template troubleshooting guide\n * @returns\n */"
    },
    {
      "lineNumber": 49,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "// 关注告警",
      "translatedText": "// Focus alarm"
    },
    {
      "lineNumber": 53,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "// 取消关注告警",
      "translatedText": "// Unfocus alarm"
    },
    {
      "lineNumber": 57,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "// 确认告警",
      "translatedText": "// Confirm alarm"
    }
  ]
}
```

### DEMOS2

#### Input:

```typescript
import request from '@/plugins/axios'

/**
 * 查询机柜报表列表
 * @param {*} params
 * @returns
 */
export function getResultList(params) {
  return request.get('/jpi/dcim/cabinet/attr/valid/results', { params })
}

// 获取机柜配电检测结果字典
export function validResultDict() {
  return request.get('/jpi/dcim/dict/cabinet/attr/valid/results')
}

// 机柜配电导出
export function exportResult(params) {
  return request.get('/jpi/dcim/cabinet/attr/valid/results/export', { params, responseType: 'blob' })
}

// 趋势图
export function trendency(params) {
  return request.get('/jpi/dcim/cabinet/attr/valid/data/tendency', { params })
}
```

#### Output:

```json
{
  "translatedCode": "import request from '@/plugins/axios'\n\n/**\n * Query cabinet report list\n * @param {*} params\n * @returns\n */\nexport function getResultList(params) {\n  return request.get('/jpi/dcim/cabinet/attr/valid/results', { params })\n}\n\n// Get cabinet power distribution check result dictionary\nexport function validResultDict() {\n  return request.get('/jpi/dcim/dict/cabinet/attr/valid/results')\n}\n\n// Cabinet power distribution export\nexport function exportResult(params) {\n  return request.get('/jpi/dcim/cabinet/attr/valid/results/export', { params, responseType: 'blob' })\n}\n\n// Trend chart\nexport function trendency(params) {\n  return request.get('/jpi/dcim/cabinet/attr/valid/data/tendency', { params })\n}",
  "details": [
    {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "查询机柜报表列表",
      "translatedText": "Query cabinet report list"
    },
    {
      "lineNumber": 11,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "获取机柜配电检测结果字典",
      "translatedText": "Get cabinet power distribution check result dictionary"
    },
    {
      "lineNumber": 15,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜配电导出",
      "translatedText": "Cabinet power distribution export"
    },
    {
      "lineNumber": 19,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "趋势图",
      "translatedText": "Trend chart"
    }
  ]
}
```

### DEMOS3

#### Input:

```typescript
/**
* DcDTO
*/
export interface DcDTO {
  affect_dc_id?: number;
  affect_level?: string;
  dc_abbr?: string;
  dc_code?: string;
  dc_gid?: number;
  dc_gname?: string;
  dc_id?: number;
  dc_name?: string;
  dc_op_type?: number;
  dc_order_by?: number;
  dc_short_name?: string;
  dc_status?: number;
  dc_type?: number;
  free_trade_zone?: number;
  level?: string;
  region_id?: number;
  rid?: number;
}

/**
* ParkTDTO
*/
export interface ParkTDTO {
  dc_name_alias?: string;
  park_gid?: number;
  park_id?: number;
  park_name?: string;
  region_gid?: number;
  region_id?: number;
  region_name?: string;
  region_order_by?: number;
}

/**
 * UserDcTreeByAuthDTO
 */
export interface authModal {
  dc_list?: DcDTO[];
  function_code?: string;
  function_id?: number;
  function_name?: string;
  function_parent?: number;
  function_type?: number;
  function_type_name?: string;
  order_by?: number;
  region_list?: ParkTDTO[];
}
```

#### Output:

```json
{
  "translatedCode": "/**\n* DcDTO\n*/\nexport interface DcDTO {\n  affect_dc_id?: number;\n  affect_level?: string;\n  dc_abbr?: string;\n  dc_code?: string;\n  dc_gid?: number;\n  dc_gname?: string;\n  dc_id?: number;\n  dc_name?: string;\n  dc_op_type?: number;\n  dc_order_by?: number;\n  dc_short_name?: string;\n  dc_status?: number;\n  dc_type?: number;\n  free_trade_zone?: number;\n  level?: string;\n  region_id?: number;\n  rid?: number;\n}\n\n/**\n* ParkTDTO\n*/\nexport interface ParkTDTO {\n  dc_name_alias?: string;\n  park_gid?: number;\n  park_id?: number;\n  park_name?: string;\n  region_gid?: number;\n  region_id?: number;\n  region_name?: string;\n  region_order_by?: number;\n}\n\n/**\n * UserDcTreeByAuthDTO\n */\nexport interface authModal {\n  dc_list?: DcDTO[];\n  function_code?: string;\n  function_id?: number;\n  function_name?: string;\n  function_parent?: number;\n  function_type?: number;\n  function_type_name?: string;\n  order_by?: number;\n  region_list?: ParkTDTO[];\n}\n"
}
```

### DEMOS4

#### Input:

```typescript
export interface alarmProps {
  alarmUnique?:string | null
  breakTime?:string | null
  confirmAccount?:string | null
  confirmFlag?:number | null
  dcId?:number | null
  dcName:string | null
  emergency?:number | null
  eventId?:number | null
  eventLevelId?:number | null
  eventName?:string | null
  eventTargetCategoryId?:number | null
  eventTargetInstId?:number | null
  eventTargetInstName:string | null
  eventTargetTypeId?:number | null
  eventTargetTypeName?:string | null
  eventTypeId?:number | null
  executeId?:number | null
  id?:number
  keyFlag?:number
  lastEventLevelId?:number | null
  locationFullName?:string | null
  locationId?:number | null
  locationName?:string | null
  majorId?:number
  messageType?:number
  planFlag?:number
  reasonDescr?:string | null
  reasonTypeId?:number | null
  recoverFlag?:number
  seq?:number
  tempEventId?: number
  tempEventName?: string
  updateTime?: string
  new?: boolean
  breakValue?: any
  index?: any
  timeoutEmergency?: any
}

/**
 * AlarmDetailDateRsp
 */
export interface dataLatestProps {
  /**
   * 告警详情id
   */
  alarm_detail_id?: number;
  /**
   * 告警id
   */
  alarm_id?: number;
  /**
   * 精度
   */
  decimal_digit?: number;
  /**
   * 设备id
   */
  device_id?: number;
  /**
   * 设备名
   */
  device_name?: string;
  /**
   * 枚举值名称
   */
  enum_name?: string;
  /**
   * 枚举类型code
   */
  enum_type_code?: string;
  /**
   * 枚举值code
   */
  enum_value_code?: string;
  /**
   * 展示值
   */
  show_value?: string;
  /**
   * 运行参数id
   */
  sys_attr_id?: number;
  /**
   * 运行参数名
   */
  sys_attr_name?: string;
  /**
   * 单位
   */
  unit?: string;
  /**
   * 采集值
   */
  value?: number;
}

/**
 * StdTempEventDebugGuide
 */
export interface debugGuideProps {
  /**
   * 排障指引内容
   */
  content?: string;
  /**
   * 主键
   */
  id?: number;
  /**
   * 排序
   */
  order_by?: number;
  /**
   * event模板id
   */
  temp_event_id?: number;
}

/**
 * EventInstRuleTransRsp
 */
export interface translationProps {
  /**
   * 数据中心id
   */
  dc_id?: number;
  /**
   * event实例id
   */
  event_id?: number;
  /**
   * event等级id
   */
  event_level_id?: number;
  /**
   * 触发规则
   */
  fire_rule?: string;
  /**
   * 触发规则（翻译后）
   */
  fire_rule_show?: string;
  /**
   * 规则实例id
   */
  id?: number;
  /**
   * 恢复规则
   */
  recovery_rule?: string;
  /**
   * 恢复规则（翻译后）
   */
  recovery_rule_show?: string;
  /**
   * 规则描述
   */
  temp_event_rule_descr?: string;
  /**
   * 规则模板id
   */
  temp_event_rule_id?: number;
  /**
   * 规则模板名
   */
  temp_event_rule_name?: string;
  /**
   * 触发规则模板
   */
  temp_fire_rule?: string;
  /**
   * 恢复规则模板
   */
  temp_recovery_rule?: string;
  fire_delay?: number;
  recovery_delay?: number;
}

// 计划单
export interface planProps {
  dcId?: number
  dcName?: string
  endTime?: string
  id?: number
  planNo?: string
  planTargetList?: any
  planTitle?:string
  planTypeId?:number
  planTypeName?:string
  startTime?:string
  status?:number
}

/**
* AttrDataProps
*/
export interface attrDataProps {
  /**
   * 告警详情id
   */
  alarm_detail_id?: number;
  /**
   * 告警id
   */
  alarm_id?: number;
  /**
   * 精度
   */
  decimal_digit?: number;
  /**
   * 设备id
   */
  device_id?: number;
  /**
   * 设备名
   */
  device_name?: string;
  /**
   * 枚举值名称
   */
  enum_name?: string;
  /**
   * 枚举类型code
   */
  enum_type_code?: string;
  /**
   * 枚举值code
   */
  enum_value_code?: string;
  /**
   * 展示值
   */
  show_value?: string;
  /**
   * 运行参数id
   */
  sys_attr_id?: number;
  /**
   * 运行参数名
   */
  sys_attr_name?: string;
  /**
   * 单位
   */
  unit?: string;
  /**
   * 采集值
   */
  value?: number;
}

/**
 * AlarmProcessDetailRsp
 */
export interface processProps {
  /**
   * 运行参数值
   */
  attr_data?: attrDataProps[];
  /**
   * 发生时间
   */
  break_time?: Date;
  /**
   * 告警详情id
   */
  detail_id?: number;
  /**
   * event实例id
   */
  event_id?: number;
  /**
   * event等级code
   */
  event_level_code?: string;
  /**
   * event等级id
   */
  event_level_id?: number;
  /**
   * event等级名
   */
  event_level_name?: string;
  /**
   * 消息类型code
   */
  message_type_code?: string;
  /**
   * 消息类型id
   */
  message_type_id?: number;
  /**
   * 消息类型名
   */
  message_type_name?: string;
}
```

#### Output:

```json
{
  "translatedCode": "export interface alarmProps {\n  alarmUnique?:string | null\n  breakTime?:string | null\n  confirmAccount?:string | null\n  confirmFlag?:number | null\n  dcId?:number | null\n  dcName:string | null\n  emergency?:number | null\n  eventId?:number | null\n  eventLevelId?:number | null\n  eventName?:string | null\n  eventTargetCategoryId?:number | null\n  eventTargetInstId?:number | null\n  eventTargetInstName:string | null\n  eventTargetTypeId?:number | null\n  eventTargetTypeName?:string | null\n  eventTypeId?:number | null\n  executeId?:number | null\n  id?:number\n  keyFlag?:number\n  lastEventLevelId?:number | null\n  locationFullName?:string | null\n  locationId?:number | null\n  locationName?:string | null\n  majorId?:number\n  messageType?:number\n  planFlag?:number\n  reasonDescr?:string | null\n  reasonTypeId?:number | null\n  recoverFlag?:number\n  seq?:number\n  tempEventId?: number\n  tempEventName?: string\n  updateTime?: string\n  new?: boolean\n  breakValue?: any\n  index?: any\n  timeoutEmergency?: any\n}\n\n/**\n * AlarmDetailDateRsp\n */\nexport interface dataLatestProps {\n  /**\n   * Alarm Detail ID\n   */\n  alarm_detail_id?: number;\n  /**\n   * Alarm ID\n   */\n  alarm_id?: number;\n  /**\n   * Precision\n   */\n  decimal_digit?: number;\n  /**\n   * Device ID\n   */\n  device_id?: number;\n  /**\n   * Device Name\n   */\n  device_name?: string;\n  /**\n   * Enum Value Name\n   */\n  enum_name?: string;\n  /**\n   * Enum Type Code\n   */\n  enum_type_code?: string;\n  /**\n   * Enum Value Code\n   */\n  enum_value_code?: string;\n  /**\n   * Display Value\n   */\n  show_value?: string;\n  /**\n   * System Attribute ID\n   */\n  sys_attr_id?: number;\n  /**\n   * System Attribute Name\n   */\n  sys_attr_name?: string;\n  /**\n   * Unit\n   */\n  unit?: string;\n  /**\n   * Collected Value\n   */\n  value?: number;\n}\n\n/**\n * StdTempEventDebugGuide\n */\nexport interface debugGuideProps {\n  /**\n   * Troubleshooting Guide Content\n   */\n  content?: string;\n  /**\n   * Primary Key\n   */\n  id?: number;\n  /**\n   * Sort Order\n   */\n  order_by?: number;\n  /**\n   * Event Template ID\n   */\n  temp_event_id?: number;\n}\n\n/**\n * EventInstRuleTransRsp\n */\nexport interface translationProps {\n  /**\n   * Data Center ID\n   */\n  dc_id?: number;\n  /**\n   * Event Instance ID\n   */\n  event_id?: number;\n  /**\n   * Event Level ID\n   */\n  event_level_id?: number;\n  /**\n   * Trigger Rule\n   */\n  fire_rule?: string;\n  /**\n   * Translated Trigger Rule\n   */\n  fire_rule_show?: string;\n  /**\n   * Rule Instance ID\n   */\n  id?: number;\n  /**\n   * Recovery Rule\n   */\n  recovery_rule?: string;\n  /**\n   * Translated Recovery Rule\n   */\n  recovery_rule_show?: string;\n  /**\n   * Rule Description\n   */\n  temp_event_rule_descr?: string;\n  /**\n   * Rule Template ID\n   */\n  temp_event_rule_id?: number;\n  /**\n   * Rule Template Name\n   */\n  temp_event_rule_name?: string;\n  /**\n   * Trigger Rule Template\n   */\n  temp_fire_rule?: string;\n  /**\n   * Recovery Rule Template\n   */\n  temp_recovery_rule?: string;\n  fire_delay?: number;\n  recovery_delay?: number;\n}\n\n// Plan Order\nexport interface planProps {\n  dcId?: number\n  dcName?: string\n  endTime?: string\n  id?: number\n  planNo?: string\n  planTargetList?: any\n  planTitle?:string\n  planTypeId?:number\n  planTypeName?:string\n  startTime?:string\n  status?:number\n}\n\n/**\n* AttrDataProps\n */\nexport interface attrDataProps {\n  /**\n   * Alarm Detail ID\n   */\n  alarm_detail_id?: number;\n  /**\n   * Alarm ID\n   */\n  alarm_id?: number;\n  /**\n   * Precision\n   */\n  decimal_digit?: number;\n  /**\n   * Device ID\n   */\n  device_id?: number;\n  /**\n   * Device Name\n   */\n  device_name?: string;\n  /**\n   * Enum Value Name\n   */\n  enum_name?: string;\n  /**\n   * Enum Type Code\n   */\n  enum_type_code?: string;\n  /**\n   * Enum Value Code\n   */\n  enum_value_code?: string;\n  /**\n   * Display Value\n   */\n  show_value?: string;\n  /**\n   * System Attribute ID\n   */\n  sys_attr_id?: number;\n  /**\n   * System Attribute Name\n   */\n  sys_attr_name?: string;\n  /**\n   * Unit\n   */\n  unit?: string;\n  /**\n   * Collected Value\n   */\n  value?: number;\n}\n\n/**\n * AlarmProcessDetailRsp\n */\nexport interface processProps {\n  /**\n   * Operating Parameter Value\n   */\n  attr_data?: attrDataProps[];\n  /**\n   * Occurrence Time\n   */\n  break_time?: Date;\n  /**\n   * Alarm Detail ID\n   */\n  detail_id?: number;\n  /**\n   * Event Instance ID\n   */\n  event_id?: number;\n  /**\n   * Event Level Code\n   */\n  event_level_code?: string;\n  /**\n   * Event Level ID\n   */\n  event_level_id?: number;\n  /**\n   * Event Level Name\n   */\n  event_level_name?: string;\n  /**\n   * Message Type Code\n   */\n  message_type_code?: string;\n  /**\n   * Message Type ID\n   */\n  message_type_id?: number;\n  /**\n   * Message Type Name\n   */\n  message_type_name?: string;\n}",
  "details": [
    {
      "lineNumber": 16,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 告警详情id\n   */",
      "translatedText": "/**\n   * Alarm Detail ID\n   */"
    },
    {
      "lineNumber": 18,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 告警id\n   */",
      "translatedText": "/**\n   * Alarm ID\n   */"
    },
    {
      "lineNumber": 20,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 精度\n   */",
      "translatedText": "/**\n   * Precision\n   */"
    },
    {
      "lineNumber": 22,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 设备id\n   */",
      "translatedText": "/**\n   * Device ID\n   */"
    },
    {
      "lineNumber": 24,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 设备名\n   */",
      "translatedText": "/**\n   * Device Name\n   */"
    },
    {
      "lineNumber": 26,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 枚举值名称\n   */",
      "translatedText": "/**\n   * Enum Value Name\n   */"
    },
    {
      "lineNumber": 28,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 枚举类型code\n   */",
      "translatedText": "/**\n   * Enum Type Code\n   */"
    },
    {
      "lineNumber": 30,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 枚举值code\n   */",
      "translatedText": "/**\n   * Enum Value Code\n   */"
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 展示值\n   */",
      "translatedText": "/**\n   * Display Value\n   */"
    },
    {
      "lineNumber": 34,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 运行参数id\n   */",
      "translatedText": "/**\n   * System Attribute ID\n   */"
    },
    {
      "lineNumber": 36,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 运行参数名\n   */",
      "translatedText": "/**\n   * System Attribute Name\n   */"
    },
    {
      "lineNumber": 38,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 单位\n   */",
      "translatedText": "/**\n   * Unit\n   */"
    },
    {
      "lineNumber": 40,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n   * 采集值\n   */",
      "translatedText": "/**\n   * Collected Value\n   */"
    }
  ]
}
```

### DEMOS5

#### Input:

```typescript
/**
* RecordCabinetDailyOverview
*/
export interface RecordCabinetDailyOverview {
  abnormal_cnt?: number;
  collect_time?: Date;
  commit_account?: string;
  dc_id?: number;
  dc_name?: string;
  dc_order?: number;
  fixed_cnt?: number;
  id?: number;
  status_id?: number;
  update_account?: string;
  update_staff?: string;
  update_time?: Date;
}
/**
 * PageResult«RecordCabinetDailyOverview»
 */
export interface OverviewProps {
  /**
   * 记录列表
   */
  data?: RecordCabinetDailyOverview[];
  /**
   * 记录总数
   */
  total?: number;
}
/**
* RecordCabinetDaily
*/

/**
 * RecordCabinetDaily
 */
export interface RecordCabinetDaily {
  abnormal_list?: any;
  indi_codes?: any;
  info_codes?: any;
  /**
   * 甲方客户id
   */
  a_customer_id?: number;
  /**
   * 甲方客户名
   */
  a_customer_name?: string;
  /**
   * 异常flag
   */
  abnormal_flag?: number;
  /**
   * 配电数量
   */
  attr_cnt?: number;
  /**
   * 机柜id
   */
  cabinet_id?: number;
  /**
   * 机柜名称
   */
  cabinet_name?: string;
  /**
   * 合约电力
   */
  cntrct_elec_kva?: number;
  /**
   * 采集时间
   */
  collect_time?: Date;
  /**
   * 功率确认值
   */
  confirm_power_kw?: number;
  /**
   * 功率确认值环比
   */
  confirm_power_kw_increment_ratio?: number;
  /**
   * 电度确认值
   */
  confirm_power_kwh?: number;
  /**
   * 电度确认值环比
   */
  confirm_power_kwh_increment_ratio?: number;
  /**
   * 上下电状态
   */
  elect_status?: number;
  /**
   * 最终客户id
   */
  end_customer_id?: number;
  /**
   * 最终客户名
   */
  end_customer_name?: string;
  /**
   * 一天的电度起始值
   */
  first_power_kwh?: number;
  /**
   * 处理flag
   */
  fixed_flag?: number;
  /**
   * 功率修正值
   */
  fixed_power_kw?: number;
  /**
   * 功率修正值环比
   */
  fixed_power_kw_increment_ratio?: number;
  /**
   * 电度修正值
   */
  fixed_power_kwh?: number;
  /**
   * 功率修正值环比
   */
  fixed_power_kwh_increment_ratio?: number;
  id?: number;
  /**
   * 一天的电度结束值
   */
  last_power_kwh?: number;
  /**
   * 功率原始值
   */
  original_power_kw?: number;
  /**
   * 功率原始值环比
   */
  original_power_kw_increment_ratio?: number;
  /**
   * 电度原始值
   */
  original_power_kwh?: number;
  /**
   * 电度原始值环比
   */
  original_power_kwh_increment_ratio?: number;
  /**
   * 概览id
   */
  overview_id?: number;
  /**
   * 项目编号
   */
  project_code?: string;
  /**
   * 机房id
   */
  room_id?: number;
  /**
   * 机房名
   */
  room_name?: string;
  /**
   * 已修改flag
   */
  update_flag?: number;
  /**
   * 昨日功率（环比参考值）
   */
  yesterday_power_kw?: number;
  /**
   * 昨日电度（环比参考值）
   */
  yesterday_power_kwh?: number;
}

/**
 * PageResult«RecordCabinetDaily»
 */
export interface detailProps {
  /**
   * 记录列表
   */
  data?: RecordCabinetDaily[];
  /**
   * 记录总数
   */
  total?: number;
}
```

#### Output:

```json
{
  "translatedCode": "/**\n* RecordCabinetDailyOverview\n*/\nexport interface RecordCabinetDailyOverview {\n  abnormal_cnt?: number;\n  collect_time?: Date;\n  commit_account?: string;\n  dc_id?: number;\n  dc_name?: string;\n  dc_order?: number;\n  fixed_cnt?: number;\n  id?: number;\n  status_id?: number;\n  update_account?: string;\n  update_staff?: string;\n  update_time?: Date;\n}\n/**\n * PageResult«RecordCabinetDailyOverview»\n */\nexport interface OverviewProps {\n  /**\n   * Record list\n   */\n  data?: RecordCabinetDailyOverview[];\n  /**\n   * Total records\n   */\n  total?: number;\n}\n/**\n* RecordCabinetDaily\n*/\n\n/**\n * RecordCabinetDaily\n */\nexport interface RecordCabinetDaily {\n  abnormal_list?: any;\n  indi_codes?: any;\n  info_codes?: any;\n  /**\n   * Party A customer ID\n   */\n  a_customer_id?: number;\n  /**\n   * Party A customer name\n   */\n  a_customer_name?: string;\n  /**\n   * Abnormal flag\n   */\n  abnormal_flag?: number;\n  /**\n   * Power distribution quantity\n   */\n  attr_cnt?: number;\n  /**\n   * Cabinet ID\n   */\n  cabinet_id?: number;\n  /**\n   * Cabinet name\n   */\n  cabinet_name?: string;\n  /**\n   * Contract power\n   */\n  cntrct_elec_kva?: number;\n  /**\n   * Collection time\n   */\n  collect_time?: Date;\n  /**\n   * Confirmed power value\n   */\n  confirm_power_kw?: number;\n  /**\n   * Confirmed power value increment ratio\n   */\n  confirm_power_kw_increment_ratio?: number;\n  /**\n   * Confirmed power kWh\n   */\n  confirm_power_kwh?: number;\n  /**\n   * Confirmed power kWh increment ratio\n   */\n  confirm_power_kwh_increment_ratio?: number;\n  /**\n   * Power-on/off status\n   */\n  elect_status?: number;\n  /**\n   * End customer ID\n   */\n  end_customer_id?: number;\n  /**\n   * End customer name\n   */\n  end_customer_name?: string;\n  /**\n   * Starting kWh of the day\n   */\n  first_power_kwh?: number;\n  /**\n   * Fixed flag\n   */\n  fixed_flag?: number;\n  /**\n   * Corrected power value\n   */\n  fixed_power_kw?: number;\n  /**\n   * Corrected power value increment ratio\n   */\n  fixed_power_kw_increment_ratio?: number;\n  /**\n   * Corrected kWh\n   */\n  fixed_power_kwh?: number;\n  /**\n   * Corrected kWh increment ratio\n   */\n  fixed_power_kwh_increment_ratio?: number;\n  id?: number;\n  /**\n   * Ending kWh of the day\n   */\n  last_power_kwh?: number;\n  /**\n   * Original power value\n   */\n  original_power_kw?: number;\n  /**\n   * Original power value increment ratio\n   */\n  original_power_kw_increment_ratio?: number;\n  /**\n   * Original kWh\n   */\n  original_power_kwh?: number;\n  /**\n   * Original kWh increment ratio\n   */\n  original_power_kwh_increment_ratio?: number;\n  /**\n   * Overview ID\n   */\n  overview_id?: number;\n  /**\n   * Project code\n   */\n  project_code?: string;\n  /**\n   * Room ID\n   */\n  room_id?: number;\n  /**\n   * Room name\n   */\n  room_name?: string;\n  /**\n   * Update flag\n   */\n  update_flag?: number;\n  /**\n   * Yesterday's power (increment reference value)\n   */\n  yesterday_power_kw?: number;\n  /**\n   * Yesterday's kWh (increment reference value)\n   */\n  yesterday_power_kwh?: number;\n}\n\n/**\n * PageResult«RecordCabinetDaily»\n */\nexport interface detailProps {\n  /**\n   * Record list\n   */\n  data?: RecordCabinetDaily[];\n  /**\n   * Total records\n   */\n  total?: number;\n}",
  "details": [
    {
      "lineNumber": 16,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 记录列表",
      "translatedText": " * Record list"
    },
    {
      "lineNumber": 19,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 记录总数",
      "translatedText": " * Total records"
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 甲方客户id",
      "translatedText": " * Party A customer ID"
    },
    {
      "lineNumber": 35,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 甲方客户名",
      "translatedText": " * Party A customer name"
    },
    {
      "lineNumber": 38,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 异常flag",
      "translatedText": " * Abnormal flag"
    },
    {
      "lineNumber": 41,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 配电数量",
      "translatedText": " * Power distribution quantity"
    },
    {
      "lineNumber": 44,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 机柜id",
      "translatedText": " * Cabinet ID"
    },
    {
      "lineNumber": 47,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 机柜名称",
      "translatedText": " * Cabinet name"
    },
    {
      "lineNumber": 50,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 合约电力",
      "translatedText": " * Contract power"
    },
    {
      "lineNumber": 53,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " * 采集时间",
      "translatedText": " * Collection time"
    }
  ]
}
```

### DEMOS6

#### Input:

```typescript
// 权限控制模块
import { getDcFuncs } from '@/api/dcim'

export interface dcsProps {
  region_id: number,
  region_gid: number,
  region_name: string,
  region_order_by:number,
  country_code:string,
  park_id: number,
  park_gid: number,
  park_name: string,
  park_order_by: number,
  dc_id: number,
  dc_gid: number,
  dc_name: string,
  dc_order_by: number
  dc_short_name: string
  children: any
}

export interface authProps {
  function_id: number,
  function_code:string,
  name: string,
  order_by: number,
  parent_id: number,
  type: number
}

export let authorities: authProps[] = []
/**
 * 初始化权限信息 获取所有权限
 * @returns {Promise<void>}
 */
export async function init(): Promise<void> {
  const { data } = await getDcFuncs()
  authorities = data
}
/**
 * 是否有某个功能权限
 * @param code
 * @param functions  先根据dc_id获取有权限的function，如果传了function则代表要根据dc判断
 * @returns {boolean}
 */
export function hasAuthority(code: string, functions?: authProps[]): boolean {
  let findResult
  if (functions) {
    findResult = functions.find(k => {
      const cropCode = k.function_code.startsWith('dcim.menu') ? k.function_code.replace('dcim.menu.', '') : k.function_code.replace('dcim.function.', '')
      return k.function_code === code || cropCode === code
    })
  } else {
    findResult = authorities.find(k => {
      const cropCode = k.function_code.startsWith('dcim.menu') ? k.function_code.replace('dcim.menu.', '') : k.function_code.replace('dcim.function.', '')
      return k.function_code === code || cropCode === code
    })
  }
  return !!findResult
}
```

#### Output:

```json
{
  "translatedCode": "// Permission control module\nimport { getDcFuncs } from '@/api/dcim'\n\nexport interface dcsProps {\n  region_id: number,\n  region_gid: number,\n  region_name: string,\n  region_order_by:number,\n  country_code:string,\n  park_id: number,\n  park_gid: number,\n  park_name: string,\n  park_order_by: number,\n  dc_id: number,\n  dc_gid: number,\n  dc_name: string,\n  dc_order_by: number\n  dc_short_name: string\n  children: any\n}\n\nexport interface authProps {\n  function_id: number,\n  function_code:string,\n  name: string,\n  order_by: number,\n  parent_id: number,\n  type: number\n}\n\nexport let authorities: authProps[] = []\n/**\n * Initialize permission information and get all permissions\n * @returns {Promise<void>}\n */\nexport async function init(): Promise<void> {\n  const { data } = await getDcFuncs()\n  authorities = data\n}\n/**\n * Check if there is a specific function permission\n * @param code\n * @param functions  First get the functions with permission based on dc_id, if a function is passed, it means checking by dc\n * @returns {boolean}\n */\nexport function hasAuthority(code: string, functions?: authProps[]): boolean {\n  let findResult\n  if (functions) {\n    findResult = functions.find(k => {\n      const cropCode = k.function_code.startsWith('dcim.menu') ? k.function_code.replace('dcim.menu.', '') : k.function_code.replace('dcim.function.', '')\n      return k.function_code === code || cropCode === code\n    })\n  } else {\n    findResult = authorities.find(k => {\n      const cropCode = k.function_code.startsWith('dcim.menu') ? k.function_code.replace('dcim.menu.', '') : k.function_code.replace('dcim.function.', '')\n      return k.function_code === code || cropCode === code\n    })\n  }\n  return !!findResult\n}",
  "details": [
    {
      "lineNumber": 1,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "// 权限控制模块",
      "translatedText": "// Permission control module"
    },
    {
      "lineNumber": 48,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 初始化权限信息 获取所有权限\n * @returns {Promise<void>}\n */",
      "translatedText": "/**\n * Initialize permission information and get all permissions\n * @returns {Promise<void>}\n */"
    },
    {
      "lineNumber": 56,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 是否有某个功能权限\n * @param code\n * @param functions  先根据dc_id获取有权限的function，如果传了function则代表要根据dc判断\n * @returns {boolean}\n */",
      "translatedText": "/**\n * Check if there is a specific function permission\n * @param code\n * @param functions  First get the functions with permission based on dc_id, if a function is passed, it means checking by dc\n * @returns {boolean}\n */"
    }
  ]
}
```

### DEMOS7

#### Input:

```typescript
/**
* TempEventRuleScopeRsp
*/
export interface TempEventRuleScopeRsp {
  /**
   * 适用范围的gid
   */
  scope?: number;
  /**
   * 适用范围名
   */
  scope_name?: string;
}
/**
* TempEventRuleDetailRsp
*/
export interface TempEventRuleDetailRsp {
  /**
   * 发生延迟
   */
  fire_delay?: number;
  /**
   * 发生规则
   */
  fire_rule?: string;
  /**
   * 恢复延迟
   */
  recovery_delay?: number;
  /**
   * 恢复规则
   */
  recovery_rule?: string;
  /**
   * 适用范围
   */
  scopes?: TempEventRuleScopeRsp[];
  /**
   * 规则模板详情id
   */
  temp_rule_detail_id?: number;
}
/**
* TempEventRuleRsp
*/
export interface TempEventRuleRsp {
  /**
   * 描述
   */
  descr?: string;
  /**
   * 规则模板名
   */
  name?: string;
  /**
   * 优先级
   */
  priority?: number;
  /**
   * 规则详情
   */
  rule_details?: TempEventRuleDetailRsp[];
  /**
   * 是否特殊规则，1：是，0：否
   */
  special_flag?: number;
  /**
   * 规则模板id
   */
  temp_rule_id?: number;
}
/**
 * TempEventRuleLevelRsp
 */
export interface ruleProps {
  /**
   * event等级code
   */
  event_level_code?: string;
  /**
   * event等级id
   */
  event_level_id?: number;
  /**
   * event等级名
   */
  event_level_name?: string;
  /**
   * 等级下的规则模板
   */
  rules?: TempEventRuleRsp[];

  enable_status?: any;
}
```

#### Output:

```json
{
  "translatedCode": "/**\n * TempEventRuleScopeRsp\n */\nexport interface TempEventRuleScopeRsp {\n  /**\n   * GID of the applicable scope\n   */\n  scope?: number;\n  /**\n   * Name of the applicable scope\n   */\n  scope_name?: string;\n}\n/**\n * TempEventRuleDetailRsp\n */\nexport interface TempEventRuleDetailRsp {\n  /**\n   * Fire delay\n   */\n  fire_delay?: number;\n  /**\n   * Fire rule\n   */\n  fire_rule?: string;\n  /**\n   * Recovery delay\n   */\n  recovery_delay?: number;\n  /**\n   * Recovery rule\n   */\n  recovery_rule?: string;\n  /**\n   * Applicable scopes\n   */\n  scopes?: TempEventRuleScopeRsp[];\n  /**\n   * Rule template detail ID\n   */\n  temp_rule_detail_id?: number;\n}\n/**\n * TempEventRuleRsp\n */\nexport interface TempEventRuleRsp {\n  /**\n   * Description\n   */\n  descr?: string;\n  /**\n   * Rule template name\n   */\n  name?: string;\n  /**\n   * Priority\n   */\n  priority?: number;\n  /**\n   * Rule details\n   */\n  rule_details?: TempEventRuleDetailRsp[];\n  /**\n   * Special rule flag, 1: Yes, 0: No\n   */\n  special_flag?: number;\n  /**\n   * Rule template ID\n   */\n  temp_rule_id?: number;\n}\n/**\n * TempEventRuleLevelRsp\n */\nexport interface ruleProps {\n  /**\n   * Event level code\n   */\n  event_level_code?: string;\n  /**\n   * Event level ID\n   */\n  event_level_id?: number;\n  /**\n   * Event level name\n   */\n  event_level_name?: string;\n  /**\n   * Rules under the level\n   */\n  rules?: TempEventRuleRsp[];\n\n  enable_status?: any;\n}",
  "details": [
    {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 适用范围的gid\n */",
      "translatedText": "/**\n * GID of the applicable scope\n */"
    },
    {
      "lineNumber": 7,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 适用范围名\n */",
      "translatedText": "/**\n * Name of the applicable scope\n */"
    },
    {
      "lineNumber": 12,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 发生延迟\n */",
      "translatedText": "/**\n * Fire delay\n */"
    },
    {
      "lineNumber": 15,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 发生规则\n */",
      "translatedText": "/**\n * Fire rule\n */"
    },
    {
      "lineNumber": 18,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 恢复延迟\n */",
      "translatedText": "/**\n * Recovery delay\n */"
    },
    {
      "lineNumber": 21,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 恢复规则\n */",
      "translatedText": "/**\n * Recovery rule\n */"
    },
    {
      "lineNumber": 24,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 适用范围\n */",
      "translatedText": "/**\n * Applicable scopes\n */"
    },
    {
      "lineNumber": 27,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 规则模板详情id\n */",
      "translatedText": "/**\n * Rule template detail ID\n */"
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 描述\n */",
      "translatedText": "/**\n * Description\n */"
    },
    {
      "lineNumber": 35,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 规则模板名\n */",
      "translatedText": "/**\n * Rule template name\n */"
    },
    {
      "lineNumber": 38,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 优先级\n */",
      "translatedText": "/**\n * Priority\n */"
    },
    {
      "lineNumber": 41,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 规则详情\n */",
      "translatedText": "/**\n * Rule details\n */"
    },
    {
      "lineNumber": 44,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 是否特殊规则，1：是，0：否\n */",
      "translatedText": "/**\n * Special rule flag, 1: Yes, 0: No\n */"
    },
    {
      "lineNumber": 47,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 规则模板id\n */",
      "translatedText": "/**\n * Rule template ID\n */"
    },
    {
      "lineNumber": 52,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * event等级code\n */",
      "translatedText": "/**\n * Event level code\n */"
    },
    {
      "lineNumber": 55,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * event等级id\n */",
      "translatedText": "/**\n * Event level ID\n */"
    },
    {
      "lineNumber": 58,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * event等级名\n */",
      "translatedText": "/**\n * Event level name\n */"
    },
    {
      "lineNumber": 61,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 等级下的规则模板\n */",
      "translatedText": "/**\n * Rules under the level\n */"
    }
  ]
}
```

### DEMOS8

#### Input:

```typescript
/**
* EventInst
*/
export interface EventInst {
  complete_detail?: any;
  create_time?: Date;
  event_name?: string;
  dc_id?: number;
  elec_sn?: number;
  elec_type?: number;
  event_level?: number[];
  event_rule_applied_cnt?: number;
  event_rule_apply_cnt?: number;
  event_target_category_id?: number;
  event_target_inst_id?: number;
  event_target_inst_name?: string;
  event_target_type_id?: number;
  event_target_type_name?: string;
  event_type_id?: number;
  id?: number;
  location_id?: number;
  location_name?: string;
  seq?: number;
  enable_status_id?: number;
  temp_event_id?: number;
  temp_event_name?: string;
  update_time?: Date;
  multiple_flag?: number;
  update_event_name?: string;
}

/**
 * PageResult«EventInst»
 */
export interface instProps {
  /**
   * 记录列表
   */
  data?: EventInst[];
  /**
   * 记录总数
   */
  total?: number;
}

/**
 * EventInstRule
 */
export interface instDetailProps {
  complete_flag?: number;
  dc_id?: number;
  event_id?: number;
  event_level_id?: number;
  fire_delay?: number;
  fire_rule?: string;
  id?: number;
  lock_flag?: number;
  special_flag?: number;
  recovery_delay?: number;
  recovery_rule?: string;
  temp_event_rule_descr?: string;
  temp_event_rule_id?: number;
  temp_event_rule_name?: string;
  temp_fire_rule?: string;
  temp_recovery_rule?: string;
}
```

#### Output:

```json
{
  "translatedCode": "/**\n * EventInst\n */\nexport interface EventInst {\n  complete_detail?: any;\n  create_time?: Date;\n  event_name?: string;\n  dc_id?: number;\n  elec_sn?: number;\n  elec_type?: number;\n  event_level?: number[];\n  event_rule_applied_cnt?: number;\n  event_rule_apply_cnt?: number;\n  event_target_category_id?: number;\n  event_target_inst_id?: number;\n  event_target_inst_name?: string;\n  event_target_type_id?: number;\n  event_target_type_name?: string;\n  event_type_id?: number;\n  id?: number;\n  location_id?: number;\n  location_name?: string;\n  seq?: number;\n  enable_status_id?: number;\n  temp_event_id?: number;\n  temp_event_name?: string;\n  update_time?: Date;\n  multiple_flag?: number;\n  update_event_name?: string;\n}\n\n/**\n * PageResult«EventInst»\n */\nexport interface instProps {\n  /**\n   * Record list\n   */\n  data?: EventInst[];\n  /**\n   * Total record count\n   */\n  total?: number;\n}\n\n/**\n * EventInstRule\n */\nexport interface instDetailProps {\n  complete_flag?: number;\n  dc_id?: number;\n  event_id?: number;\n  event_level_id?: number;\n  fire_delay?: number;\n  fire_rule?: string;\n  id?: number;\n  lock_flag?: number;\n  special_flag?: number;\n  recovery_delay?: number;\n  recovery_rule?: string;\n  temp_event_rule_descr?: string;\n  temp_event_rule_id?: number;\n  temp_event_rule_name?: string;\n  temp_fire_rule?: string;\n  temp_recovery_rule?: string;\n}",
  "details": [
    {
      "lineNumber": 33,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 记录列表\n */",
      "translatedText": "/**\n * Record list\n */"
    },
    {
      "lineNumber": 37,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 记录总数\n */",
      "translatedText": "/**\n * Total record count\n */"
    }
  ]
}
```

### DEMOS9

#### Input:

```typescript
import request from '@/plugins/axios'

/**
 * 获取菜单列表
 * @param account
 * @param function_type
 * @returns {*}
 */
// export function getMenus(account, function_type?) {
//   return request.get('/jpi/dcim/auth/menus', {
//     params: {
//       account,
//       function_type
//     }
//   })
// }
export function getMenus() {
  return request.get('/jpi/dcim/auth/v2/menus')
}

/**
 * 查询用户名
 */
export function myInfo(username) {
  return request.get('/oauth/token/myinfo', {
    params: {
      username
    }
  })
}

// 获取用户的权限列表
export function getDcFuncs(dc_id?) {
  return request.get('/jpi/dcim/auth/v2/dc_functions', {
    params: {
      dc_id
    }
  })
}

// 获取用户有某个权限的数据中心列表
export function getFuncDcs(function_code) {
  return request.get('/jpi/dcim/auth/v2/function_dcs', {
    params: {
      function_code
    }
  })
}
```

#### Output:

```json
{
  "translatedCode": "import request from '@/plugins/axios'\n\n/**\n * Get menu list\n * @param account\n * @param function_type\n * @returns {*}\n */\n// export function getMenus(account, function_type?) {\n//   return request.get('/jpi/dcim/auth/menus', {\n//     params: {\n//       account,\n//       function_type\n//     }\n//   })\n// }\nexport function getMenus() {\n  return request.get('/jpi/dcim/auth/v2/menus')\n}\n\n/**\n * Query username\n */\nexport function myInfo(username) {\n  return request.get('/oauth/token/myinfo', {\n    params: {\n      username\n    }\n  })\n}\n\n// Get the user's list of permissions\nexport function getDcFuncs(dc_id?) {\n  return request.get('/jpi/dcim/auth/v2/dc_functions', {\n    params: {\n      dc_id\n    }\n  })\n}\n\n// Get the list of data centers where the user has a specific permission\nexport function getFuncDcs(function_code) {\n  return request.get('/jpi/dcim/auth/v2/function_dcs', {\n    params: {\n      function_code\n    }\n  })\n}",
  "details": [
    {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "获取菜单列表",
      "translatedText": "Get menu list"
    },
    {
      "lineNumber": 22,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "查询用户名",
      "translatedText": "Query username"
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "获取用户的权限列表",
      "translatedText": "Get the user's list of permissions"
    },
    {
      "lineNumber": 41,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "获取用户有某个权限的数据中心列表",
      "translatedText": "Get the list of data centers where the user has a specific permission"
    }
  ]
}
```

### DEMOS10

#### Input:

```typescript

```

#### Output:

```json

```

### DEMOS11

#### Input:

```typescript

```

#### Output:

```json

```

### DEMOS12

#### Input:

```typescript

```

#### Output:

```json

```
