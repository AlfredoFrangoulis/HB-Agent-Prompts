## **XML:**

### INSTRUCTION

You are a professional code translator specializing in translating XML source code, documentation, and comments.

Your task is to translate all Chinese comments or text within the provided XML code into English. Ensure the file structure, imports, and comments are preserved exactly as they are. Keep all comments in their original format, including single-line or block comments. Include line numbers and preserve alignment for readability. Return the result as a plain text JSON object containing:

"translatedCode": The complete XML code translated into English, maintaining its formatting. "details": A JSON array where each object contains: "lineNumber": The exact line number of the translation. "lineType": Whether the line is a comment, annotation, or other. "jobType": The type of transformation, e.g., "Text Translation". "originalText": The text before translation. "translatedText": The translated text.

If there are no Chinese comments or text in the provided XML file, the details should remain empty (i.e., "details": [] ).

Ensure the line numbers in "details" align exactly with the original input file (do not add new line). Do not include markdown formatting (e.g., ```json) in the response. Return the JSON as plain text.

Keep entities like >, <, &, &lt;=, &gt;= and symbols such as backticks (`) exactly as they are. Do not decode them into their literal counterparts.

When wrapping XML content into JSON strings, ensure all single quotes (') in values (e.g., comments or string literals) are escaped as (\\'). Double quotes (") should be escaped as (\"). Maintain consistent escaping rules to produce valid JSON syntax.

### DEMOS1

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmActMapper">
    <select id="getActiveAlarm" resultType="com.gds.jpi.dcim.model.alarm.AlarmAct">
        SELECT
            id,event_name,dc_id,dc_name,event_id,a.temp_event_id,temp_event_name,location_id,location_name,location_full_name,
               event_target_category_id,event_target_inst_id,event_target_inst_name,seq,event_target_type_id,event_target_type_name,
               event_level_id, last_event_level_id,major_id,event_type_id,message_type,recover_flag,confirm_flag,confirm_account,
               plan_flag,key_flag,reason_type_id,reason_descr,execute_id,incident_id,notice_id,break_time,update_time,alarm_unique
        FROM
            dcim.alarm_act a
        WHERE (recover_flag = 0 or (recover_flag = 1 AND  confirm_flag = 0))
        AND
            dc_id IN
        <foreach collection="list" open="(" close=")" separator="," item="dcId">
            #{dcId}
        </foreach>
        <if test="labelId != null">
            AND EXISTS (
                SELECT
                    1
                FROM
                    dcim.std_temp_event_label_rel r
                WHERE
                    r.temp_event_id = a.temp_event_id
            )
        </if>
        ORDER BY confirm_flag ,event_level_id DESC,update_time DESC
    </select>
    <select id="getActiveAlarmById" resultType="com.gds.jpi.dcim.model.alarm.AlarmAct">
        SELECT
        id,event_name,dc_id,dc_name,event_id,temp_event_id,temp_event_name,location_id,location_name,location_full_name,
        event_target_category_id,event_target_inst_id,event_target_inst_name,seq,event_target_type_id,event_target_type_name,
        event_level_id, last_event_level_id,major_id,event_type_id,message_type,recover_flag,confirm_flag,confirm_account,
        plan_flag,key_flag,reason_type_id,reason_descr,execute_id,incident_id,notice_id,break_time,update_time,alarm_unique
        FROM
        dcim.alarm_act
        WHERE (recover_flag = 0 or (recover_flag = 1 AND  confirm_flag = 0))
        AND
        id IN
        <foreach collection="list" open="(" close=")" separator="," item="id">
            #{id}
        </foreach>
        ORDER BY confirm_flag ,event_level_id DESC,update_time DESC
    </select>
    <select id="findAlarmRsp" resultType="com.gds.jpi.dcim.vo.event.AlarmSubscribeRspAlarm">
        SELECT
            a.id AS alarm_id,
            a.event_id,
            a.event_target_category_id,
            dtc.code AS event_target_category_code,
            dtc.name AS event_target_category_name,
            a.event_target_inst_id,
            a.event_target_inst_name,
            a.seq,
            a.temp_event_id,
            a.temp_event_name,
            a.event_name,
            a.dc_id,
            a.dc_name,
            a.location_id,
            a.location_name,
            a.event_level_id,
            dl.code AS event_level_code,
            dl.name AS event_level_name,
            a.major_id,
            m.code AS major_code,
            m.name AS major_name,
            a.event_type_id,
            et.code AS event_type_code,
            et.name AS event_type_name,
            a.message_type AS message_type_id,
            mt.code AS message_type_code,
            mt.name AS message_type_name,
            a.recover_flag,
            a.break_time,
            a.update_time
        FROM
            dcim.alarm_act a
        JOIN
            dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id
        JOIN
            dcim.dict_event_level dl ON dl.id = a.event_level_id
        JOIN
            dcim.dict_major m ON m.id = a.major_id
        JOIN
            dcim.dict_event_type et ON et.id = a.event_type_id
        JOIN
            dcim.dict_alarm_message_type mt ON mt.id = a.message_type
        WHERE
            a.id = #{alarmId}
    </select>
    <select id="findAlarmExecute" resultType="com.gds.jpi.dcim.core.alarmhandlercenter.request.EmergencyExecuteRequest">
        SELECT
           a.id AS alarm_id,
           a.alarm_content,
           a.event_name,
           a.event_target_category_id,
           dtc.code AS event_target_category_code,
           a.event_target_inst_id,
           a.event_target_inst_name,
           a.temp_event_id AS sceneId,
           a.dc_id,
           a.dc_name,
           a.location_id,
           a.location_name,
           a.execute_id,
           a.simulate_flag
        FROM
            dcim.alarm_act a
        JOIN
            dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id
        WHERE
            a.id = #{alarmId}
    </select>
    <select id="findRoccAlarmAct" resultType="com.gds.jpi.dcim.response.RoccAlarmActResponse">
        SELECT
            a.id AS alarm_id,
            a.alarm_content,
            a.event_target_category_id,
            dtc.code AS event_target_category_code,
            dtc.name AS event_target_category_name,
            a.event_target_inst_id,
            a.event_target_inst_name,
            a.seq,
            a.event_id,
            a.event_name,
            a.dc_id,
            a.dc_name,
            a.location_id,
            a.location_name,
            a.event_level_id,
            dl.code AS event_level_code,
            dl.name AS event_level_name,
            IF(ea.auto_notice = 0, 3, IF(notice_way &amp; 2, 1, 2)) AS notice_priority,
            IF(ea.auto_notice = 0, '低', IF(notice_way &amp; 2, '高', '中')) AS notice_priority_name,
            a.major_id,
            m.code AS major_code,
            m.name AS major_name,
            a.message_type AS message_type_id,
            mt.code AS message_type_code,
            mt.name AS message_type_name,
            a.recover_flag,
            a.simulate_flag,
            a.plan_flag,
            a.break_time,
            a.update_time,
            a.opt_time
        FROM
            dcim.alarm_act a
        JOIN
            dcim.std_temp_event_notice_cfg n ON n.temp_event_id = a.temp_event_id
        JOIN
            dcim.event_apply ea ON ea.dc_id = a.dc_id AND ea.temp_event_id = a.temp_event_id
        JOIN
            dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id
        JOIN
            dcim.dict_event_level dl ON dl.id = a.event_level_id
        JOIN
            dcim.dict_major m ON m.id = a.major_id
        JOIN
            dcim.dict_alarm_message_type mt ON mt.id = a.message_type
        WHERE
            a.event_type_id = 1
            AND a.dc_id IN
            <foreach collection="dcIds" item="dcId" open="(" separator="," close=")">
                #{dcId}
            </foreach>
            <if test="fromTime != null">
                AND a.opt_time &gt;= #{fromTime}
            </if>
            <if test="endTime != null">
                AND a.opt_time &lt;= #{endTime}
            </if>
            <if test="recoverFlag != null">
                AND a.recover_flag = #{recoverFlag}
            </if>
    </select>
    <select id="findRoccAlarmActSummary" resultType="com.gds.jpi.dcim.response.RoccAlarmSummaryMajorResponse">
        SELECT
            a.dc_id,
            a.dc_name,
            a.major_id,
            m.code AS major_code,
            m.name AS major_name,
            l.id AS level_id,
            l.code AS level_code,
            l.name AS level_name,
            COUNT(*) AS count
        FROM
            dcim.alarm_act a
        JOIN
            dcim.dict_major m ON m.id = a.major_id
        JOIN
            dcim.dict_event_level l ON l.id = a.event_level_id
        WHERE
            a.event_type_id = 1
            AND a.recover_flag = 0
            AND a.dc_id IN
            <foreach collection="dcIds" item="dcId" open="(" separator="," close=")">
                #{dcId}
            </foreach>
        GROUP BY
            a.dc_id,
            m.id,
            l.id
    </select>
    <select id="getNoticeResult" resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.PhoneResult">
        SELECT r.account, s.position, s.name,
               IF(r.notice_result = 2, 1, 0) AS result
               FROM
            cbms.alarm_notice n
                JOIN
            cbms.alarm_notice_phone_result r ON n.id = r.notice_id
                JOIN
            sms.staff s ON r.account = s.account
        WHERE n.is_new = 1 and alarm_id = #{alarm_id};
    </select>
    <select id="getNoticeResultWithOutPosition" resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.PhoneResult">
        SELECT r.account, s.name,
               IF(r.notice_result = 2, 1, 0) AS result
        FROM
            cbms.alarm_notice n
                JOIN
            cbms.alarm_notice_phone_result r ON n.id = r.notice_id
                JOIN
            sms.staff s ON r.account = s.account
        WHERE n.is_new = 1 and alarm_id = #{alarm_id};
    </select>
</mapper>
```

#### Output:

```JSON {
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmActMapper\">\n    <select id=\"getActiveAlarm\" resultType=\"com.gds.jpi.dcim.model.alarm.AlarmAct\">\n        SELECT\n            id,event_name,dc_id,dc_name,event_id,a.temp_event_id,temp_event_name,location_id,location_name,location_full_name,\n               event_target_category_id,event_target_inst_id,event_target_inst_name,seq,event_target_type_id,event_target_type_name,\n               event_level_id, last_event_level_id,major_id,event_type_id,message_type,recover_flag,confirm_flag,confirm_account,\n               plan_flag,key_flag,reason_type_id,reason_descr,execute_id,incident_id,notice_id,break_time,update_time,alarm_unique\n        FROM\n            dcim.alarm_act a\n        WHERE (recover_flag = 0 or (recover_flag = 1 AND  confirm_flag = 0))\n        AND\n            dc_id IN\n        <foreach collection=\"list\" open=\"(\" close=\")\" separator=\",\" item=\"dcId\">\n            #{dcId}\n        </foreach>\n        <if test=\"labelId != null\">\n            AND EXISTS (\n                SELECT\n                    1\n                FROM\n                    dcim.std_temp_event_label_rel r\n                WHERE\n                    r.temp_event_id = a.temp_event_id\n            )\n        </if>\n        ORDER BY confirm_flag ,event_level_id DESC,update_time DESC\n    </select>\n    <select id=\"getActiveAlarmById\" resultType=\"com.gds.jpi.dcim.model.alarm.AlarmAct\">\n        SELECT\n        id,event_name,dc_id,dc_name,event_id,temp_event_id,temp_event_name,location_id,location_name,location_full_name,\n        event_target_category_id,event_target_inst_id,event_target_inst_name,seq,event_target_type_id,event_target_type_name,\n        event_level_id, last_event_level_id,major_id,event_type_id,message_type,recover_flag,confirm_flag,confirm_account,\n        plan_flag,key_flag,reason_type_id,reason_descr,execute_id,incident_id,notice_id,break_time,update_time,alarm_unique\n        FROM\n        dcim.alarm_act\n        WHERE (recover_flag = 0 or (recover_flag = 1 AND  confirm_flag = 0))\n        AND\n        id IN\n        <foreach collection=\"list\" open=\"(\" close=\")\" separator=\",\" item=\"id\">\n            #{id}\n        </foreach>\n        ORDER BY confirm_flag ,event_level_id DESC,update_time DESC\n    </select>\n    <select id=\"findAlarmRsp\" resultType=\"com.gds.jpi.dcim.vo.event.AlarmSubscribeRspAlarm\">\n        SELECT\n            a.id AS alarm_id,\n            a.event_id,\n            a.event_target_category_id,\n            dtc.code AS event_target_category_code,\n            dtc.name AS event_target_category_name,\n            a.event_target_inst_id,\n            a.event_target_inst_name,\n            a.seq,\n            a.temp_event_id,\n            a.temp_event_name,\n            a.event_name,\n            a.dc_id,\n            a.dc_name,\n            a.location_id,\n            a.location_name,\n            a.event_level_id,\n            dl.code AS event_level_code,\n            dl.name AS event_level_name,\n            a.major_id,\n            m.code AS major_code,\n            m.name AS major_name,\n            a.event_type_id,\n            et.code AS event_type_code,\n            et.name AS event_type_name,\n            a.message_type AS message_type_id,\n            mt.code AS message_type_code,\n            mt.name AS message_type_name,\n            a.recover_flag,\n            a.break_time,\n            a.update_time\n        FROM\n            dcim.alarm_act a\n        JOIN\n            dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n        JOIN\n            dcim.dict_event_level dl ON dl.id = a.event_level_id\n        JOIN\n            dcim.dict_major m ON m.id = a.major_id\n        JOIN\n            dcim.dict_event_type et ON et.id = a.event_type_id\n        JOIN\n            dcim.dict_alarm_message_type mt ON mt.id = a.message_type\n        WHERE\n            a.id = #{alarmId}\n    </select>\n    <select id=\"findAlarmExecute\" resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.request.EmergencyExecuteRequest\">\n        SELECT\n           a.id AS alarm_id,\n           a.alarm_content,\n           a.event_name,\n           a.event_target_category_id,\n           dtc.code AS event_target_category_code,\n           a.event_target_inst_id,\n           a.event_target_inst_name,\n           a.temp_event_id AS sceneId,\n           a.dc_id,\n           a.dc_name,\n           a.location_id,\n           a.location_name,\n           a.execute_id,\n           a.simulate_flag\n        FROM\n            dcim.alarm_act a\n        JOIN\n            dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n        WHERE\n            a.id = #{alarmId}\n    </select>\n    <select id=\"findRoccAlarmAct\" resultType=\"com.gds.jpi.dcim.response.RoccAlarmActResponse\">\n        SELECT\n            a.id AS alarm_id,\n            a.alarm_content,\n            a.event_target_category_id,\n            dtc.code AS event_target_category_code,\n            dtc.name AS event_target_category_name,\n            a.event_target_inst_id,\n            a.event_target_inst_name,\n            a.seq,\n            a.event_id,\n            a.event_name,\n            a.dc_id,\n            a.dc_name,\n            a.location_id,\n            a.location_name,\n            a.event_level_id,\n            dl.code AS event_level_code,\n            dl.name AS event_level_name,\n            IF(ea.auto_notice = 0, 3, IF(notice_way &amp; 2, 1, 2)) AS notice_priority,\n            IF(ea.auto_notice = 0, 'Low', IF(notice_way &amp; 2, 'high', 'middle')) AS notice_priority_name,\n            a.major_id,\n            m.code AS major_code,\n            m.name AS major_name,\n            a.message_type AS message_type_id,\n            mt.code AS message_type_code,\n            mt.name AS message_type_name,\n            a.recover_flag,\n            a.simulate_flag,\n            a.plan_flag,\n            a.break_time,\n            a.update_time,\n            a.opt_time\n        FROM\n            dcim.alarm_act a\n        JOIN\n            dcim.std_temp_event_notice_cfg n ON n.temp_event_id = a.temp_event_id\n        JOIN\n            dcim.event_apply ea ON ea.dc_id = a.dc_id AND ea.temp_event_id = a.temp_event_id\n        JOIN\n            dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n        JOIN\n            dcim.dict_event_level dl ON dl.id = a.event_level_id\n        JOIN\n            dcim.dict_major m ON m.id = a.major_id\n        JOIN\n            dcim.dict_alarm_message_type mt ON mt.id = a.message_type\n        WHERE\n            a.event_type_id = 1\n            AND a.dc_id IN\n            <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" separator=\",\" close=\")\">\n                #{dcId}\n            </foreach>\n            <if test=\"fromTime != null\">\n                AND a.opt_time &gt;= #{fromTime}\n            </if>\n            <if test=\"endTime != null\">\n                AND a.opt_time &lt;= #{endTime}\n            </if>\n            <if test=\"recoverFlag != null\">\n                AND a.recover_flag = #{recoverFlag}\n            </if>\n    </select>\n    <select id=\"findRoccAlarmActSummary\" resultType=\"com.gds.jpi.dcim.response.RoccAlarmSummaryMajorResponse\">\n        SELECT\n            a.dc_id,\n            a.dc_name,\n            a.major_id,\n            m.code AS major_code,\n            m.name AS major_name,\n            l.id AS level_id,\n            l.code AS level_code,\n            l.name AS level_name,\n            COUNT(*) AS count\n        FROM\n            dcim.alarm_act a\n        JOIN\n            dcim.dict_major m ON m.id = a.major_id\n        JOIN\n            dcim.dict_event_level l ON l.id = a.event_level_id\n        WHERE\n            a.event_type_id = 1\n            AND a.recover_flag = 0\n            AND a.dc_id IN\n            <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" separator=\",\" close=\")\">\n                #{dcId}\n            </foreach>\n        GROUP BY\n            a.dc_id,\n            m.id,\n            l.id\n    </select>\n    <select id=\"getNoticeResult\" resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.PhoneResult\">\n        SELECT r.account, s.position, s.name,\n               IF(r.notice_result = 2, 1, 0) AS result\n               FROM\n            cbms.alarm_notice n\n                JOIN\n            cbms.alarm_notice_phone_result r ON n.id = r.notice_id\n                JOIN\n            sms.staff s ON r.account = s.account\n        WHERE n.is_new = 1 and alarm_id = #{alarm_id};\n    </select>\n    <select id=\"getNoticeResultWithOutPosition\" resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.PhoneResult\">\n        SELECT r.account, s.name,\n               IF(r.notice_result = 2, 1, 0) AS result\n        FROM\n            cbms.alarm_notice n\n                JOIN\n            cbms.alarm_notice_phone_result r ON n.id = r.notice_id\n                JOIN\n            sms.staff s ON r.account = s.account\n        WHERE n.is_new = 1 and alarm_id = #{alarm_id};\n    </select>\n</mapper>",
  "details": [
{
"lineNumber": 127,
"lineType": "comment",
"jobType": "Text Translation",
"originalText": "低",
"translatedText": "Low"
},
{
"lineNumber": 127,
"lineType": "comment",
"jobType": "Text Translation",
"originalText": "高",
"translatedText": "High"
},
{
"lineNumber": 127,
"lineType": "comment",
"jobType": "Text Translation",
"originalText": "中",
"translatedText": "Medium"
}
]
}

```

### DEMOS2

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmCallbackMapper">
    <select id="findSubscribeByAlarmDc"
            resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">
        SELECT r.id AS record_id,
               r.business_code,
               r.business_guid,
               b.callback_url
        FROM dcim.alarm_subscribe_dc d
                 JOIN
             dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 2
                 JOIN
             dcim.alarm_subscribe_event_type t ON t.record_id = r.id
                 JOIN
             dcim.alarm_subscribe_business b ON b.business_code = r.business_code
        WHERE d.dc_id = #{alarm.dcId}
          AND t.event_type_id = #{alarm.eventTypeId}
          <if test="emergencyFlag">
              AND b.emergency_callback = 1
          </if>
    </select>
    <select id="findSubscribeByLabel"
            resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">
        SELECT
            DISTINCT
            r.id AS record_id,
            r.business_code,
            r.business_guid,
            b.callback_url
        FROM
            dcim.alarm_subscribe_dc d
        JOIN
            dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 3
        JOIN
            dcim.alarm_subscribe_event_type t ON t.record_id = r.id
        JOIN
            dcim.alarm_subscribe_event_label l ON l.record_id = r.id
        JOIN
            dcim.alarm_subscribe_business b ON b.business_code = r.business_code
        JOIN
            dcim.std_temp_event_label_rel re ON re.temp_event_id = #{alarm.tempEventId} AND l.event_label_id = re.event_label_id
        WHERE
            d.dc_id = #{alarm.dcId}
            AND t.event_type_id = #{alarm.eventTypeId}
            <if test="emergencyFlag">
                AND b.emergency_callback = 1
            </if>
    </select>
    <select id="findSubscribeByAlarmTarget"
            resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">
        SELECT r.id AS record_id,
               r.business_code,
               r.business_guid,
               b.callback_url
        FROM dcim.alarm_subscribe_event_target d
                 JOIN
             dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 1
                 JOIN
             dcim.alarm_subscribe_event_type t ON t.record_id = r.id
                 JOIN
             dcim.alarm_subscribe_business b ON b.business_code = r.business_code
        WHERE d.event_target_category_id = #{alarm.eventTargetCategoryId}
          AND d.event_target_inst_id = #{alarm.eventTargetInstId}
          AND d.temp_event_id IN (0, #{alarm.tempEventId})
          AND t.event_type_id = #{alarm.eventTypeId}
          <if test="emergencyFlag">
              AND b.emergency_callback = 1
          </if>
    </select>
    <select id="selectAlarmDetail"
            resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmCallbackAlarmDetail">
        SELECT
        a.id AS alarm_id,
        a.alarm_content,
        a.event_id,
        a.event_target_category_id,
        dtc.code AS event_target_category_code,
        dtc.name AS event_target_category_name,
        a.event_target_type_id,
        a.event_target_type_name,
        a.event_target_inst_id,
        a.event_target_inst_name,
        a.seq,
        a.temp_event_id,
        a.temp_event_name,
        a.event_name,
        a.dc_id,
        a.dc_name,
        a.location_id,
        a.location_name,
        a.event_level_id,
        dl.code AS event_level_code,
        dl.name AS event_level_name,
        a.major_id,
        m.code AS major_code,
        m.name AS major_name,
        a.event_type_id,
        et.code AS event_type_code,
        et.name AS event_type_name,
        a.message_type AS message_type_id,
        mt.code AS message_type_code,
        mt.name AS message_type_name,
        a.recover_flag,
        a.simulate_flag,
        a.execute_id,
        a.break_time,
        a.update_time
        FROM
        dcim.alarm_act a
        JOIN
        dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id
        JOIN
        dcim.dict_event_level dl ON dl.id = a.event_level_id
        JOIN
        dcim.dict_major m ON m.id = a.major_id
        JOIN
        dcim.dict_event_type et ON et.id = a.event_type_id
        JOIN
        dcim.dict_alarm_message_type mt ON mt.id = a.message_type
        WHERE
        a.id IN
        <foreach collection="alarmIds" item="alarmId" open="(" separator="," close=")">
            #{alarmId}
        </foreach>
    </select>
    <select id="findCallbackRetry"
            resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">
        SELECT l.record_id,
               r.business_code,
               l.business_guid,
               b.callback_url,
               l.id AS log_id,
               l.retry,
               l.alarm_id
        FROM dcim.alarm_subscribe_callback_log l
                 JOIN
             dcim.alarm_subscribe_record r ON l.record_id = r.id AND r.status = 1
                 JOIN
             dcim.alarm_subscribe_business b ON b.business_code = r.business_code
        WHERE l.create_time &gt; #{date}
          AND l.retry &lt; 10
          AND l.status = 1
    </select>
</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmCallbackMapper\">\n    <select id=\"findSubscribeByAlarmDc\"\n            resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail\">\n        SELECT r.id AS record_id,\n               r.business_code,\n               r.business_guid,\n               b.callback_url\n        FROM dcim.alarm_subscribe_dc d\n                 JOIN\n             dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 2\n                 JOIN\n             dcim.alarm_subscribe_event_type t ON t.record_id = r.id\n                 JOIN\n             dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n        WHERE d.dc_id = #{alarm.dcId}\n          AND t.event_type_id = #{alarm.eventTypeId}\n          <if test=\"emergencyFlag\">\n              AND b.emergency_callback = 1\n          </if>\n    </select>\n    <select id=\"findSubscribeByLabel\"\n            resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail\">\n        SELECT\n            DISTINCT\n            r.id AS record_id,\n            r.business_code,\n            r.business_guid,\n            b.callback_url\n        FROM\n            dcim.alarm_subscribe_dc d\n        JOIN\n            dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 3\n        JOIN\n            dcim.alarm_subscribe_event_type t ON t.record_id = r.id\n        JOIN\n            dcim.alarm_subscribe_event_label l ON l.record_id = r.id\n        JOIN\n            dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n        JOIN\n            dcim.std_temp_event_label_rel re ON re.temp_event_id = #{alarm.tempEventId} AND l.event_label_id = re.event_label_id\n        WHERE\n            d.dc_id = #{alarm.dcId}\n            AND t.event_type_id = #{alarm.eventTypeId}\n            <if test=\"emergencyFlag\">\n                AND b.emergency_callback = 1\n            </if>\n    </select>\n    <select id=\"findSubscribeByAlarmTarget\"\n            resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail\">\n        SELECT r.id AS record_id,\n               r.business_code,\n               r.business_guid,\n               b.callback_url\n        FROM dcim.alarm_subscribe_event_target d\n                 JOIN\n             dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 1\n                 JOIN\n             dcim.alarm_subscribe_event_type t ON t.record_id = r.id\n                 JOIN\n             dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n        WHERE d.event_target_category_id = #{alarm.eventTargetCategoryId}\n          AND d.event_target_inst_id = #{alarm.eventTargetInstId}\n          AND d.temp_event_id IN (0, #{alarm.tempEventId})\n          AND t.event_type_id = #{alarm.eventTypeId}\n          <if test=\"emergencyFlag\">\n              AND b.emergency_callback = 1\n          </if>\n    </select>\n    <select id=\"selectAlarmDetail\"\n            resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmCallbackAlarmDetail\">\n        SELECT\n        a.id AS alarm_id,\n        a.alarm_content,\n        a.event_id,\n        a.event_target_category_id,\n        dtc.code AS event_target_category_code,\n        dtc.name AS event_target_category_name,\n        a.event_target_type_id,\n        a.event_target_type_name,\n        a.event_target_inst_id,\n        a.event_target_inst_name,\n        a.seq,\n        a.temp_event_id,\n        a.temp_event_name,\n        a.event_name,\n        a.dc_id,\n        a.dc_name,\n        a.location_id,\n        a.location_name,\n        a.event_level_id,\n        dl.code AS event_level_code,\n        dl.name AS event_level_name,\n        a.major_id,\n        m.code AS major_code,\n        m.name AS major_name,\n        a.event_type_id,\n        et.code AS event_type_code,\n        et.name AS event_type_name,\n        a.message_type AS message_type_id,\n        mt.code AS message_type_code,\n        mt.name AS message_type_name,\n        a.recover_flag,\n        a.simulate_flag,\n        a.execute_id,\n        a.break_time,\n        a.update_time\n        FROM\n        dcim.alarm_act a\n        JOIN\n        dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n        JOIN\n        dcim.dict_event_level dl ON dl.id = a.event_level_id\n        JOIN\n        dcim.dict_major m ON m.id = a.major_id\n        JOIN\n        dcim.dict_event_type et ON et.id = a.event_type_id\n        JOIN\n        dcim.dict_alarm_message_type mt ON mt.id = a.message_type\n        WHERE\n        a.id IN\n        <foreach collection=\"alarmIds\" item=\"alarmId\" open=\"(\" separator=\",\" close=\")\">\n            #{alarmId}\n        </foreach>\n    </select>\n    <select id=\"findCallbackRetry\"\n            resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail\">\n        SELECT l.record_id,\n               r.business_code,\n               l.business_guid,\n               b.callback_url,\n               l.id AS log_id,\n               l.retry,\n               l.alarm_id\n        FROM dcim.alarm_subscribe_callback_log l\n                 JOIN\n             dcim.alarm_subscribe_record r ON l.record_id = r.id AND r.status = 1\n                 JOIN\n             dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n        WHERE l.create_time &gt; #{date}\n          AND l.retry &lt; 10\n          AND l.status = 1\n    </select>\n</mapper>",
  "details": []
}
```

### DEMOS3

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.AlarmCustomerLogMapper">
    <select id="selectCustomerLogsByReq" parameterType="integer" resultType="com.gds.jpi.dcim.vo.AlarmCustomerLogVO">
        SELECT cl.log_id,
        cl.ope_time,
        st.name AS typeName,
        st.id AS typeId,
        st.code AS typeCode,
        CONCAT(om.name, '(', dc.dc_name_inner_short, ' ', '|', ' ', cc.name, ')') AS opeModel,
        sa.name AS opeName
        FROM cbms.alarm_notice_customer_log cl
        LEFT JOIN cbms.sys_operate_type_dict st ON cl.type_code = st.code
        LEFT JOIN cbms.alarm_operate_module_dict om ON cl.ope_module_id = om.id
        LEFT JOIN cbms.alarm_notice_customer_rule cr ON cl.rule_id = cr.id
        LEFT JOIN sms.account sa ON cl.operator = sa.account
        LEFT JOIN geo.dc_mapping dc ON cr.dc_id = dc.id
        LEFT JOIN cip.cip_customer cc ON cr.customer_id = cc.customer_id
        <where>
            <if test="ruleId != null and ruleId > 0">
                AND cl.rule_id = #{ruleId}
            </if>
            <if test="dcId != null">
                AND dc.id = #{dcId}
            </if>
            <if test="customerId != null">
                AND cr.customer_id = #{customerId}
            </if>
            <if test="start != null">
                AND DATE_FORMAT(cl.ope_time, '%Y-%m-%d') &gt;= #{start}
            </if>
            <if test="end != null">
                AND DATE_FORMAT(cl.ope_time, '%Y-%m-%d') &lt;= #{end}
            </if>
        </where>
        ORDER BY cl.ope_time DESC
    </select>

    <select id="selectOneCustomerLog" parameterType="integer" resultType="map">
        SELECT cl.log_id,
        <if test="opeModel == 5">
            cld.field_code,
            CONCAT('【', fm.name, '】') AS field_name,
            cld.`before`,
            cld.`after`
        </if>
        <if test="opeModel == 6">
            sot.id AS type_id,
            ccd.type_code,
            sot.name AS type_name,
            ccd.customer_id,
            cc.name AS customer_name,
            ccd.contact_id
        </if>
        <if test="opeModel == 7">
            csd.scene_id,
            csd.type_code AS type_code,
            sot.id AS type_id,
            sot.name AS type_name,
            cas.scene_name
        </if>
        FROM cbms.alarm_notice_customer_log cl
        <if test="opeModel == 5">
            LEFT JOIN cbms.alarm_notice_customer_rule_log_detail cld
            ON cl.log_id = cld.log_id
            LEFT JOIN cbms.sys_field_mapping fm ON cld.field_code = fm.code AND fm.biz_belong = 'NOTICE_CUSTOMER_RULE'
        </if>
        <if test="opeModel == 6">
            LEFT JOIN cbms.alarm_notice_customer_contact_log_detail ccd ON cl.log_id = ccd.log_id
            LEFT JOIN cbms.sys_operate_type_dict sot ON ccd.type_code = sot.`code`
            LEFT JOIN cip.cip_customer cc ON ccd.customer_id = cc.customer_id
        </if>
        <if test="opeModel == 7">
            LEFT JOIN cbms.alarm_notice_customer_scene_log_detail csd ON cl.log_id = csd.log_id
            LEFT JOIN cbms.alarm_scene cas ON csd.scene_id = cas.scene_id
            LEFT JOIN cbms.sys_operate_type_dict sot ON csd.type_code = sot.code
        </if>
        <where>
            <if test="logId != null">
                AND cl.log_id = #{logId}
            </if>
            <if test="opeModel == 5">
                AND fm.is_show = 1
            </if>
        </where>
    </select>
</mapper>

```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.AlarmCustomerLogMapper\">\n    <select id=\"selectCustomerLogsByReq\" parameterType=\"integer\" resultType=\"com.gds.jpi.dcim.vo.AlarmCustomerLogVO\">\n        SELECT cl.log_id,\n        cl.ope_time,\n        st.name AS typeName,\n        st.id AS typeId,\n        st.code AS typeCode,\n        CONCAT(om.name, '(', dc.dc_name_inner_short, ' ', '|', ' ', cc.name, ')') AS opeModel,\n        sa.name AS opeName\n        FROM cbms.alarm_notice_customer_log cl\n        LEFT JOIN cbms.sys_operate_type_dict st ON cl.type_code = st.code\n        LEFT JOIN cbms.alarm_operate_module_dict om ON cl.ope_module_id = om.id\n        LEFT JOIN cbms.alarm_notice_customer_rule cr ON cl.rule_id = cr.id\n        LEFT JOIN sms.account sa ON cl.operator = sa.account\n        LEFT JOIN geo.dc_mapping dc ON cr.dc_id = dc.id\n        LEFT JOIN cip.cip_customer cc ON cr.customer_id = cc.customer_id\n        <where>\n            <if test=\"ruleId != null and ruleId > 0\">\n                AND cl.rule_id = #{ruleId}\n            </if>\n            <if test=\"dcId != null\">\n                AND dc.id = #{dcId}\n            </if>\n            <if test=\"customerId != null\">\n                AND cr.customer_id = #{customerId}\n            </if>\n            <if test=\"start != null\">\n                AND DATE_FORMAT(cl.ope_time, '%Y-%m-%d') &gt;= #{start}\n            </if>\n            <if test=\"end != null\">\n                AND DATE_FORMAT(cl.ope_time, '%Y-%m-%d') &lt;= #{end}\n            </if>\n        </where>\n        ORDER BY cl.ope_time DESC\n    </select>\n\n    <select id=\"selectOneCustomerLog\" parameterType=\"integer\" resultType=\"map\">\n        SELECT cl.log_id,\n        <if test=\"opeModel == 5\">\n            cld.field_code,\n            CONCAT('【', fm.name, '】') AS field_name,\n            cld.`before`,\n            cld.`after`\n        </if>\n        <if test=\"opeModel == 6\">\n            sot.id AS type_id,\n            ccd.type_code,\n            sot.name AS type_name,\n            ccd.customer_id,\n            cc.name AS customer_name,\n            ccd.contact_id\n        </if>\n        <if test=\"opeModel == 7\">\n            csd.scene_id,\n            csd.type_code AS type_code,\n            sot.id AS type_id,\n            sot.name AS type_name,\n            cas.scene_name\n        </if>\n        FROM cbms.alarm_notice_customer_log cl\n        <if test=\"opeModel == 5\">\n            LEFT JOIN cbms.alarm_notice_customer_rule_log_detail cld\n            ON cl.log_id = cld.log_id\n            LEFT JOIN cbms.sys_field_mapping fm ON cld.field_code = fm.code AND fm.biz_belong = 'NOTICE_CUSTOMER_RULE'\n        </if>\n        <if test=\"opeModel == 6\">\n            LEFT JOIN cbms.alarm_notice_customer_contact_log_detail ccd ON cl.log_id = ccd.log_id\n            LEFT JOIN cbms.sys_operate_type_dict sot ON ccd.type_code = sot.`code`\n            LEFT JOIN cip.cip_customer cc ON ccd.customer_id = cc.customer_id\n        </if>\n        <if test=\"opeModel == 7\">\n            LEFT JOIN cbms.alarm_notice_customer_scene_log_detail csd ON cl.log_id = csd.log_id\n            LEFT JOIN cbms.alarm_scene cas ON csd.scene_id = cas.scene_id\n            LEFT JOIN cbms.sys_operate_type_dict sot ON csd.type_code = sot.code\n        </if>\n        <where>\n            <if test=\"logId != null\">\n                AND cl.log_id = #{logId}\n            </if>\n            <if test=\"opeModel == 5\">\n                AND fm.is_show = 1\n            </if>\n        </where>\n    </select>\n</mapper>",
"details": []
}
```

### DEMOS4

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.RecordCabinetDailyMapper">
<update id="createTable" parameterType="String">
        CREATE TABLE `dcim`.`record_cabinet_daily_${dcId}`
        (
            `id`                                 int(11) NOT NULL AUTO_INCREMENT,
            `overview_id`                        int(11) NOT NULL COMMENT '关联概览id',
            `collect_time`                       datetime NOT NULL COMMENT '采集时间',
            `cabinet_id`                         int(11) NOT NULL COMMENT '机柜id，来源cbms.sys_cabinet.cabinet_id',
            `cabinet_name`                       varchar(10) COLLATE utf8_unicode_ci  DEFAULT NULL COMMENT '机柜名，来源cbms.sys_cabinet.name',
            `room_id`                            int(11) DEFAULT NULL COMMENT '机房id，来源cbms.sys_cabinet.room_id',
            `room_name`                          varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT '机房名，来源dcrm.room.name',
            `project_code`                       varchar(20) COLLATE utf8_unicode_ci  DEFAULT NULL COMMENT '项目编号，来源cbms.sys_cabinet.project_code',
            `cntrct_elec_kva`                    decimal(10, 2)                       DEFAULT NULL COMMENT '合约电力，来源cbms.sys_cabinet.cntrct_elec_kva',
            `a_customer_id`                      bigint(20) DEFAULT NULL COMMENT '甲方客户id，来源cip.cip_project.a_customer_id',
            `a_customer_name`                    varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT '甲方客户名，来源cip.cip_customer.name',
            `end_customer_id`                    bigint(20) DEFAULT NULL COMMENT '最终客户id，来源cip.cip_project.end_customer_id',
            `end_customer_name`                  varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT '最终客户名，来源cip.cip_customer.name',
            `elect_status`                       tinyint(1) NOT NULL DEFAULT 0 COMMENT '上电状态，0：未上电，1：上电，来源：cbms.sys_cabinet.elec_status',
            `attr_cnt`                           int(11) NOT NULL DEFAULT 0 COMMENT '机柜配电数量，来源cbms.sys_cabinet.attr_count',
            `original_power_kw`                  double   DEFAULT NULL COMMENT '功率原始值',
            `original_power_kwh`                 double   DEFAULT NULL COMMENT '电度原始值',
            `original_current`                   double   DEFAULT NULL COMMENT '电流原始值',
            `original_power_kw_increment_ratio`  double   DEFAULT NULL COMMENT '功率原始值环比',
            `original_power_kwh_increment_ratio` double   DEFAULT NULL COMMENT '电度原始值环比',
            `fixed_power_kw`                     double   DEFAULT NULL COMMENT '功率修正值',
            `fixed_power_kwh`                    double   DEFAULT NULL COMMENT '电度修正值',
            `fixed_power_kw_increment_ratio`     double   DEFAULT NULL COMMENT '功率修正值环比',
            `fixed_power_kwh_increment_ratio`    double   DEFAULT NULL COMMENT '电度修正值环比',
            `confirm_power_kw`                   double   DEFAULT NULL COMMENT '功率确认值',
            `confirm_power_kwh`                  double   DEFAULT NULL COMMENT '电度确认值',
            `confirm_power_kw_increment_ratio`   double   DEFAULT NULL COMMENT '功率确认值环比',
            `confirm_power_kwh_increment_ratio`  double   DEFAULT NULL COMMENT '电度确认值环比',
            `yesterday_power_kw`                 double   DEFAULT NULL COMMENT '昨日功率（环比参考值）',
            `yesterday_power_kwh`                double   DEFAULT NULL COMMENT '昨日电度（环比参考值）',
            `first_power_kwh`                    double   DEFAULT NULL COMMENT '一天的电度起始值',
            `last_power_kwh`                     double   DEFAULT NULL COMMENT '一天的电度结束值',
            `abnormal_flag`                      tinyint(4) NOT NULL DEFAULT 0 COMMENT '是否有异常，1：是，0：否',
            `update_flag`                        tinyint(4) NOT NULL DEFAULT 0 COMMENT '是否已修改，0：未修改，1：修改了功率，2：修改了电度，3：全都修改了',
            `fixed_flag`                         tinyint(4) NOT NULL DEFAULT -1 COMMENT '是否已处理异常，-1：无需处理，0：未处理，1：已处理',
            `update_time`                        datetime DEFAULT NULL COMMENT '更新时间',
            `auto_fixed_flag`                    tinyint(4) NOT NULL DEFAULT 0 COMMENT '是否是自动修正,  0: 人工修正 , 1: 系统自动修正',
            PRIMARY KEY (`id`) USING BTREE,
            UNIQUE KEY `idx_collect_time_cabinet_id` (`collect_time`,`cabinet_id`) USING BTREE,
            KEY                                  `idx_update_time` (`update_time`) USING BTREE,
            KEY                                  `idx_overview_id_room_id_cabinet_name` (`overview_id`,`room_id`,`cabinet_name`) USING BTREE
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci ROW_FORMAT=DYNAMIC COMMENT='机柜指标日填报详情表';
    </update>

</mapper>
```

#### Output:

```json
{
"translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.RecordCabinetDailyMapper\">\n    <update id=\"createTable\" parameterType=\"String\">\n        CREATE TABLE `dcim`.`record_cabinet_daily_${dcId}`\n        (\n            `id`                                 int(11) NOT NULL AUTO_INCREMENT,\n            `overview_id`                        int(11) NOT NULL COMMENT 'Associated overview ID',\n            `collect_time`                       datetime NOT NULL COMMENT 'Collection time',\n            `cabinet_id`                         int(11) NOT NULL COMMENT 'Cabinet ID, source: cbms.sys_cabinet.cabinet_id',\n            `cabinet_name`                       varchar(10) COLLATE utf8_unicode_ci  DEFAULT NULL COMMENT 'Cabinet name, source: cbms.sys_cabinet.name',\n            `room_id`                            int(11) DEFAULT NULL COMMENT 'Room ID, source: cbms.sys_cabinet.room_id',\n            `room_name`                          varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT 'Room name, source: dcrm.room.name',\n            `project_code`                       varchar(20) COLLATE utf8_unicode_ci  DEFAULT NULL COMMENT 'Project code, source: cbms.sys_cabinet.project_code',\n            `cntrct_elec_kva`                    decimal(10, 2)                       DEFAULT NULL COMMENT 'Contracted power, source: cbms.sys_cabinet.cntrct_elec_kva',\n            `a_customer_id`                      bigint(20) DEFAULT NULL COMMENT 'Party A customer ID, source: cip.cip_project.a_customer_id',\n            `a_customer_name`                    varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT 'Party A customer name, source: cip.cip_customer.name',\n            `end_customer_id`                    bigint(20) DEFAULT NULL COMMENT 'End customer ID, source: cip.cip_project.end_customer_id',\n            `end_customer_name`                  varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT 'End customer name, source: cip.cip_customer.name',\n            `elect_status`                       tinyint(1) NOT NULL DEFAULT 0 COMMENT 'Power status, 0: Not powered, 1: Powered, source: cbms.sys_cabinet.elec_status',\n            `attr_cnt`                           int(11) NOT NULL DEFAULT 0 COMMENT 'Cabinet power distribution quantity, source: cbms.sys_cabinet.attr_count',\n            `original_power_kw`                  double   DEFAULT NULL COMMENT 'Original power value',\n            `original_power_kwh`                 double   DEFAULT NULL COMMENT 'Original energy value',\n            `original_current`                   double   DEFAULT NULL COMMENT 'Original current value',\n            `original_power_kw_increment_ratio`  double   DEFAULT NULL COMMENT 'Original power value ratio',\n            `original_power_kwh_increment_ratio` double   DEFAULT NULL COMMENT 'Original energy value ratio',\n            `fixed_power_kw`                     double   DEFAULT NULL COMMENT 'Corrected power value',\n            `fixed_power_kwh`                    double   DEFAULT NULL COMMENT 'Corrected energy value',\n            `fixed_power_kw_increment_ratio`     double   DEFAULT NULL COMMENT 'Corrected power value ratio',\n            `fixed_power_kwh_increment_ratio`    double   DEFAULT NULL COMMENT 'Corrected energy value ratio',\n            `confirm_power_kw`                   double   DEFAULT NULL COMMENT 'Confirmed power value',\n            `confirm_power_kwh`                  double   DEFAULT NULL COMMENT 'Confirmed energy value',\n            `confirm_power_kw_increment_ratio`   double   DEFAULT NULL COMMENT 'Confirmed power value ratio',\n            `confirm_power_kwh_increment_ratio`  double   DEFAULT NULL COMMENT 'Confirmed energy value ratio',\n            `yesterday_power_kw`                 double   DEFAULT NULL COMMENT 'Yesterday\\'s power (ratio reference value)',\n            `yesterday_power_kwh`                double   DEFAULT NULL COMMENT 'Yesterday\\'s energy (ratio reference value)',\n            `first_power_kwh`                    double   DEFAULT NULL COMMENT 'Starting energy value of the day',\n            `last_power_kwh`                     double   DEFAULT NULL COMMENT 'Ending energy value of the day',\n            `abnormal_flag`                      tinyint(4) NOT NULL DEFAULT 0 COMMENT 'Whether there is an anomaly, 1: Yes, 0: No',\n            `update_flag`                        tinyint(4) NOT NULL DEFAULT 0 COMMENT 'Whether modified, 0: Not modified, 1: Power modified, 2: Energy modified, 3: All modified',\n            `fixed_flag`                         tinyint(4) NOT NULL DEFAULT -1 COMMENT 'Whether anomaly handled, -1: No need to handle, 0: Not handled, 1: Handled',\n            `update_time`                        datetime DEFAULT NULL COMMENT 'Update time',\n            `auto_fixed_flag`                    tinyint(4) NOT NULL DEFAULT 0 COMMENT 'Whether auto-corrected, 0: Manual correction, 1: System auto-correction',\n            PRIMARY KEY (`id`) USING BTREE,\n            UNIQUE KEY `idx_collect_time_cabinet_id` (`collect_time`,`cabinet_id`) USING BTREE,\n            KEY                                  `idx_update_time` (`update_time`) USING BTREE,\n            KEY                                  `idx_overview_id_room_id_cabinet_name` (`overview_id`,`room_id`,`cabinet_name`) USING BTREE\n        ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci ROW_FORMAT=DYNAMIC COMMENT='Cabinet indicator daily report details table'\n    </update>\n\n</mapper>",
"details": [
    {
      "lineNumber": 177,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "关联概览id",
      "translatedText": "Associated overview ID"
    },
    {
      "lineNumber": 178,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "采集时间",
      "translatedText": "Collection time"
    },
    {
      "lineNumber": 179,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜id，来源cbms.sys_cabinet.cabinet_id",
      "translatedText": "Cabinet ID, source: cbms.sys_cabinet.cabinet_id"
    },
    {
      "lineNumber": 180,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜名，来源cbms.sys_cabinet.name",
      "translatedText": "Cabinet name, source: cbms.sys_cabinet.name"
    },
    {
      "lineNumber": 181,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机房id，来源cbms.sys_cabinet.room_id",
      "translatedText": "Room ID, source: cbms.sys_cabinet.room_id"
    },
    {
      "lineNumber": 182,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机房名，来源dcrm.room.name",
      "translatedText": "Room name, source: dcrm.room.name"
    },
    {
      "lineNumber": 183,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "项目编号，来源cbms.sys_cabinet.project_code",
      "translatedText": "Project code, source: cbms.sys_cabinet.project_code"
    },
    {
      "lineNumber": 184,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "合约电力，来源cbms.sys_cabinet.cntrct_elec_kva",
      "translatedText": "Contracted power, source: cbms.sys_cabinet.cntrct_elec_kva"
    },
    {
      "lineNumber": 185,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "甲方客户id，来源cip.cip_project.a_customer_id",
      "translatedText": "Party A customer ID, source: cip.cip_project.a_customer_id"
    },
    {
      "lineNumber": 186,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "甲方客户名，来源cip.cip_customer.name",
      "translatedText": "Party A customer name, source: cip.cip_customer.name"
    },
    {
      "lineNumber": 187,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "最终客户id，来源cip.cip_project.end_customer_id",
      "translatedText": "End customer ID, source: cip.cip_project.end_customer_id"
    },
    {
      "lineNumber": 188,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "最终客户名，来源cip.cip_customer.name",
      "translatedText": "End customer name, source: cip.cip_customer.name"
    },
    {
      "lineNumber": 189,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "上电状态，0：未上电，1：上电，来源：cbms.sys_cabinet.elec_status",
      "translatedText": "Power status, 0: Not powered, 1: Powered, source: cbms.sys_cabinet.elec_status"
    },
    {
      "lineNumber": 190,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜配电数量，来源cbms.sys_cabinet.attr_count",
      "translatedText": "Cabinet power distribution quantity, source: cbms.sys_cabinet.attr_count"
    },
    {
      "lineNumber": 191,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "功率原始值",
      "translatedText": "Original power value"
    },
    {
      "lineNumber": 192,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电度原始值",
      "translatedText": "Original energy value"
    },
    {
      "lineNumber": 193,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电流原始值",
      "translatedText": "Original current value"
    },
    {
      "lineNumber": 194,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "功率原始值环比",
      "translatedText": "Original power value ratio"
    },
    {
      "lineNumber": 195,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电度原始值环比",
      "translatedText": "Original energy value ratio"
    },
    {
      "lineNumber": 196,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "功率修正值",
      "translatedText": "Corrected power value"
    },
    {
      "lineNumber": 197,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电度修正值",
      "translatedText": "Corrected energy value"
    },
    {
      "lineNumber": 198,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "功率修正值环比",
      "translatedText": "Corrected power value ratio"
    },
    {
      "lineNumber": 199,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电度修正值环比",
      "translatedText": "Corrected energy value ratio"
    },
    {
      "lineNumber": 200,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "功率确认值",
      "translatedText": "Confirmed power value"
    },
    {
      "lineNumber": 201,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电度确认值",
      "translatedText": "Confirmed energy value"
    },
    {
      "lineNumber": 202,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "功率确认值环比",
      "translatedText": "Confirmed power value ratio"
    },
    {
      "lineNumber": 203,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电度确认值环比",
      "translatedText": "Confirmed energy value ratio"
    },
    {
      "lineNumber": 204,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "昨日功率（环比参考值）",
      "translatedText": "Yesterday's power (ratio reference value)"
    },
    {
      "lineNumber": 205,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "昨日电度（环比参考值）",
      "translatedText": "Yesterday's energy (ratio reference value)"
    },
    {
      "lineNumber": 206,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "一天的电度起始值",
      "translatedText": "Starting energy value of the day"
    },
    {
      "lineNumber": 207,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "一天的电度结束值",
      "translatedText": "Ending energy value of the day"
    },
    {
      "lineNumber": 208,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "是否有异常，1：是，0：否",
      "translatedText": "Whether there is an anomaly, 1: Yes, 0: No"
    },
    {
      "lineNumber": 209,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "是否已修改，0：未修改，1：修改了功率，2：修改了电度，3：全都修改了",
      "translatedText": "Whether modified, 0: Not modified, 1: Power modified, 2: Energy modified, 3: All modified"
    },
    {
      "lineNumber": 210,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "是否已处理异常，-1：无需处理，0：未处理，1：已处理",
      "translatedText": "Whether anomaly handled, -1: No need to handle, 0: Not handled, 1: Handled"
    },
    {
      "lineNumber": 211,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "更新时间",
      "translatedText": "Update time"
    },
    {
      "lineNumber": 212,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "是否是自动修正,  0: 人工修正 , 1: 系统自动修正",
      "translatedText": "Whether auto-corrected, 0: Manual correction, 1: System auto-correction"
    },
    {
      "lineNumber": 215,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜指标日填报详情表",
      "translatedText": "Cabinet indicator daily report details table"
    }
  ]
}

```

### DEMOS5

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.alarm.AlarmInfluenceCustomerMapper">
    <select id="getRoomLocIds" resultType="java.lang.Integer">
        SELECT
            ll.id
        FROM
            geo.location l
        JOIN
            geo.location_type lt ON lt.type_id = l.sp_type
        JOIN
            geo.location_category lc ON lc.id = lt.category_id AND lc.code = 'gsac.location_category.modules'
        LEFT JOIN
            geo.location ll ON ll.`left` &gt;= l.`left` AND ll.`right` &lt;= l.`right` AND ll.status = 1
        WHERE
            l.status = 1
            AND l.id IN
            <foreach collection="locationIds" item="locationId" open="(" separator="," close=")">
                #{locationId}
            </foreach>
    </select>
    <select id="getAssetTypeByRoot" resultType="com.gds.jpi.dcim.vo.alarm.AssetTypeRoot">
        SELECT
            t.id AS root_type_id,
            t.code AS root_type_code,
            tc.id AS type_id
        FROM
            asset.asset_type t
        JOIN
            asset.asset_type tc ON tc.`left` &gt;= t.`left` AND tc.`right` &lt;=t.`right` AND tc.is_delete = 0
        WHERE
            t.is_delete = 0
            AND t.code IN
            <foreach collection="typeCodes" item="typeCode" open="(" separator="," close=")">
                #{typeCode}
            </foreach>
    </select>
</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.alarm.AlarmInfluenceCustomerMapper\">\n    <select id=\"getRoomLocIds\" resultType=\"java.lang.Integer\">\n        SELECT\n            ll.id\n        FROM\n            geo.location l\n        JOIN\n            geo.location_type lt ON lt.type_id = l.sp_type\n        JOIN\n            geo.location_category lc ON lc.id = lt.category_id AND lc.code = 'gsac.location_category.modules'\n        LEFT JOIN\n            geo.location ll ON ll.`left` &gt;= l.`left` AND ll.`right` &lt;= l.`right` AND ll.status = 1\n        WHERE\n            l.status = 1\n            AND l.id IN\n            <foreach collection=\"locationIds\" item=\"locationId\" open=\"(\" separator=\",\" close=\")\">\n                #{locationId}\n            </foreach>\n    </select>\n    <select id=\"getAssetTypeByRoot\" resultType=\"com.gds.jpi.dcim.vo.alarm.AssetTypeRoot\">\n        SELECT\n            t.id AS root_type_id,\n            t.code AS root_type_code,\n            tc.id AS type_id\n        FROM\n            asset.asset_type t\n        JOIN\n            asset.asset_type tc ON tc.`left` &gt;= t.`left` AND tc.`right` &lt;=t.`right` AND tc.is_delete = 0\n        WHERE\n            t.is_delete = 0\n            AND t.code IN\n            <foreach collection=\"typeCodes\" item=\"typeCode\" open=\"(\" separator=\",\" close=\")\">\n                #{typeCode}\n            </foreach>\n    </select>\n</mapper>",
  "details": []
}
```

### DEMOS6

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.AlarmInnerMapper">
    <select id="selectInnerMatrix" parameterType="com.gds.jpi.dcim.vo.AlarmInnerGetReqVO"
            resultType="com.gds.jpi.dcim.vo.AlarmInnerNoticeVO">
        SELECT DISTINCT
        adrr.id AS innerId,
        sa.name AS accountName,
        adrr.account AS account,
        gdc.gid AS dcGid,
        gdc.short_name AS dcLimitation,
        ar.id AS roleId,
        ar.name AS roleName,
        ar.role_type_id AS roleTypeId,
        ar.phone_priority AS phonePriority,
        apd.id AS priorityId,
        apd.name AS priorityName,
        m.id AS majorId,
        m.name AS majorName
        FROM cbms.alarm_notice_account_dc_role_r adrr
        LEFT JOIN sms.account sa ON adrr.account = sa.account
        LEFT JOIN geo.dc_tree gdc ON adrr.dc_gid = gdc.gid
        LEFT JOIN geo.dc_tree dct ON dct.gid >= gdc.min_id  AND gdc.max_id >=dct.gid
        LEFT JOIN cbms.alarm_notice_role ar ON adrr.role_id = ar.id
        LEFT JOIN dcim.dict_major m ON adrr.major_id = m.id
        LEFT JOIN cbms.alarm_notice_priority_dict apd ON adrr.priority = apd.id
        <if test="noticeLevelIds != null and noticeLevelIds.size() != 0">
            LEFT JOIN cbms.alarm_notice_rule nr ON ar.id = nr.role_id
        </if>
        <where>
            AND ar.status = 1
            <if test="null != account and '' != account">
                AND sa.account = #{account}
            </if>
            <if test="null != dcGid">
                AND gdc.max_id &gt;= #{dcGid}
                AND gdc.min_id &lt;= #{dcGid}
            </if>
            <if test="null != roleIds and roleIds.size() > 0">
                AND ar.id IN
                <foreach collection="roleIds" item="roleId" open="(" separator="," close=")">
                    #{roleId}
                </foreach>
            </if>
            <if test="null != priority">
                AND apd.id = #{priority}
            </if>
            <if test="null != majorIds and majorIds.size() > 0">
                AND m.id IN
                <foreach collection="majorIds" item="majorId" open="(" separator="," close=")">
                    #{majorId}
                </foreach>
            </if>
            <if test="dcIds != null and dcIds.size() != 0">
                AND dct.dc_id IN
                <foreach collection="dcIds" item="dcId" open="(" separator="," close=")">
                    #{dcId}
                </foreach>
            </if>
            <if test="dcGids != null and dcGids.size() != 0">
                AND gdc.gid IN
                <foreach collection="dcGids" item="dcGid" open="(" separator="," close=")">
                    #{dcGid}
                </foreach>
            </if>
            <if test="noticeLevelIds != null and noticeLevelIds.size() != 0">
                AND nr.notice_level_id IN
                <foreach collection="noticeLevelIds" item="noticeLevelId" open="(" separator="," close=")">
                    #{noticeLevelId}
                </foreach>
            </if>
        </where>
        ORDER BY gdc.gid, ar.role_type_id DESC, CONVERT(ar.name using gbk), m.id, apd.id, CONVERT(sa.name using gbk)
    </select>

    <select id="queryInnerMatrix" parameterType="com.gds.jpi.dcim.vo.AlarmInnerGetReqVO"
            resultType="com.gds.jpi.dcim.vo.AlarmInnerNoticeVO">
        SELECT
        rel.id AS innerId,
        a.name AS accountName,
        rel.account AS account,
        pp.gid AS dcGid,
        pp.short_name AS dcLimitation,
        role.id AS roleId,
        role.name AS roleName,
        role.role_type_id AS roleTypeId,
        role.phone_priority AS phonePriority,
        apd.id AS priorityId,
        apd.name AS priorityName,
        m.id AS majorId,
        m.name AS majorName,
        a.email as email
        FROM
        dcim.event_notice_level l  <!-- 通知等级 -->
        INNER JOIN
        cbms.alarm_notice_rule rule ON rule.notice_level_id = l.id <!-- 根据通知等级关联相应通知规则 -->
        INNER JOIN
        cbms.alarm_notice_role role ON role.id = rule.role_id <!-- 匹配相应通知规则中的通知角色-->
        INNER JOIN
        cbms.alarm_notice_account_dc_role_r rel ON rel.role_id = role.id <!-- 根据角色初步筛选到人-->
        INNER JOIN
        (
        <!-- 结合dc_id的条件匹配数据中心运营主数据中的DC这一级 -->
        <!-- 关联所有上级节点（全国-区域-园区-数据中心）-->
        SELECT DISTINCT p.* FROM geo.dc_tree t
        INNER JOIN geo.dc_tree p ON p.min_id &lt;= t.gid AND p.max_id &gt;= t.gid
        WHERE 1=1
        <!-- 配置数据中心运营主数据中的DC级 -->
        <if test="dcIds != null and dcIds.size() != 0">
            AND t.dc_id IN
            <foreach collection="dcIds" item="dcId" open="(" separator="," close=")">
                #{dcId}
            </foreach>
        </if>
        ) pp ON rel.dc_gid = pp.gid
        <!-- 配置通知人的账户 -->
        LEFT JOIN
        sms.account a ON a.account = rel.account
        <!-- 配置通知人的专业，通知人可能无对应专业 -->
        LEFT JOIN
        dcim.dict_major m ON rel.major_id = m.id
        <!-- 配置通知人的A/B角，通知人可能无A/B角 -->
        LEFT JOIN
        cbms.alarm_notice_priority_dict apd ON rel.priority = apd.id
        WHERE
        a.status = 1 <!-- 账户必须有效 -->
        AND role.status = 1 <!-- 角色必须有效 -->
        <!-- 根据通知等级找到对应的通知规则 -->
        <if test="noticeLevelIds != null and noticeLevelIds.size() != 0">
            AND rule.notice_level_id IN
            <foreach collection="noticeLevelIds" item="noticeLevelId" open="(" separator="," close=")">
                #{noticeLevelId}
            </foreach>
        </if>
        <!-- 根据专业筛选合适的同志人，有的通知人是没有专业的 -->
        <if test="null != majorIds and majorIds.size() > 0">
            AND (
                m.id IN
            <foreach collection="majorIds" item="majorId" open="(" separator="," close=")">
                #{majorId}
            </foreach>
                OR m.id IS NULL
                )
        </if>
    </select>
</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.AlarmInnerMapper\">\n    <select id=\"selectInnerMatrix\" parameterType=\"com.gds.jpi.dcim.vo.AlarmInnerGetReqVO\"\n            resultType=\"com.gds.jpi.dcim.vo.AlarmInnerNoticeVO\">\n        SELECT DISTINCT\n        adrr.id AS innerId,\n        sa.name AS accountName,\n        adrr.account AS account,\n        gdc.gid AS dcGid,\n        gdc.short_name AS dcLimitation,\n        ar.id AS roleId,\n        ar.name AS roleName,\n        ar.role_type_id AS roleTypeId,\n        ar.phone_priority AS phonePriority,\n        apd.id AS priorityId,\n        apd.name AS priorityName,\n        m.id AS majorId,\n        m.name AS majorName\n        FROM cbms.alarm_notice_account_dc_role_r adrr\n        LEFT JOIN sms.account sa ON adrr.account = sa.account\n        LEFT JOIN geo.dc_tree gdc ON adrr.dc_gid = gdc.gid\n        LEFT JOIN geo.dc_tree dct ON dct.gid >= gdc.min_id  AND gdc.max_id >=dct.gid\n        LEFT JOIN cbms.alarm_notice_role ar ON adrr.role_id = ar.id\n        LEFT JOIN dcim.dict_major m ON adrr.major_id = m.id\n        LEFT JOIN cbms.alarm_notice_priority_dict apd ON adrr.priority = apd.id\n        <if test=\"noticeLevelIds != null and noticeLevelIds.size() != 0\">\n            LEFT JOIN cbms.alarm_notice_rule nr ON ar.id = nr.role_id\n        </if>\n        <where>\n            AND ar.status = 1\n            <if test=\"null != account and '' != account\">\n                AND sa.account = #{account}\n            </if>\n            <if test=\"null != dcGid\">\n                AND gdc.max_id &gt;= #{dcGid}\n                AND gdc.min_id &lt;= #{dcGid}\n            </if>\n            <if test=\"null != roleIds and roleIds.size() > 0\">\n                AND ar.id IN\n                <foreach collection=\"roleIds\" item=\"roleId\" open=\"(\" separator=\",\" close=\")\">\n                    #{roleId}\n                </foreach>\n            </if>\n            <if test=\"null != priority\">\n                AND apd.id = #{priority}\n            </if>\n            <if test=\"null != majorIds and majorIds.size() > 0\">\n                AND m.id IN\n                <foreach collection=\"majorIds\" item=\"majorId\" open=\"(\" separator=\",\" close=\")\">\n                    #{majorId}\n                </foreach>\n            </if>\n            <if test=\"dcIds != null and dcIds.size() != 0\">\n                AND dct.dc_id IN\n                <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" separator=\",\" close=\")\">\n                    #{dcId}\n                </foreach>\n            </if>\n            <if test=\"dcGids != null and dcGids.size() != 0\">\n                AND gdc.gid IN\n                <foreach collection=\"dcGids\" item=\"dcGid\" open=\"(\" separator=\",\" close=\")\">\n                    #{dcGid}\n                </foreach>\n            </if>\n            <if test=\"noticeLevelIds != null and noticeLevelIds.size() != 0\">\n                AND nr.notice_level_id IN\n                <foreach collection=\"noticeLevelIds\" item=\"noticeLevelId\" open=\"(\" separator=\",\" close=\")\">\n                    #{noticeLevelId}\n                </foreach>\n            </if>\n        </where>\n        ORDER BY gdc.gid, ar.role_type_id DESC, CONVERT(ar.name using gbk), m.id, apd.id, CONVERT(sa.name using gbk)\n    </select>\n\n    <select id=\"queryInnerMatrix\" parameterType=\"com.gds.jpi.dcim.vo.AlarmInnerGetReqVO\"\n            resultType=\"com.gds.jpi.dcim.vo.AlarmInnerNoticeVO\">\n        SELECT\n        rel.id AS innerId,\n        a.name AS accountName,\n        rel.account AS account,\n        pp.gid AS dcGid,\n        pp.short_name AS dcLimitation,\n        role.id AS roleId,\n        role.name AS roleName,\n        role.role_type_id AS roleTypeId,\n        role.phone_priority AS phonePriority,\n        apd.id AS priorityId,\n        apd.name AS priorityName,\n        m.id AS majorId,\n        m.name AS majorName,\n        a.email as email\n        FROM\n        dcim.event_notice_level l  <!-- Notification level -->\n        INNER JOIN\n        cbms.alarm_notice_rule rule ON rule.notice_level_id = l.id <!-- Associate notification rules based on notification level -->\n        INNER JOIN\n        cbms.alarm_notice_role role ON role.id = rule.role_id <!-- Match the notification role in the corresponding notification rule-->\n        INNER JOIN\n        cbms.alarm_notice_account_dc_role_r rel ON rel.role_id = role.id <!-- Preliminary screening of people based on role-->\n        INNER JOIN\n        (\n        <!-- Combine the condition of dc_id to match the data center operation master data at the DC level -->\n        <!-- Associate all parent nodes (National-Region-Park-Data Center)-->\n        SELECT DISTINCT p.* FROM geo.dc_tree t\n        INNER JOIN geo.dc_tree p ON p.min_id &lt;= t.gid AND p.max_id &gt;= t.gid\n        WHERE 1=1\n        <!-- Configure the DC level in the data center operation master data -->\n        <if test=\"dcIds != null and dcIds.size() != 0\">\n            AND t.dc_id IN\n            <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" separator=\",\" close=\")\">\n                #{dcId}\n            </foreach>\n        </if>\n        ) pp ON rel.dc_gid = pp.gid\n        <!-- Configure the notification person's account -->\n        LEFT JOIN\n        sms.account a ON a.account = rel.account\n        <!-- Configure the notification person's major, the notification person may not have a corresponding major -->\n        LEFT JOIN\n        dcim.dict_major m ON rel.major_id = m.id\n        <!-- Configure the notification person's A/B role, the notification person may not have an A/B role -->\n        LEFT JOIN\n        cbms.alarm_notice_priority_dict apd ON rel.priority = apd.id\n        WHERE\n        a.status = 1 <!-- The account must be valid -->\n        AND role.status = 1 <!-- The role must be valid -->\n        <!-- Find the corresponding notification rule based on the notification level -->\n        <if test=\"noticeLevelIds != null and noticeLevelIds.size() != 0\">\n            AND rule.notice_level_id IN\n            <foreach collection=\"noticeLevelIds\" item=\"noticeLevelId\" open=\"(\" separator=\",\" close=\")\">\n                #{noticeLevelId}\n            </foreach>\n        </if>\n        <!-- Screen suitable comrades based on majors, some notification persons do not have a major -->\n        <if test=\"null != majorIds and majorIds.size() > 0\">\n            AND (\n                m.id IN\n            <foreach collection=\"majorIds\" item=\"majorId\" open=\"(\" separator=\",\" close=\")\">\n                #{majorId}\n            </foreach>\n                OR m.id IS NULL\n                )\n        </if>\n    </select>\n</mapper>",
  "details": [
    {
      "lineNumber": 27,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "通知等级",
      "translatedText": "Notification level"
    },
    {
      "lineNumber": 29,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "根据通知等级关联相应通知规则",
      "translatedText": "Associate notification rules based on notification level"
    },
    {
      "lineNumber": 31,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "匹配相应通知规则中的通知角色",
      "translatedText": "Match the notification role in the corresponding notification rule"
    },
    {
      "lineNumber": 33,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "根据角色初步筛选到人",
      "translatedText": "Preliminary screening of people based on role"
    },
    {
      "lineNumber": 37,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "结合dc_id的条件匹配数据中心运营主数据中的DC这一级",
      "translatedText": "Combine the condition of dc_id to match the data center operation master data at the DC level"
    },
    {
      "lineNumber": 39,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "关联所有上级节点（全国-区域-园区-数据中心）",
      "translatedText": "Associate all parent nodes (National-Region-Park-Data Center)"
    },
    {
      "lineNumber": 42,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "配置数据中心运营主数据中的DC级",
      "translatedText": "Configure the DC level in the data center operation master data"
    },
    {
      "lineNumber": 48,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "配置通知人的账户",
      "translatedText": "Configure the notification person's account"
    },
    {
      "lineNumber": 51,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "配置通知人的专业，通知人可能无对应专业",
      "translatedText": "Configure the notification person's major, the notification person may not have a corresponding major"
    },
    {
      "lineNumber": 54,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "配置通知人的A/B角，通知人可能无A/B角",
      "translatedText": "Configure the notification person's A/B role, the notification person may not have an A/B role"
    },
    {
      "lineNumber": 57,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "账户必须有效",
      "translatedText": "The account must be valid"
    },
    {
      "lineNumber": 59,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "角色必须有效",
      "translatedText": "The role must be valid"
    },
    {
      "lineNumber": 62,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "根据通知等级找到对应的通知规则",
      "translatedText": "Find the corresponding notification rule based on the notification level"
    },
    {
      "lineNumber": 66,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "根据专业筛选合适的同志人，有的通知人是没有专业的",
      "translatedText": "Screen suitable comrades based on majors, some notification persons do not have a major"
    }
  ]
}
```

### DEMOS7

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmSyncMapper">
    <select id="fetchEventDetail" resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.EventDetail">
        SELECT
            e.id AS event_id,
            e.event_name,
            e.event_type_id,
            e.temp_event_id,
            e.temp_event_name,
            e.location_id,
            e.location_name,
            e.location_full_name,
            e.event_target_category_id,
            e.event_target_type_id,
            e.event_target_type_name,
            t.event_major_id AS major_id,
            e.dc_id,
            dm.dc_name_inner_short AS dc_name,
            e.event_target_inst_id,
            e.event_target_inst_name,
            e.seq
        FROM
            dcim.event_inst e
        JOIN
            dcim.std_temp_event t ON t.id = e.temp_event_id
        JOIN
            geo.dc_mapping dm ON dm.id = e.dc_id
        WHERE
            e.id = #{eventId}
    </select>
    <insert id="createAlarmDetail" keyProperty="detail.id" useGeneratedKeys="true" parameterType="com.gds.jpi.dcim.model.alarm.AlarmActDetail">
        INSERT INTO
            dcim.alarm_act_detail (event_id, alarm_id, event_type_id, message_type, event_level_id, break_time, event_rule_id)
        VALUES
            (#{detail.eventId}, #{detail.alarmId}, #{detail.eventTypeId}, #{detail.messageType}, #{detail.eventLevelId}, #{detail.breakTime}, #{detail.eventRuleId});
    </insert>
    <insert id="createAlarmDetailData">
        INSERT INTO
            dcim.alarm_act_detail_data (alarm_detail_id, sys_attr_id, value, show_value, unit, enum_value_code, collect_time, device_id, device_name, sys_attr_name)
        VALUES
        <foreach collection="alarmData" item="item" separator=",">
            (#{detailId}, #{item.sysAttrId}, #{item.value}, #{item.showValue}, #{item.dataUnit}, #{item.enumCode}, #{item.collectTime}, #{item.deviceId}, #{item.deviceName}, #{item.sysAttrName})
        </foreach>
    </insert>
    <update id="updateAlarmDetailData">
        UPDATE
            dcim.alarm_act_detail_data a
        JOIN
            cbms.sys_device_attr da ON da.sys_attr_id = a.sys_attr_id
        LEFT JOIN
            cbms.sys_device d ON d.device_id = da.device_id
        SET
            a.device_id = da.device_id,
            a.device_name = d.`name`,
            a.sys_attr_name = da.`name`
        WHERE
            a.alarm_detail_id = #{detailId}
    </update>
    <update id="updateAlarmDetailAllData">
        UPDATE
            dcim.alarm_act_detail_data a
        JOIN
            cbms.sys_device_attr da ON da.sys_attr_id = a.sys_attr_id
        JOIN
            dcim.std_temp_attr ta ON ta.temp_attr_id = da.temp_attr_id
        LEFT JOIN
            dcim.std_enum_dict ed ON ta.enum_type_code = ed.type_code AND ed.value = a.value
        LEFT JOIN
            cbms.sys_device d ON d.device_id = da.device_id
        SET
            a.show_value = IFNULL(ed.name, ROUND(a.value, ta.decimal_digit)),
            a.unit = ta.unit,
            a.enum_value_code = ed.code,
            a.device_id = da.device_id,
            a.device_name = d.`name`,
            a.sys_attr_name = da.`name`
        WHERE
            a.alarm_detail_id = #{detailId}
    </update>
</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmSyncMapper\">\n    <select id=\"fetchEventDetail\" resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.EventDetail\">\n        SELECT\n            e.id AS event_id,\n            e.event_name,\n            e.event_type_id,\n            e.temp_event_id,\n            e.temp_event_name,\n            e.location_id,\n            e.location_name,\n            e.location_full_name,\n            e.event_target_category_id,\n            e.event_target_type_id,\n            e.event_target_type_name,\n            t.event_major_id AS major_id,\n            e.dc_id,\n            dm.dc_name_inner_short AS dc_name,\n            e.event_target_inst_id,\n            e.event_target_inst_name,\n            e.seq\n        FROM\n            dcim.event_inst e\n        JOIN\n            dcim.std_temp_event t ON t.id = e.temp_event_id\n        JOIN\n            geo.dc_mapping dm ON dm.id = e.dc_id\n        WHERE\n            e.id = #{eventId}\n    </select>\n    <insert id=\"createAlarmDetail\" keyProperty=\"detail.id\" useGeneratedKeys=\"true\" parameterType=\"com.gds.jpi.dcim.model.alarm.AlarmActDetail\">\n        INSERT INTO\n            dcim.alarm_act_detail (event_id, alarm_id, event_type_id, message_type, event_level_id, break_time, event_rule_id)\n        VALUES\n            (#{detail.eventId}, #{detail.alarmId}, #{detail.eventTypeId}, #{detail.messageType}, #{detail.eventLevelId}, #{detail.breakTime}, #{detail.eventRuleId});\n    </insert>\n    <insert id=\"createAlarmDetailData\">\n        INSERT INTO\n            dcim.alarm_act_detail_data (alarm_detail_id, sys_attr_id, value, show_value, unit, enum_value_code, collect_time, device_id, device_name, sys_attr_name)\n        VALUES\n        <foreach collection=\"alarmData\" item=\"item\" separator=\",\">\n            (#{detailId}, #{item.sysAttrId}, #{item.value}, #{item.showValue}, #{item.dataUnit}, #{item.enumCode}, #{item.collectTime}, #{item.deviceId}, #{item.deviceName}, #{item.sysAttrName})\n        </foreach>\n    </insert>\n    <update id=\"updateAlarmDetailData\">\n        UPDATE\n            dcim.alarm_act_detail_data a\n        JOIN\n            cbms.sys_device_attr da ON da.sys_attr_id = a.sys_attr_id\n        LEFT JOIN\n            cbms.sys_device d ON d.device_id = da.device_id\n        SET\n            a.device_id = da.device_id,\n            a.device_name = d.`name`,\n            a.sys_attr_name = da.`name`\n        WHERE\n            a.alarm_detail_id = #{detailId}\n    </update>\n    <update id=\"updateAlarmDetailAllData\">\n        UPDATE\n            dcim.alarm_act_detail_data a\n        JOIN\n            cbms.sys_device_attr da ON da.sys_attr_id = a.sys_attr_id\n        JOIN\n            dcim.std_temp_attr ta ON ta.temp_attr_id = da.temp_attr_id\n        LEFT JOIN\n            dcim.std_enum_dict ed ON ta.enum_type_code = ed.type_code AND ed.value = a.value\n        LEFT JOIN\n            cbms.sys_device d ON d.device_id = da.device_id\n        SET\n            a.show_value = IFNULL(ed.name, ROUND(a.value, ta.decimal_digit)),\n            a.unit = ta.unit,\n            a.enum_value_code = ed.code,\n            a.device_id = da.device_id,\n            a.device_name = d.`name`,\n            a.sys_attr_name = da.`name`\n        WHERE\n            a.alarm_detail_id = #{detailId}\n    </update>\n</mapper>",
  "details": []
}
```


### DEMOS8

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.CfgBmsDeviceMapper">
    <insert id="batchInsert" useGeneratedKeys="true" keyProperty="id">
        insert into `cbms`.`cfg_bms_device`(agent_id, device_no, device_name, parent_no, update_time, find_time, is_del) values
            <foreach collection="cfgBmsDevices" item="bmsDevice" index="index" separator=",">
                (#{bmsDevice.agentId}, #{bmsDevice.deviceNo}, #{bmsDevice.deviceName}, #{bmsDevice.parentNo}, #{bmsDevice.updateTime}, #{bmsDevice.findTime}, #{bmsDevice.isDel})
            </foreach>
    </insert>

    <insert id="batchInsertV7" useGeneratedKeys="true" keyProperty="id">
        insert into `cbms`.`cfg_bms_device`(agent_id, device_no, device_name, station_no, update_time,station_name, find_time, is_del) values
        <foreach collection="cfgBmsDevices" item="bmsDevice" index="index" separator=",">
            (#{bmsDevice.agentId}, #{bmsDevice.deviceNo}, #{bmsDevice.deviceName}, #{bmsDevice.stationNo}, #{bmsDevice.updateTime},#{bmsDevice.stationName}, #{bmsDevice.findTime}, #{bmsDevice.isDel})
        </foreach>
    </insert>

    <update id="batchModifyUpdateTime">
        update `cbms`.`cfg_bms_device` set update_time=now(), is_del =0
        where id in
        <foreach item="item" index="index" collection="cfgBmsDevices" open="(" separator="," close=")">
            #{item.id}
        </foreach>
    </update>

    <update id="batchUpdate">
        update `cbms`.`cfg_bms_device`
        <trim prefix="set" suffixOverrides=",">
            <trim prefix="agent_id = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    <if test="item.agentId!=null">
                        when id=#{item.id} then #{item.agentId}
                    </if>
                </foreach>
            </trim>
            <trim prefix="device_no = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    <if test="item.deviceNo!=null">
                        when id=#{item.id} then #{item.deviceNo}
                    </if>
                </foreach>
            </trim>
            <trim prefix="device_name = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    <if test="item.deviceName!=null">
                        when id=#{item.id} then #{item.deviceName}
                    </if>
                </foreach>
            </trim>
            <trim prefix="station_name = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    <if test="item.stationName!=null">
                        when id=#{item.id} then #{item.stationName}
                    </if>
                </foreach>
            </trim>
            <trim prefix="station_no = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    <if test="item.stationNo!=null">
                        when id=#{item.id} then #{item.stationNo}
                    </if>
                </foreach>
            </trim>
            <trim prefix="parent_no = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    <if test="item.parentNo!=null">
                        when id=#{item.id} then #{item.parentNo}
                    </if>
                </foreach>
            </trim>
            <trim prefix="update_time = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    when id=#{item.id} then now()
                </foreach>
            </trim>
            <trim prefix="find_time = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    when id=#{item.id} then now()
                </foreach>
            </trim>
            <trim prefix="is_del = case" suffix="end,">
                <foreach collection="cfgBmsDevices" item="item">
                    when id=#{item.id} then 0
                </foreach>
            </trim>
        </trim>
        where id in
        <foreach item="item" index="index" collection="cfgBmsDevices" open="(" separator="," close=")">
            #{item.id}
        </foreach>
    </update>
    <select id="fetchVersion" resultType="java.lang.String">
        SELECT
            MD5(GROUP_CONCAT(IFNULL(UPDATE_TIME, ''))) hash_code
        FROM
            information_schema.tables
        WHERE
            (TABLE_SCHEMA = 'cbms' AND TABLE_NAME = 'sys_device_attr')
    </select>
    <select id="findByAgentIdAndDeviceNos" resultType="com.gds.jpi.dcim.model.cfg.CfgBmsDevice">
        select a.*
        from cbms.cfg_bms_device a
        where a.agent_id = #{agentId} and  a.device_no in
        <foreach item="item" index="index" collection="deviceNos" open="(" separator="," close=")">
            #{item}
        </foreach>
            and a.is_del = 0;
    </select>

    <update id="deleteByAgentId">
        update cbms.cfg_bms_device
        set is_del = 1
        where agent_id = #{agentId} and (update_time is null or update_time <![CDATA[ < ]]> date_add(now(), interval -7 day))
    </update>

</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.CfgBmsDeviceMapper\">\n    <insert id=\"batchInsert\" useGeneratedKeys=\"true\" keyProperty=\"id\">\n        insert into `cbms`.`cfg_bms_device`(agent_id, device_no, device_name, parent_no, update_time, find_time, is_del) values\n            <foreach collection=\"cfgBmsDevices\" item=\"bmsDevice\" index=\"index\" separator=\",\">\n                (#{bmsDevice.agentId}, #{bmsDevice.deviceNo}, #{bmsDevice.deviceName}, #{bmsDevice.parentNo}, #{bmsDevice.updateTime}, #{bmsDevice.findTime}, #{bmsDevice.isDel})\n            </foreach>\n    </insert>\n\n    <insert id=\"batchInsertV7\" useGeneratedKeys=\"true\" keyProperty=\"id\">\n        insert into `cbms`.`cfg_bms_device`(agent_id, device_no, device_name, station_no, update_time,station_name, find_time, is_del) values\n        <foreach collection=\"cfgBmsDevices\" item=\"bmsDevice\" index=\"index\" separator=\",\">\n            (#{bmsDevice.agentId}, #{bmsDevice.deviceNo}, #{bmsDevice.deviceName}, #{bmsDevice.stationNo}, #{bmsDevice.updateTime},#{bmsDevice.stationName}, #{bmsDevice.findTime}, #{bmsDevice.isDel})\n        </foreach>\n    </insert>\n\n    <update id=\"batchModifyUpdateTime\">\n        update `cbms`.`cfg_bms_device` set update_time=now(), is_del =0\n        where id in\n        <foreach item=\"item\" index=\"index\" collection=\"cfgBmsDevices\" open=\"(\" separator=\",\" close=\")\">\n            #{item.id}\n        </foreach>\n    </update>\n\n    <update id=\"batchUpdate\">\n        update `cbms`.`cfg_bms_device`\n        <trim prefix=\"set\" suffixOverrides=\",\">\n            <trim prefix=\"agent_id = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    <if test=\"item.agentId!=null\">\n                        when id=#{item.id} then #{item.agentId}\n                    </if>\n                </foreach>\n            </trim>\n            <trim prefix=\"device_no = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    <if test=\"item.deviceNo!=null\">\n                        when id=#{item.id} then #{item.deviceNo}\n                    </if>\n                </foreach>\n            </trim>\n            <trim prefix=\"device_name = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    <if test=\"item.deviceName!=null\">\n                        when id=#{item.id} then #{item.deviceName}\n                    </if>\n                </foreach>\n            </trim>\n            <trim prefix=\"station_name = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    <if test=\"item.stationName!=null\">\n                        when id=#{item.id} then #{item.stationName}\n                    </if>\n                </foreach>\n            </trim>\n            <trim prefix=\"station_no = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    <if test=\"item.stationNo!=null\">\n                        when id=#{item.id} then #{item.stationNo}\n                    </if>\n                </foreach>\n            </trim>\n            <trim prefix=\"parent_no = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    <if test=\"item.parentNo!=null\">\n                        when id=#{item.id} then #{item.parentNo}\n                    </if>\n                </foreach>\n            </trim>\n            <trim prefix=\"update_time = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    when id=#{item.id} then now()\n                </foreach>\n            </trim>\n            <trim prefix=\"find_time = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    when id=#{item.id} then now()\n                </foreach>\n            </trim>\n            <trim prefix=\"is_del = case\" suffix=\"end,\">\n                <foreach collection=\"cfgBmsDevices\" item=\"item\">\n                    when id=#{item.id} then 0\n                </foreach>\n            </trim>\n        </trim>\n        where id in\n        <foreach item=\"item\" index=\"index\" collection=\"cfgBmsDevices\" open=\"(\" separator=\",\" close=\")\">\n            #{item.id}\n        </foreach>\n    </update>\n    <select id=\"fetchVersion\" resultType=\"java.lang.String\">\n        SELECT\n            MD5(GROUP_CONCAT(IFNULL(UPDATE_TIME, ''))) hash_code\n        FROM\n            information_schema.tables\n        WHERE\n            (TABLE_SCHEMA = 'cbms' AND TABLE_NAME = 'sys_device_attr')\n    </select>\n    <select id=\"findByAgentIdAndDeviceNos\" resultType=\"com.gds.jpi.dcim.model.cfg.CfgBmsDevice\">\n        select a.*\n        from cbms.cfg_bms_device a\n        where a.agent_id = #{agentId} and  a.device_no in\n        <foreach item=\"item\" index=\"index\" collection=\"deviceNos\" open=\"(\" separator=\",\" close=\")\">\n            #{item}\n        </foreach>\n            and a.is_del = 0;\n    </select>\n\n    <update id=\"deleteByAgentId\">\n        update cbms.cfg_bms_device\n        set is_del = 1\n        where agent_id = #{agentId} and (update_time is null or update_time <![CDATA[ < ]]> date_add(now(), interval -7 day))\n    </update>\n\n</mapper>",
  "details": []
}
```


### DEMOS9

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.CustomerImpactMapper">

    <!-- 查询数据中心影响客户 -->
    <select id="findImpactCustomersByDc" resultType="com.gds.jpi.dcim.vo.CustomerImpactVO">
        SELECT DISTINCT
            cm.customer_id,
            cm.`name` AS customer_name,
            cm.alias AS customer_alias,
            IF(cm.customer_id = cp.end_customer_id, '甲方&amp;最终用户', '甲方') AS type
        FROM cip.cip_project cp
        JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code
        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id
        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id
        WHERE
            cp.status = 0
          AND rpd.is_released = 0
          AND cm.customer_id IS NOT NULL
          <if test="dcIds != null and dcIds.size() > 0">
            AND rpd.dc_id IN
            <foreach collection="dcIds" item="dcId" open="(" close=")" separator=",">
                #{dcId}
            </foreach>
          </if>
        UNION ALL
        SELECT DISTINCT
            ecm.customer_id,
            ecm.`name` AS customer_name,
            ecm.alias AS customer_alias,
            '最终用户' AS type
        FROM cip.cip_project cp
        JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code
        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id
        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id
        JOIN cip.cip_customer ecm ON ecm.customer_id = cp.end_customer_id
        WHERE
            cp.status = 0
          AND rpd.is_released = 0
          AND cm.customer_id IS NOT NULL
          <if test="dcIds != null and dcIds.size() > 0">
            AND rpd.dc_id IN
            <foreach collection="dcIds" item="dcId" open="(" close=")" separator=",">
                #{dcId}
            </foreach>
          </if>
          AND cp.end_customer_id IS NOT NULL
          AND cm.customer_id != cp.end_customer_id
        ORDER BY
            type,
            customer_name
    </select>

    <!-- 查询位置影响客户 -->
    <select id="findImpactCustomersByLocation" resultType="com.gds.jpi.dcim.vo.CustomerImpactVO">
        SELECT DISTINCT
            cm.customer_id,
            cm.`name` AS customer_name,
            cm.alias AS customer_alias,
            IF(cm.customer_id = cp.end_customer_id, '甲方&amp;最终用户', '甲方') AS type
        FROM cip.cip_project cp
        JOIN dcrm.rel_project_room rpd ON cp.project_code = rpd.project_code
        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id
        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id
        WHERE
            cp.status = 0
          AND rpd.is_released = 0
          AND cm.customer_id IS NOT NULL
          <if test="locationIds != null and locationIds.size() > 0">
            AND rpd.room_location_id IN
            <foreach collection="locationIds" item="locationId" open="(" close=")" separator=",">
                #{locationId}
            </foreach>
          </if>
        UNION ALL
        SELECT DISTINCT
            ecm.customer_id,
            ecm.`name` AS customer_name,
            ecm.alias AS customer_alias,
            '最终用户' AS type
        FROM cip.cip_project cp
        JOIN dcrm.rel_project_room rpd ON cp.project_code = rpd.project_code
        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id
        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id
        JOIN cip.cip_customer ecm ON ecm.customer_id = cp.end_customer_id
        WHERE
            cp.status = 0
          AND rpd.is_released = 0
          AND cm.customer_id IS NOT NULL
          <if test="locationIds != null and locationIds.size() > 0">
            AND rpd.room_location_id IN
            <foreach collection="locationIds" item="locationId" open="(" close=")" separator=",">
                #{locationId}
            </foreach>
          </if>
          AND cp.end_customer_id IS NOT NULL
          AND cm.customer_id != cp.end_customer_id
        ORDER BY
            type,
            customer_name
    </select>

    <!-- 查询机柜影响 -->
    <select id="findImpactCustomersByCabinet" resultType="com.gds.jpi.dcim.vo.CustomerImpactVO">
       SELECT DISTINCT
           cm.customer_id,
           cm.`name` AS customer_name,
           cm.alias AS customer_alias,
           IF(cm.customer_id = cp.end_customer_id, '甲方&amp;最终用户', '甲方') AS type
       FROM cip.cip_project cp
       JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id
       JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id
       JOIN cbms.sys_cabinet sc ON cp.project_code = sc.project_code
       JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code
       WHERE
           cp.status = 0
         AND rpd.is_released = 0
         AND sc.status = 1
         AND sc.elec_status = 1
        <if test="cabinetIds != null and cabinetIds.size() > 0">
         AND sc.cabinet_id IN
            <foreach collection="cabinetIds" item="cabinetId" open="(" close=")" separator=",">
                #{cabinetId}
            </foreach>
       </if>
       UNION ALL
       SELECT DISTINCT
           ecm.customer_id,
           ecm.`name` AS customer_name,
           ecm.alias AS customer_alias,
           '最终用户' AS type
       FROM cip.cip_project cp
       JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id
       JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id
       JOIN cbms.sys_cabinet sc ON cp.project_code = sc.project_code
       JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code
       JOIN cip.cip_customer ecm ON ecm.customer_id = cp.end_customer_id
       WHERE
           cp.status = 0
         AND rpd.is_released = 0
         AND sc.status = 1
         AND sc.elec_status = 1
         <if test="cabinetIds != null and cabinetIds.size() > 0">
           AND sc.cabinet_id IN
            <foreach collection="cabinetIds" item="cabinetId" open="(" close=")" separator=",">
                #{cabinetId}
            </foreach>
         </if>
         AND cp.end_customer_id IS NOT NULL
         AND cm.customer_id != cp.end_customer_id
    </select>

    <!-- 查询影响客户的ADM联系人（通过项目维度进行影响分析） -->
    <!--<select id="findCustomerImpactADMs" resultType="com.gds.jpi.dcim.vo.CustomerImpactAdmVO"
            parameterType="com.gds.jpi.dcim.dto.CustomerImpactContactDTO">
        SELECT DISTINCT
            cm.customer_id AS customer_id,
            cm.`name` AS customer_name,
            cp.end_customer_id,
            ecm.`name` AS end_customer_name,
            cc.contract_id AS contract_id,
            cp.project_code AS project_code,
            cp.pm_account AS adm_account,
            acc.`name` AS adm_name
        FROM cip.cip_customer cm
        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id
        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id
        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id
        LEFT JOIN sms.account acc ON cp.pm_account = acc.account
        WHERE cp.status = 0
          AND cp.pm_account IS NOT NULL
    <if test="(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)">
       AND (
        <trim prefixOverrides="OR">
        <if test="customerIds != null and customerIds.size() > 0">
          OR cc.customer_id IN
                  <foreach collection="customerIds" item="customerId" open="(" close=")" separator=",">
                    #{customerId}
                  </foreach>
        </if>
        <if test="endCustomerIds != null and endCustomerIds.size() > 0">
          OR cp.end_customer_id IN
                  <foreach collection="endCustomerIds" item="customerId" open="(" close=")" separator=",">
                    #{customerId}
                  </foreach>
        </if>
        </trim>
        )
    </if>
        <if test="dcIds != null and dcIds.size() > 0">
          AND EXISTS (
                SELECT 1
                FROM dcrm.rel_project_dc pd
                WHERE pd.project_code = cp.project_code
                  AND pd.is_released = 0
                  AND pd.dc_id IN
                  <foreach collection="dcIds" item="dcId" open="(" close=")" separator=",">
                    #{dcId}
                  </foreach>
            )
        </if>
        <if test="(locationIds != null and locationIds.size() > 0) || (roomIds != null and roomIds.size() > 0) ">
          AND EXISTS (
                SELECT 1
                FROM dcrm.rel_project_room pr
                LEFT JOIN dcrm.room r ON pr.room_location_id = r.location_id
                WHERE pr.project_code = cp.project_code
                  AND pr.is_released = 0
                  AND pr.room_location_id IN
                  <foreach collection="locationIds" item="locationId" open="(" close=")" separator=",">
                    #{locationId}
                  </foreach>
                  AND r.id IN
                  <foreach collection="roomIds" item="roomId" open="(" close=")" separator=",">
                    #{roomId}
                  </foreach>
            )
        </if>
        <if test="cabinetIds != null and cabinetIds.size() > 0">
          AND EXISTS (
                SELECT 1
                FROM cbms.sys_cabinet sc
                WHERE sc.project_code = cp.project_code
                  AND sc.elec_status = 1
                  AND sc.`status` = 1
                  AND sc.cabinet_id IN
                  <foreach collection="cabinetIds" item="cabinetId" open="(" close=")" separator=",">
                    #{cabinetId}
                  </foreach>
            )
        </if>
    </select>-->

    <select id="findCustomerImpactADMsByCabinet" resultType="com.gds.jpi.dcim.vo.CustomerImpactAdmVO"
            parameterType="com.gds.jpi.dcim.dto.CustomerImpactContactDTO">
        SELECT DISTINCT
        cm.customer_id AS customer_id,
        cm.`name` AS customer_name,
        cp.end_customer_id,
        ecm.`name` AS end_customer_name,
        cc.contract_id AS contract_id,
        cp.project_code AS project_code,
        cp.pm_account AS adm_account,
        acc.`name` AS adm_name
        FROM cip.cip_customer cm
        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id
        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id
        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id
        LEFT JOIN sms.account acc ON cp.pm_account = acc.account
        WHERE cp.status = 0
        AND cp.pm_account IS NOT NULL
        <if test="(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)">
            AND (
            <trim prefixOverrides="OR">
                <if test="customerIds != null and customerIds.size() > 0">
                    OR cc.customer_id IN
                    <foreach collection="customerIds" item="customerId" open="(" close=")" separator=",">
                        #{customerId}
                    </foreach>
                </if>
                <if test="endCustomerIds != null and endCustomerIds.size() > 0">
                    OR cp.end_customer_id IN
                    <foreach collection="endCustomerIds" item="customerId" open="(" close=")" separator=",">
                        #{customerId}
                    </foreach>
                </if>
            </trim>
            )
        </if>
            AND EXISTS (
            SELECT 1
            FROM cbms.sys_cabinet sc
            WHERE sc.project_code = cp.project_code
            AND sc.elec_status = 1
            AND sc.`status` = 1
           <if test="cabinetIds != null and cabinetIds.size() > 0">
             AND sc.cabinet_id IN
             <foreach collection="cabinetIds" item="cabinetId" open="(" close=")" separator=",">
                #{cabinetId}
             </foreach>
           </if>
            )
    </select>

    <select id="findCustomerImpactADMsByRoomOrLocation" resultType="com.gds.jpi.dcim.vo.CustomerImpactAdmVO"
            parameterType="com.gds.jpi.dcim.dto.CustomerImpactContactDTO">
        SELECT DISTINCT
        cm.customer_id AS customer_id,
        cm.`name` AS customer_name,
        cp.end_customer_id,
        ecm.`name` AS end_customer_name,
        cc.contract_id AS contract_id,
        cp.project_code AS project_code,
        cp.pm_account AS adm_account,
        acc.`name` AS adm_name
        FROM cip.cip_customer cm
        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id
        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id
        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id
        LEFT JOIN sms.account acc ON cp.pm_account = acc.account
        WHERE cp.status = 0
        AND cp.pm_account IS NOT NULL
        <if test="(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)">
            AND (
            <trim prefixOverrides="OR">
                <if test="customerIds != null and customerIds.size() > 0">
                    OR cc.customer_id IN
                    <foreach collection="customerIds" item="customerId" open="(" close=")" separator=",">
                        #{customerId}
                    </foreach>
                </if>
                <if test="endCustomerIds != null and endCustomerIds.size() > 0">
                    OR cp.end_customer_id IN
                    <foreach collection="endCustomerIds" item="customerId" open="(" close=")" separator=",">
                        #{customerId}
                    </foreach>
                </if>
            </trim>
            )
        </if>
            AND EXISTS (
            SELECT 1
            FROM dcrm.rel_project_room pr
            LEFT JOIN geo.room r ON pr.room_location_id = r.location_id
            WHERE pr.project_code = cp.project_code
            AND pr.is_released = 0
           <!-- <if test = "locationIds != null and locationIds.size()>0">
                  AND pr.room_location_id IN
                  <foreach collection="locationIds" item="locationId" open="(" close=")" separator=",">
                      #{locationId}
                  </foreach>
            </if>-->
            <if test="roomIds != null and roomIds.size() > 0">
                AND r.id IN
                <foreach collection="roomIds" item="roomId" open="(" close=")" separator=",">
                    #{roomId}
                </foreach>
            </if>
            )
    </select>

    <select id="findCustomerImpactADMsByDc" resultType="com.gds.jpi.dcim.vo.CustomerImpactAdmVO"
            parameterType="com.gds.jpi.dcim.dto.CustomerImpactContactDTO">
        SELECT DISTINCT
        cm.customer_id AS customer_id,
        cm.`name` AS customer_name,
        cp.end_customer_id,
        ecm.`name` AS end_customer_name,
        cc.contract_id AS contract_id,
        cp.project_code AS project_code,
        cp.pm_account AS adm_account,
        acc.`name` AS adm_name
        FROM cip.cip_customer cm
        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id
        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id
        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id
        LEFT JOIN sms.account acc ON cp.pm_account = acc.account
        WHERE cp.status = 0
        AND cp.pm_account IS NOT NULL
        <if test="(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)">
            AND (
            <trim prefixOverrides="OR">
                <if test="customerIds != null and customerIds.size() > 0">
                    OR cc.customer_id IN
                    <foreach collection="customerIds" item="customerId" open="(" close=")" separator=",">
                        #{customerId}
                    </foreach>
                </if>
                <if test="endCustomerIds != null and endCustomerIds.size() > 0">
                    OR cp.end_customer_id IN
                    <foreach collection="endCustomerIds" item="customerId" open="(" close=")" separator=",">
                        #{customerId}
                    </foreach>
                </if>
            </trim>
            )
        </if>
            AND EXISTS (
            SELECT 1
            FROM dcrm.rel_project_dc pd
            WHERE pd.project_code = cp.project_code
            AND pd.is_released = 0
            <if test="dcIds != null and dcIds.size() > 0">
             AND pd.dc_id IN
              <foreach collection="dcIds" item="dcId" open="(" close=")" separator=",">
                #{dcId}
              </foreach>
            </if>
            )
    </select>
</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.CustomerImpactMapper\">\n\n    <!-- Query data center impact customers -->\n    <select id=\"findImpactCustomersByDc\" resultType=\"com.gds.jpi.dcim.vo.CustomerImpactVO\">\n        SELECT DISTINCT\n            cm.customer_id,\n            cm.`name` AS customer_name,\n            cm.alias AS customer_alias,\n            IF(cm.customer_id = cp.end_customer_id, 'Party A &amp; Final User', 'Party A') AS type\n        FROM cip.cip_project cp\n        JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code\n        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id\n        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id\n        WHERE\n            cp.status = 0\n          AND rpd.is_released = 0\n          AND cm.customer_id IS NOT NULL\n          <if test=\"dcIds != null and dcIds.size() > 0\">\n            AND rpd.dc_id IN\n            <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" close=\")\" separator=\",\">\n                #{dcId}\n            </foreach>\n          </if>\n        UNION ALL\n        SELECT DISTINCT\n            ecm.customer_id,\n            ecm.`name` AS customer_name,\n            ecm.alias AS customer_alias,\n            'Final User' AS type\n        FROM cip.cip_project cp\n        JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code\n        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id\n        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id\n        JOIN cip.cip_customer ecm ON ecm.customer_id = cp.end_customer_id\n        WHERE\n            cp.status = 0\n          AND rpd.is_released = 0\n          AND cm.customer_id IS NOT NULL\n          <if test=\"dcIds != null and dcIds.size() > 0\">\n            AND rpd.dc_id IN\n            <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" close=\")\" separator=\",\">\n                #{dcId}\n            </foreach>\n          </if>\n          AND cp.end_customer_id IS NOT NULL\n          AND cm.customer_id != cp.end_customer_id\n        ORDER BY\n            type,\n            customer_name\n    </select>\n\n    <!-- Query location impact customers -->\n    <select id=\"findImpactCustomersByLocation\" resultType=\"com.gds.jpi.dcim.vo.CustomerImpactVO\">\n        SELECT DISTINCT\n            cm.customer_id,\n            cm.`name` AS customer_name,\n            cm.alias AS customer_alias,\n            IF(cm.customer_id = cp.end_customer_id, 'Party A &amp; Final User', 'Party A') AS type\n        FROM cip.cip_project cp\n        JOIN dcrm.rel_project_room rpd ON cp.project_code = rpd.project_code\n        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id\n        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id\n        WHERE\n            cp.status = 0\n          AND rpd.is_released = 0\n          AND cm.customer_id IS NOT NULL\n          <if test=\"locationIds != null and locationIds.size() > 0\">\n            AND rpd.room_location_id IN\n            <foreach collection=\"locationIds\" item=\"locationId\" open=\"(\" close=\")\" separator=\",\">\n                #{locationId}\n            </foreach>\n          </if>\n        UNION ALL\n        SELECT DISTINCT\n            ecm.customer_id,\n            ecm.`name` AS customer_name,\n            ecm.alias AS customer_alias,\n            'Final User' AS type\n        FROM cip.cip_project cp\n        JOIN dcrm.rel_project_room rpd ON cp.project_code = rpd.project_code\n        JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id\n        JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id\n        JOIN cip.cip_customer ecm ON ecm.customer_id = cp.end_customer_id\n        WHERE\n            cp.status = 0\n          AND rpd.is_released = 0\n          AND cm.customer_id IS NOT NULL\n          <if test=\"locationIds != null and locationIds.size() > 0\">\n            AND rpd.room_location_id IN\n            <foreach collection=\"locationIds\" item=\"locationId\" open=\"(\" close=\")\" separator=\",\">\n                #{locationId}\n            </foreach>\n          </if>\n          AND cp.end_customer_id IS NOT NULL\n          AND cm.customer_id != cp.end_customer_id\n        ORDER BY\n            type,\n            customer_name\n    </select>\n\n    <!-- Query cabinet impact -->\n    <select id=\"findImpactCustomersByCabinet\" resultType=\"com.gds.jpi.dcim.vo.CustomerImpactVO\">\n       SELECT DISTINCT\n           cm.customer_id,\n           cm.`name` AS customer_name,\n           cm.alias AS customer_alias,\n           IF(cm.customer_id = cp.end_customer_id, 'Party A &amp; Final User', 'Party A') AS type\n       FROM cip.cip_project cp\n       JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id\n       JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id\n       JOIN cbms.sys_cabinet sc ON cp.project_code = sc.project_code\n       JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code\n       WHERE\n           cp.status = 0\n         AND rpd.is_released = 0\n         AND sc.status = 1\n         AND sc.elec_status = 1\n        <if test=\"cabinetIds != null and cabinetIds.size() > 0\">\n         AND sc.cabinet_id IN\n            <foreach collection=\"cabinetIds\" item=\"cabinetId\" open=\"(\" close=\")\" separator=\",\">\n                #{cabinetId}\n            </foreach>\n       </if>\n       UNION ALL\n       SELECT DISTINCT\n           ecm.customer_id,\n           ecm.`name` AS customer_name,\n           ecm.alias AS customer_alias,\n           'Final User' AS type\n       FROM cip.cip_project cp\n       JOIN cip.cip_contract cc ON cp.contract_id = cc.contract_id\n       JOIN cip.cip_customer cm ON cc.customer_id = cm.customer_id\n       JOIN cbms.sys_cabinet sc ON cp.project_code = sc.project_code\n       JOIN dcrm.rel_project_dc rpd ON cp.project_code = rpd.project_code\n       JOIN cip.cip_customer ecm ON ecm.customer_id = cp.end_customer_id\n       WHERE\n           cp.status = 0\n         AND rpd.is_released = 0\n         AND sc.status = 1\n         AND sc.elec_status = 1\n         <if test=\"cabinetIds != null and cabinetIds.size() > 0\">\n           AND sc.cabinet_id IN\n            <foreach collection=\"cabinetIds\" item=\"cabinetId\" open=\"(\" close=\")\" separator=\",\">\n                #{cabinetId}\n            </foreach>\n         </if>\n         AND cp.end_customer_id IS NOT NULL\n         AND cm.customer_id != cp.end_customer_id\n    </select>\n\n    <!-- Query impact customer's ADM contacts (impact analysis through project dimension) -->\n    <!--<select id=\"findCustomerImpactADMs\" resultType=\"com.gds.jpi.dcim.vo.CustomerImpactAdmVO\"\n            parameterType=\"com.gds.jpi.dcim.dto.CustomerImpactContactDTO\">\n        SELECT DISTINCT\n            cm.customer_id AS customer_id,\n            cm.`name` AS customer_name,\n            cp.end_customer_id,\n            ecm.`name` AS end_customer_name,\n            cc.contract_id AS contract_id,\n            cp.project_code AS project_code,\n            cp.pm_account AS adm_account,\n            acc.`name` AS adm_name\n        FROM cip.cip_customer cm\n        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id\n        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id\n        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id\n        LEFT JOIN sms.account acc ON cp.pm_account = acc.account\n        WHERE cp.status = 0\n          AND cp.pm_account IS NOT NULL\n    <if test=\"(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)\">\n       AND (\n        <trim prefixOverrides=\"OR\">\n        <if test=\"customerIds != null and customerIds.size() > 0\">\n          OR cc.customer_id IN\n                  <foreach collection=\"customerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                    #{customerId}\n                  </foreach>\n        </if>\n        <if test=\"endCustomerIds != null and endCustomerIds.size() > 0\">\n          OR cp.end_customer_id IN\n                  <foreach collection=\"endCustomerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                    #{customerId}\n                  </foreach>\n        </if>\n        </trim>\n        \n    </if>\n        <if test=\"dcIds != null and dcIds.size() > 0\">\n          AND EXISTS (\n                SELECT 1\n                FROM dcrm.rel_project_dc pd\n                WHERE pd.project_code = cp.project_code\n                  AND pd.is_released = 0\n                  AND pd.dc_id IN\n                  <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" close=\")\" separator=\",\">\n                    #{dcId}\n                  </foreach>\n            \n        </if>\n        <if test=\"(locationIds != null and locationIds.size() > 0) || (roomIds != null and roomIds.size() > 0) \">\n          AND EXISTS (\n                SELECT 1\n                FROM dcrm.rel_project_room pr\n                LEFT JOIN dcrm.room r ON pr.room_location_id = r.location_id\n                WHERE pr.project_code = cp.project_code\n                  AND pr.is_released = 0\n                  AND pr.room_location_id IN\n                  <foreach collection=\"locationIds\" item=\"locationId\" open=\"(\" close=\")\" separator=\",\">\n                    #{locationId}\n                  </foreach>\n                  AND r.id IN\n                  <foreach collection=\"roomIds\" item=\"roomId\" open=\"(\" close=\")\" separator=\",\">\n                    #{roomId}\n                  </foreach>\n            \n        </if>\n        <if test=\"cabinetIds != null and cabinetIds.size() > 0\">\n          AND EXISTS (\n                SELECT 1\n                FROM cbms.sys_cabinet sc\n                WHERE sc.project_code = cp.project_code\n                  AND sc.elec_status = 1\n                  AND sc.`status` = 1\n                  AND sc.cabinet_id IN\n                  <foreach collection=\"cabinetIds\" item=\"cabinetId\" open=\"(\" close=\")\" separator=\",\">\n                    #{cabinetId}\n                  </foreach>\n            \n        </if>\n    </select>-->\n\n    <select id=\"findCustomerImpactADMsByCabinet\" resultType=\"com.gds.jpi.dcim.vo.CustomerImpactAdmVO\"\n            parameterType=\"com.gds.jpi.dcim.dto.CustomerImpactContactDTO\">\n        SELECT DISTINCT\n        cm.customer_id AS customer_id,\n        cm.`name` AS customer_name,\n        cp.end_customer_id,\n        ecm.`name` AS end_customer_name,\n        cc.contract_id AS contract_id,\n        cp.project_code AS project_code,\n        cp.pm_account AS adm_account,\n        acc.`name` AS adm_name\n        FROM cip.cip_customer cm\n        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id\n        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id\n        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id\n        LEFT JOIN sms.account acc ON cp.pm_account = acc.account\n        WHERE cp.status = 0\n        AND cp.pm_account IS NOT NULL\n        <if test=\"(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)\">\n            AND (\n            <trim prefixOverrides=\"OR\">\n                <if test=\"customerIds != null and customerIds.size() > 0\">\n                    OR cc.customer_id IN\n                    <foreach collection=\"customerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                        #{customerId}\n                    </foreach>\n                </if>\n                <if test=\"endCustomerIds != null and endCustomerIds.size() > 0\">\n                    OR cp.end_customer_id IN\n                    <foreach collection=\"endCustomerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                        #{customerId}\n                    </foreach>\n                </if>\n            </trim>\n            \n        </if>\n            AND EXISTS (\n            SELECT 1\n            FROM cbms.sys_cabinet sc\n            WHERE sc.project_code = cp.project_code\n            AND sc.elec_status = 1\n            AND sc.`status` = 1\n           <if test=\"cabinetIds != null and cabinetIds.size() > 0\">\n             AND sc.cabinet_id IN\n             <foreach collection=\"cabinetIds\" item=\"cabinetId\" open=\"(\" close=\")\" separator=\",\">\n                #{cabinetId}\n             </foreach>\n           </if>\n            \n        </select>\n\n    <select id=\"findCustomerImpactADMsByRoomOrLocation\" resultType=\"com.gds.jpi.dcim.vo.CustomerImpactAdmVO\"\n            parameterType=\"com.gds.jpi.dcim.dto.CustomerImpactContactDTO\">\n        SELECT DISTINCT\n        cm.customer_id AS customer_id,\n        cm.`name` AS customer_name,\n        cp.end_customer_id,\n        ecm.`name` AS end_customer_name,\n        cc.contract_id AS contract_id,\n        cp.project_code AS project_code,\n        cp.pm_account AS adm_account,\n        acc.`name` AS adm_name\n        FROM cip.cip_customer cm\n        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id\n        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id\n        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id\n        LEFT JOIN sms.account acc ON cp.pm_account = acc.account\n        WHERE cp.status = 0\n        AND cp.pm_account IS NOT NULL\n        <if test=\"(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)\">\n            AND (\n            <trim prefixOverrides=\"OR\">\n                <if test=\"customerIds != null and customerIds.size() > 0\">\n                    OR cc.customer_id IN\n                    <foreach collection=\"customerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                        #{customerId}\n                    </foreach>\n                </if>\n                <if test=\"endCustomerIds != null and endCustomerIds.size() > 0\">\n                    OR cp.end_customer_id IN\n                    <foreach collection=\"endCustomerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                        #{customerId}\n                    </foreach>\n                </if>\n            </trim>\n            \n        </if>\n            AND EXISTS (\n            SELECT 1\n            FROM dcrm.rel_project_room pr\n            LEFT JOIN geo.room r ON pr.room_location_id = r.location_id\n            WHERE pr.project_code = cp.project_code\n            AND pr.is_released = 0\n           <!-- <if test = \"locationIds != null and locationIds.size()>0\">\n                  AND pr.room_location_id IN\n                  <foreach collection=\"locationIds\" item=\"locationId\" open=\"(\" close=\")\" separator=\",\">\n                      #{locationId}\n                  </foreach>\n            </if>-->\n            <if test=\"roomIds != null and roomIds.size() > 0\">\n                AND r.id IN\n                <foreach collection=\"roomIds\" item=\"roomId\" open=\"(\" close=\")\" separator=\",\">\n                    #{roomId}\n                </foreach>\n            </if>\n            \n        </select>\n\n    <select id=\"findCustomerImpactADMsByDc\" resultType=\"com.gds.jpi.dcim.vo.CustomerImpactAdmVO\"\n            parameterType=\"com.gds.jpi.dcim.dto.CustomerImpactContactDTO\">\n        SELECT DISTINCT\n        cm.customer_id AS customer_id,\n        cm.`name` AS customer_name,\n        cp.end_customer_id,\n        ecm.`name` AS end_customer_name,\n        cc.contract_id AS contract_id,\n        cp.project_code AS project_code,\n        cp.pm_account AS adm_account,\n        acc.`name` AS adm_name\n        FROM cip.cip_customer cm\n        LEFT JOIN cip.cip_contract cc ON cc.customer_id = cm.customer_id\n        LEFT JOIN cip.cip_project cp ON cp.contract_id = cc.contract_id\n        LEFT JOIN cip.cip_customer ecm ON cp.end_customer_id = ecm.customer_id\n        LEFT JOIN sms.account acc ON cp.pm_account = acc.account\n        WHERE cp.status = 0\n        AND cp.pm_account IS NOT NULL\n        <if test=\"(customerIds != null and customerIds.size() > 0) or (endCustomerIds != null and endCustomerIds.size() > 0)\">\n            AND (\n            <trim prefixOverrides=\"OR\">\n                <if test=\"customerIds != null and customerIds.size() > 0\">\n                    OR cc.customer_id IN\n                    <foreach collection=\"customerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                        #{customerId}\n                    </foreach>\n                </if>\n                <if test=\"endCustomerIds != null and endCustomerIds.size() > 0\">\n                    OR cp.end_customer_id IN\n                    <foreach collection=\"endCustomerIds\" item=\"customerId\" open=\"(\" close=\")\" separator=\",\">\n                        #{customerId}\n                    </foreach>\n                </if>\n            </trim>\n            \n        </if>\n            AND EXISTS (\n            SELECT 1\n            FROM dcrm.rel_project_dc pd\n            WHERE pd.project_code = cp.project_code\n            AND pd.is_released = 0\n            <if test=\"dcIds != null and dcIds.size() > 0\">\n             AND pd.dc_id IN\n              <foreach collection=\"dcIds\" item=\"dcId\" open=\"(\" close=\")\" separator=\",\">\n                #{dcId}\n              </foreach>\n            </if>\n            \n        </select>\n</mapper>",
  "details": [
    {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "查询数据中心影响客户",
      "translatedText": "Query data center impact customers"
    },
    {
      "lineNumber": 54,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "查询位置影响客户",
      "translatedText": "Query location impact customers"
    },
    {
      "lineNumber": 104,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "查询机柜影响",
      "translatedText": "Query cabinet impact"
    },
    {
      "lineNumber": 157,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "查询影响客户的ADM联系人（通过项目维度进行影响分析）",
      "translatedText": "Query impact customer's ADM contacts (impact analysis through project dimension)"
    },
    {
      "lineNumber": 160,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "甲方&最终用户",
      "translatedText": "Party A & Final User"
    },
    {
      "lineNumber": 173,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "最终用户",
      "translatedText": "Final User"
    },
    {
      "lineNumber": 222,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "甲方&最终用户",
      "translatedText": "Party A & Final User"
    },
    {
      "lineNumber": 235,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "最终用户",
      "translatedText": "Final User"
    },
    {
      "lineNumber": 284,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "甲方&最终用户",
      "translatedText": "Party A & Final User"
    },
    {
      "lineNumber": 297,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "最终用户",
      "translatedText": "Final User"
    },
    {
      "lineNumber": 346,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "甲方&最终用户",
      "translatedText": "Party A & Final User"
    },
    {
      "lineNumber": 359,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "最终用户",
      "translatedText": "Final User"
    }
  ]
}
```
