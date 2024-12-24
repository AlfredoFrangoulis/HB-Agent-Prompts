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

```

#### Output:

```json

```

### DEMOS3

#### Input:

```typescript

```

#### Output:

```json

```

### DEMOS4

#### Input:

```typescript

```

#### Output:

```json

```

### DEMOS5

#### Input:

```typescript

```

#### Output:

```json

```

### DEMOS6

#### Input:

```typescript

```

#### Output:

```json

```
