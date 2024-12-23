## **XML:**

### INSTRUCTION

You are a professional code translator specializing in translating XML source code, documentation, and comments.

Your task is to translate all Chinese comments or text within the provided XML code into English.
Ensure the file structure, imports, and comments are preserved exactly as they are.
Keep all comments in their original format, including single-line or block comments.
Include line numbers and preserve alignment for readability.
Return the result as a plain text JSON object containing:

"translatedCode": The complete XML code translated into English, maintaining its formatting.
"details": A JSON array where each object contains:
"lineNumber": The exact line number of the translation.
"lineType": Whether the line is a comment, annotation, or other.
"jobType": The type of transformation, e.g., "Text Translation".
"originalText": The text before translation.
"translatedText": The translated text.

Ensure the line numbers in "details" align exactly with the original input file (do not add new line). Do not include markdown formatting (e.g., ```json) in the response. Return the JSON as plain text.

Keep entities like &gt;, &lt;, &amp;, &lt;=, &gt;= and symbols such as backticks (`) exactly as they are. Do not decode them into their literal counterparts.

### DEMOS1

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

```JSON {
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.AlarmInnerMapper\">\n  <select id=\"selectInnerMatrix\" parameterType=\"com.gds.jpi.dcim.vo.AlarmInnerGetReqVO\" resultType=\"com.gds.jpi.dcim.vo.AlarmInnerNoticeVO\">\n    SELECT DISTINCT\n      adrr.id AS innerId,\n      sa.name AS accountName,\n      adrr.account AS account,\n      gdc.gid AS dcGid,\n      gdc.short_name AS dcLimitation,\n      ar.id AS roleId,\n      ar.name AS roleName,\n      ar.role_type_id AS roleTypeId,\n      ar.phone_priority AS phonePriority,\n      apd.id AS priorityId,\n      apd.name AS priorityName,\n      m.id AS majorId,\n      m.name AS majorName\n    FROM cbms.alarm_notice_account_dc_role_r adrr\n    LEFT JOIN sms.account sa ON adrr.account = sa.account\n    LEFT JOIN geo.dc_tree gdc ON adrr.dc_gid = gdc.gid\n    LEFT JOIN geo.dc_tree dct ON dct.gid >= gdc.min_id AND gdc.max_id >= dct.gid\n    LEFT JOIN cbms.alarm_notice_role ar ON adrr.role_id = ar.id\n    LEFT JOIN dcim.dict_major m ON adrr.major_id = m.id\n    LEFT JOIN cbms.alarm_notice_priority_dict apd ON adrr.priority = apd.id\n    <if test=\"noticeLevelIds != null and noticeLevelIds.size() != 0\">\n      LEFT JOIN cbms.alarm_notice_rule nr ON ar.id = nr.role_id\n    </if>\n    <where>\n      AND ar.status = 1\n      <if test=\"null != account and '' != account\">\n        AND sa.account = #{account}\n      </if>\n      <if test=\"null != dcGid\">\n        AND gdc.max_id >= #{dcGid}\n        AND gdc.min_id <= #{dcGid}\n      </if>\n      <if test=\"null != roleIds and roleIds.size() > 0\">\n        AND ar.id IN\n        <foreach collection=\"roleIds\" item=\"roleId\" open=\"(\" separator=\",\" close=\")\">\n          #{roleId}\n        </foreach>\n      </if>\n      <if test=\"null != priority\">\n        AND apd.id = #{priority}\n      </if>\n      <if test=\"null != majorIds and majorIds.size() > 0\">\n        AND m.id IN\n        <foreach collection=\"majorIds\" item=\"majorId\" open=\"(\" separator=\",\" close=\")\">\n          #{majorId}\n        </foreach>\n      </if>\n    </where>\n    ORDER BY gdc.gid, ar.role_type_id DESC, CONVERT(ar.name using gbk), m.id, apd.id, CONVERT(sa.name using gbk)\n  </select>\n</mapper>",
  "details": [
    {
      "lineNumber": 75,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 通知等级 -->",
      "translatedText": "<!-- Notification Level -->"
    },
    {
      "lineNumber": 77,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 根据通知等级关联相应通知规则 -->",
      "translatedText": "<!-- Link the corresponding notification rule based on the notification level -->"
    },
    {
      "lineNumber": 79,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 匹配相应通知规则中的通知角色-->",
      "translatedText": "<!-- Match the corresponding notification role in the notification rule -->"
    },
    {
      "lineNumber": 81,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 根据角色初步筛选到人-->",
      "translatedText": "<!-- Filter people based on role -->"
    },
    {
      "lineNumber": 83,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 结合dc_id的条件匹配数据中心运营主数据中的DC这一级 -->",
      "translatedText": "<!-- Match the data center level in the main data of the data center operation based on dc_id condition -->"
    },
    {
      "lineNumber": 84,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 关联所有上级节点（全国-区域-园区-数据中心）-->",
      "translatedText": "<!-- Join all parent nodes (National-Region-Park-Data Center) -->"
    },
    {
      "lineNumber": 86,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 配置数据中心运营主数据中的DC级 -->",
      "translatedText": "<!-- Configure the DC level in the main data of the data center operation -->"
    },
    {
      "lineNumber": 92,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 配置通知人的账户 -->",
      "translatedText": "<!-- Configure the account of the notification person -->"
    },
    {
      "lineNumber": 94,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 配置通知人的专业，通知人可能无对应专业 -->",
      "translatedText": "<!-- Configure the major of the notification person, the notification person may not have a corresponding major -->"
    },
    {
      "lineNumber": 96,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 配置通知人的A/B角，通知人可能无A/B角 -->",
      "translatedText": "<!-- Configure the A/B role of the notification person, the notification person may not have an A/B role -->"
    },
    {
      "lineNumber": 98,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 账户必须有效 -->",
      "translatedText": "<!-- The account must be valid -->"
    },
    {
      "lineNumber": 99,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 角色必须有效 -->",
      "translatedText": "<!-- The role must be valid -->"
    },
    {
      "lineNumber": 101,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 根据通知等级找到对应的通知规则 -->",
      "translatedText": "<!-- Find the corresponding notification rule based on the notification level -->"
    },
    {
      "lineNumber": 103,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 根据专业筛选合适的同志人，有的通知人是没有专业的 -->",
      "translatedText": "<!-- Filter suitable notification people based on the major, some notification people may not have a major -->"
    }
  ]
}

```

### DEMOS2

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.AlarmNoticeDetailCustMapper">

    <resultMap type="com.gds.jpi.dcim.model.AlarmNoticeDetailCust" id="AlarmNoticeDetailCustMap">
        <result property="id" column="id" jdbcType="INTEGER"/>
        <result property="detailId" column="detail_id" jdbcType="INTEGER"/>
        <result property="customerId" column="customer_id" jdbcType="INTEGER"/>
        <result property="admAccount" column="adm_account" jdbcType="VARCHAR"/>
        <result property="contactId" column="contact_id" jdbcType="INTEGER"/>
    </resultMap>

    <!-- 获取告警进展通知已通知客户联系人员列表 -->
    <select id="findAlarmNoticeDetailCustomers" resultType="com.gds.jpi.dcim.vo.AlarmNoticeDetailCustomerVO"
            parameterType="java.lang.Integer">
           SELECT an.customer_id,
                  an.customer_name,
                  an.contact_account,
                  an.contact_name,
                  an.contact_id,
                  an.adm_account,
                  an.adm_name  FROM
            (
                SELECT DISTINCT andc.customer_id,
                             cust.name AS customer_name,
                             andd.account as contact_account,
                             NULL  AS contact_name,
                             andd.contact_id,
                             null as adm_account,
                             null as adm_name
             FROM cbms.alarm_notice_detail_customer andc
                      LEFT JOIN cbms.alarm_notice_detail_contact andd ON andc.id = andd.detail_customer_id
                      LEFT JOIN cip.cip_customer cust ON andc.customer_id = cust.customer_id
             WHERE andd.notifier_type = 0
               AND andc.detail_id = #{detail_id}
               AND andd.is_notice =1
             UNION ALL
             SELECT DISTINCT andc.customer_id,
                             NULL AS customer_name,
                             andd.account as contact_account,
                             NULL  AS contact_name,
                             andd.contact_id,
                             andd.account as adm_account,
                             a.name as adm_name
             FROM cbms.alarm_notice_detail_customer andc
                      LEFT JOIN cbms.alarm_notice_detail_contact andd ON andc.id = andd.detail_customer_id
                      LEFT JOIN sms.account a ON a.account = andd.account
             WHERE andd.notifier_type = 1
               AND andc.detail_id = #{detail_id}
               AND andd.is_notice = 1
                )  an
        ORDER BY an.customer_id
    </select>

    <select id="findAdmCustNoticeResults" resultType="com.gds.jpi.dcim.vo.AlarmNoticeResultCustVO">
        SELECT DISTINCT
        andc.customer_id,
        cust.NAME AS customer_name,
        acc.NAME,
        anpr.notice_result
        FROM cbms.alarm_notice_detail_customer andc
        INNER JOIN cbms.alarm_notice_detail_contact andcc ON andc.id = andcc.detail_customer_id and andcc.is_notice = 1
        INNER JOIN cbms.alarm_notice_detail nd ON andc.detail_id = nd.id
        INNER JOIN cbms.alarm_notice_phone_result anpr ON anpr.notice_id = nd.notice_id AND anpr.account = andcc.account
        INNER JOIN cip.cip_customer cust ON andc.customer_id = cust.customer_id
        LEFT JOIN sms.account acc ON andcc.account = acc.account
        WHERE nd.id = #{detailId}
        AND andcc.notifier_type = 1
    </select>

    <select id="findCustNoticeResults" resultType="com.gds.jpi.dcim.vo.AlarmNoticeResultCustVO">
        SELECT
            andc.customer_id,
            cust.NAME AS customer_name,
            andcc.contact_id,
            anpr.notice_result
        FROM cbms.alarm_notice_detail_customer andc
                 INNER JOIN cbms.alarm_notice_detail_contact andcc ON andc.id = andcc.detail_customer_id and andcc.is_notice = 1
                 INNER JOIN cbms.alarm_notice_detail nd ON andc.detail_id = nd.id
                 INNER JOIN cbms.alarm_notice_phone_result anpr ON anpr.notice_id = nd.notice_id AND anpr.account = andcc.account
                 INNER JOIN cip.cip_customer cust ON andc.customer_id = cust.customer_id
        WHERE nd.id = #{detailId}
          AND andcc.notifier_type = 0
    </select>
</mapper>
```

#### Output:

```json
{
"translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace="com.gds.jpi.dcim.core.mapper.AlarmNoticeDetailCustMapper">\n\n    <resultMap type="com.gds.jpi.dcim.model.AlarmNoticeDetailCust" id="AlarmNoticeDetailCustMap">\n        <result property="id" column="id" jdbcType="INTEGER"/>\n        <result property="detailId" column="detail_id" jdbcType="INTEGER"/>\n        <result property="customerId" column="customer_id" jdbcType="INTEGER"/>\n        <result property="admAccount" column="adm_account" jdbcType="VARCHAR"/>\n        <result property="contactId" column="contact_id" jdbcType="INTEGER"/>\n    </resultMap>\n\n    <!-- Get the list of customer contacts who have been notified of the alarm progress notification -->\n    <select id="findAlarmNoticeDetailCustomers" resultType="com.gds.jpi.dcim.vo.AlarmNoticeDetailCustomerVO"\n            parameterType="java.lang.Integer">\n           SELECT an.customer_id,\n                  an.customer_name,\n                  an.contact_account,\n                  an.contact_name,\n                  an.contact_id,\n                  an.adm_account,\n                  an.adm_name  FROM\n           (\n                SELECT DISTINCT andc.customer_id,\n                             cust.name AS customer_name,\n                             andd.account as contact_account,\n                             NULL  AS contact_name,\n                             andd.contact_id,\n                             null as adm_account,\n                             null as adm_name\n             FROM cbms.alarm_notice_detail_customer andc\n                      LEFT JOIN cbms.alarm_notice_detail_contact andd ON andc.id = andd.detail_customer_id\n                      LEFT JOIN cip.cip_customer cust ON andc.customer_id = cust.customer_id\n             WHERE andd.notifier_type = 0\n               AND andc.detail_id = #{detail_id}\n               AND andd.is_notice =1\n             UNION ALL\n             SELECT DISTINCT andc.customer_id,\n                             NULL AS customer_name,\n                             andd.account as contact_account,\n                             NULL  AS contact_name,\n                             andd.contact_id,\n                             andd.account as adm_account,\n                             a.name as adm_name\n             FROM cbms.alarm_notice_detail_customer andc\n                      LEFT JOIN cbms.alarm_notice_detail_contact andd ON andc.id = andd.detail_customer_id\n                      LEFT JOIN sms.account a ON a.account = andd.account\n             WHERE andd.notifier_type = 1\n               AND andc.detail_id = #{detail_id}\n               AND andd.is_notice = 1\n                )  an\n        ORDER BY an.customer_id\n    </select>\n\n    <select id="findAdmCustNoticeResults" resultType="com.gds.jpi.dcim.vo.AlarmNoticeResultCustVO">\n        SELECT DISTINCT\n        andc.customer_id,\n        cust.NAME AS customer_name,\n        acc.NAME,\n        anpr.notice_result\n        FROM cbms.alarm_notice_detail_customer andc\n        INNER JOIN cbms.alarm_notice_detail_contact andcc ON andc.id = andcc.detail_customer_id and andcc.is_notice = 1\n        INNER JOIN cbms.alarm_notice_detail nd ON andc.detail_id = nd.id\n        INNER JOIN cbms.alarm_notice_phone_result anpr ON anpr.notice_id = nd.notice_id AND anpr.account = andcc.account\n        INNER JOIN cip.cip_customer cust ON andc.customer_id = cust.customer_id\n        LEFT JOIN sms.account acc ON andcc.account = acc.account\n        WHERE nd.id = #{detailId}\n        AND andcc.notifier_type = 1\n    </select>\n\n    <select id="findCustNoticeResults" resultType="com.gds.jpi.dcim.vo.AlarmNoticeResultCustVO">\n        SELECT\n            andc.customer_id,\n            cust.NAME AS customer_name,\n            andcc.contact_id,\n            anpr.notice_result\n        FROM cbms.alarm_notice_detail_customer andc\n                 INNER JOIN cbms.alarm_notice_detail_contact andcc ON andc.id = andcc.detail_customer_id and andcc.is_notice = 1\n                 INNER JOIN cbms.alarm_notice_detail nd ON andc.detail_id = nd.id\n                 INNER JOIN cbms.alarm_notice_phone_result anpr ON anpr.notice_id = nd.notice_id AND anpr.account = andcc.account\n                 INNER JOIN cip.cip_customer cust ON andc.customer_id = cust.customer_id\n        WHERE nd.id = #{detailId}\n          AND andcc.notifier_type = 0\n    </select></mapper>",
"details": [
    {
      "lineNumber": 16,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- 获取告警进展通知已通知客户联系人员列表 -->",
      "translatedText": "<!-- Get the list of customer contacts who have been notified of the alarm progress notification -->"
    }
]
}
```

### DEMOS3

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.SysDeviceMapper">

    <!--  属性条件  -->
    <sql id="attrsConditions">
        WHERE d.status = 1
        AND d.is_monitoring = 1
        AND temp.is_del = 0
        <if test="deviceIdList != null and deviceIdList.size() > 0">
            AND d.device_id IN
            <foreach collection="deviceIdList" separator="," open="(" close=")" item="item">
                #{item}
            </foreach>
        </if>
        <if test="codeList != null and codeList.size() > 0">
            AND temp.code IN
            <foreach collection="codeList" separator="," open="(" close=")" item="item">
                #{item}
            </foreach>
        </if>
    </sql>

    <select id="searchSysDevices" resultType="com.gds.jpi.dcim.model.SysDevice">
        SELECT device.*
        FROM cbms.sys_device device
                 LEFT JOIN geo.dc_mapping dc ON dc.id = device.dc_id
        WHERE device.status = 1
          AND dc.dc_name_outer_full like CONCAT('%', CONCAT(#{dcName}, '%'))


    </select>
    <select id="searchLocationByDcs" resultType="com.gds.jpi.dcim.model.GeoLocation">
        SELECT a.*
        FROM geo.location a
        LEFT JOIN geo.dc_location b on a.id = b.location_id
        LEFT JOIN geo.dc_mapping c on b.dc_id = c.id
        WHERE a.status = 1
        AND c.id in
        <foreach collection="dcIds" index="index" item="item" open="(" separator="," close=")">
            #{item}
        </foreach>
    </select>
    <select id="searchAllChildLocations" resultType="com.gds.jpi.dcim.model.GeoLocation">
        select a.*
        from geo.location a
        where a.status = 1 and
        <foreach collection="parentLocations" item="location" open=" (" close=")" separator="or">
            ( a.`left` <![CDATA[ >= ]]>  #{location.left} and a.`right` <![CDATA[ <= ]]> #{location.right} )
        </foreach>
        order by a.`left`
    </select>

    <select id="list" resultType="com.gds.jpi.dcim.vo.DeviceVO" parameterType="com.gds.jpi.dcim.vo.DeviceReqVO">
        SELECT
        sd.device_id ,
        sd.name,
        sd.name AS device_name,
        sd.short_name,
        sd.identify_code,
        sd.ga_code,
        sd.cus_code,
        sd.location_id,
        gl.path AS location_name,
        sd.length,
        sd.width,
        sd.height,
        sd.remark,
        sd.dc_id,
        dm.`dc_name_outer_short` dcName,
        aat.id typeId,
        aat.code typeCode,
        aat.`name` typeName,
        sd.`has_attachment`,
        sd.`is_monitoring`,
        sd.p_device_id,
        CASE
        WHEN sd.p_device_id IS NULL THEN
        false
        ELSE true
        END AS isAttachment,
        IFNULL(dtd.has_std, 0) has_std,
        am.id modelId,
        am.`model` modelName,
        amf.id mnftId,
        amf.`name` mnftName,
        sd.asset_status
        FROM
        cbms.sys_device sd
        LEFT JOIN asset.asset_type aat ON sd.asset_type_id = aat.id
        LEFT JOIN dcim.sys_device_type_detail dtd  ON dtd.asset_type_id = aat.id
        LEFT JOIN asset.model am ON sd.model_id = am.id
        LEFT JOIN asset.manufacture amf ON amf.id = sd.mnft_id
        LEFT JOIN geo.dc_mapping dm ON dm.id = sd.dc_id
        LEFT JOIN geo.location gl ON sd.location_id = gl.id
        <where>
            sd.`status` = true
            <if test="dcId != null">
                AND sd.`dc_id` = #{dcId}
            </if>
            <if test="cusCodes != null and cusCodes.size() > 0">
                AND sd.cus_code IN
                <foreach collection="cusCodes" item="cusCode" open="(" separator="," close=")">
                    #{cusCode}
                </foreach>
            </if>
            <if test="authDcs != null and authDcs.size() > 0">
                AND sd.`dc_id` IN
                <foreach collection="authDcs" item="authDc" open="(" separator="," close=")">
                    #{authDc}
                </foreach>
            </if>
            <if test="dcIds != null and dcIds.size() > 0">
                AND sd.`dc_id` IN
                <foreach collection="dcIds" item="dc" open="(" separator="," close=")">
                    #{dc}
                </foreach>
            </if>
            <if test="locationIds != null and locationIds.size() > 0">
                AND sd.location_id IN
                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">
                    #{locationId}
                </foreach>
            </if>
            <if test="typeIds != null and typeIds.size() > 0">
                AND sd.asset_type_id IN
                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">
                    #{typeId}
                </foreach>
            </if>
            <if test="typeCodes != null and typeCodes.size() > 0">
                AND aat.code IN
                <foreach collection="typeCodes" open="(" close=")" separator="," item="typeCode">
                    #{typeCode}
                </foreach>
            </if>
            <if test="mnftIds != null and mnftIds.size() > 0">
                AND sd.mnft_id IN
                <foreach collection="mnftIds" open="(" close=")" separator="," item="mnftId">
                    #{mnftId}
                </foreach>
            </if>

            <if test="isMonitoring != null">
                AND sd.`is_monitoring` = #{isMonitoring}
            </if>
            <if test="isAttachment != null">
                <choose>
                    <when test="'true'.toString() == isAttachment.toString()">
                        AND sd.`p_device_id` IS NOT NULL
                    </when>
                    <when test="'false'.toString() == isAttachment.toString()">
                        AND sd.`p_device_id` IS NULL
                    </when>
                </choose>
            </if>
            <if test="hasAttachment != null">
                AND sd.`has_attachment` = #{hasAttachment}
            </if>
            <if test="name != null and name != ''">
                AND (sd.name like "%"#{name}"%" OR sd.short_name like "%"#{name}"%")
            </if>
            <if test="keyword != null and keyword != ''">
                AND (sd.name like "%"#{keyword}"%" OR sd.short_name like "%"#{keyword}"%" OR sd.cus_code like "%"#{keyword}"%")
            </if>
            <if test="gaCode != null and gaCode != ''">
                AND sd.ga_code = #{gaCode}
            </if>
            <if test="deviceId != null">
                AND sd.`device_id` = #{deviceId}
            </if>
            <if test="pDeviceId != null">
                AND sd.`p_device_id` = #{pDeviceId}
            </if>
            <if test="assetStatuses != null and assetStatuses.size() > 0">
                AND sd.`asset_status` IN
                <foreach collection="assetStatuses" open="(" close=")" separator="," item="assetStatus">
                    #{assetStatus}
                </foreach>
            </if>
            <if test="deviceIds != null and deviceIds.size() > 0">
                AND sd.`device_id` IN
                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">
                    #{deviceId}
                </foreach>
            </if>
        </where>
        ORDER BY
        <if test="locationOrderFlag != null">
            gl.`left`
            <choose>
                <when test="locationOrderFlag">
                    ASC,
                </when>
                <otherwise>
                    DESC,
                </otherwise>
            </choose>
        </if>
        <if test="nameOrderFlag != null">
            sd.`name`
            <choose>
                <when test="nameOrderFlag">
                    ASC,
                </when>
                <otherwise>
                    DESC,
                </otherwise>
            </choose>
        </if>
        <if test="shortNameOrderFlag != null">
            sd.`short_name`
            <choose>
                <when test="shortNameOrderFlag">
                    ASC,
                </when>
                <otherwise>
                    DESC,
                </otherwise>
            </choose>
        </if>
        <if test="typeOrderFlag != null">
            aat.`left`
            <choose>
                <when test="typeOrderFlag">
                    ASC,
                </when>
                <otherwise>
                    DESC,
                </otherwise>
            </choose>
        </if>
        <if test="assetStatusOrderFlag != null">
            sd.asset_status
            <choose>
                <when test="assetStatusOrderFlag">
                    ASC,
                </when>
                <otherwise>
                    DESC,
                </otherwise>
            </choose>
        </if>
        sd.device_id
        <choose>
            <when test="deviceIdOrderFlag == null or 'false'.toString() == deviceIdOrderFlag.toString()">
                DESC
            </when>
            <otherwise>
                ASC
            </otherwise>
        </choose>
    </select>


    <select id="remoteList" resultType="com.gds.jpi.dcim.vo.RemoteDeviceVO">
        SELECT
        sd.device_id ,
        sd.name,
        sd.status,
        sd.status,
        sd.short_name,
        sd.identify_code,
        sd.ga_code,
        sd.location_id,
        sd.length,
        sd.width,
        sd.height,
        sd.remark,
        sd.dc_id,
        dm.`dc_name_outer_short` dcName,
        aat.id typeId,
        aat.`name` typeName,
        sd.`has_attachment`,
        sd.`is_monitoring`,
        CASE
        WHEN sd.p_device_id IS NULL THEN
        false
        ELSE true
        END AS isAttachment,
        IFNULL(dtd.has_std, 0) has_std,
        am.id modelId,
        am.`model` modelName,
        amf.id mnftId,
        amf.`name` mnftName
        FROM
        sys_device sd
        LEFT JOIN asset.asset_type aat ON sd.asset_type_id = aat.id
        LEFT JOIN dcim.sys_device_type_detail dtd  ON dtd.asset_type_id = aat.id
        LEFT JOIN asset.model am ON sd.model_id = am.id
        LEFT JOIN asset.manufacture amf ON amf.id = sd.mnft_id
        LEFT JOIN geo.dc_mapping dm ON dm.id = sd.dc_id
        <where>
            <if test="status != null">
                sd.`status` = #{status}
            </if>
            <if test="deviceIds != null and deviceIds.size() > 0">
                AND sd.`device_id` IN
                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">
                    #{deviceId}
                </foreach>
            </if>
        </where>
    </select>

    <select id="listAllChildLocationsByIds" resultType="com.gds.jpi.dcim.model.GeoLocation">
        SELECT gl2.*
        FROM geo.location gl
        LEFT JOIN geo.location gl2 ON
        gl.`left` <![CDATA[ &gt;= ]]> gl2.`left` AND gl.`right`  <![CDATA[ &gt;= ]]>  gl2.`right`
        <where>
            gl2.`status` = 1
            <if test="locationIds != null and locationIds.size() > 0">
                AND gl.id IN
                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">
                    #{locationId}
                </foreach>
            </if>
        </where>
    </select>

    <select id="listAllChildAssetTypesByIds" resultType="com.gds.jpi.dcim.model.AssetType">
        SELECT
        aat2.*
        FROM asset.asset_type aat
        LEFT JOIN asset.asset_type aat2 ON
        aat.`left` <![CDATA[ <= ]]> aat2.`left` AND aat.`right` <![CDATA[ >= ]]> aat2.`right`
        <where>
            aat2.`is_delete` = 0
            <if test="typeIds != null and typeIds.size() > 0">
                AND aat.id IN
                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">
                    #{typeId}
                </foreach>
            </if>

        </where>
    </select>

    <select id="listParentLocationsByIds" resultType="com.gds.jpi.dcim.model.GeoLocation">
        SELECT
        GROUP_CONCAT(gl2.`name` ORDER BY gl2.depth SEPARATOR '/') name,
        gl.id
        FROM
        geo.location gl
        LEFT JOIN geo.location gl2 ON gl.`left` <![CDATA[ >= ]]> gl2.`left`
        AND gl.`right` <![CDATA[ <= ]]>  gl2.`right`
        <where>
            gl2.`status` = 1
            AND gl2.`depth` <![CDATA[ >= ]]> 5
            <if test="locationIds != null and locationIds.size() > 0">
                AND gl.id IN
                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">
                    #{locationId}
                </foreach>
            </if>
        </where>
        GROUP BY gl.id
    </select>

    <select id="listParentTypesByIds" resultType="com.gds.jpi.dcim.model.AssetType">
        SELECT
        GROUP_CONCAT(aat2.`name` ORDER BY aat2.depth SEPARATOR '/') name,
        aat.id
        FROM
        asset.asset_type aat
        LEFT JOIN asset.asset_type aat2 ON aat.`left` <![CDATA[ >= ]]> aat2.`left`
        AND aat.`right` <![CDATA[ <= ]]>aat2.`right`
        <where>
            aat2.`is_delete` = 0
            AND aat2.depth <![CDATA[ >= ]]> 2
            <if test="typeIds != null and typeIds.size() > 0">
                AND aat.id IN
                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">
                    #{typeId}
                </foreach>
            </if>
        </where>
        GROUP BY aat.id
    </select>

    <select id="hasAttrDataByDeviceIds" resultType="java.lang.Integer">
        SELECT 1
        FROM cbms.sys_device_attr sda
        <where>
            <if test="deviceIds != null and deviceIds.size() > 0">
                sda.device_id IN
                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">
                    #{deviceId}
                </foreach>
            </if>
        </where>
        LIMIT 1
    </select>

    <select id="checkHasAttrDataByDeviceIds" resultType="java.lang.Long">
        SELECT
            distinct device_id
        FROM
            cbms.sys_device_attr sda
        <where>
            <if test="deviceIds != null and deviceIds.size() > 0">
                sda.device_id IN
                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">
                    #{deviceId}
                </foreach>
            </if>
        </where>
    </select>

    <select id="listAssetsByTypeIds" resultType="com.gds.jpi.dcim.dto.AssetDTO"
            parameterType="com.gds.jpi.dcim.vo.AssetsImportReqVO">
        SELECT
        aa.identify_code,
        aa.identify_code ga_code,
        aa.name,
        aa.type_id assetTypeId,
        aa.location_id,
        aa.model_id,
        aa.mnft_id,
        aa.status AS asset_status,
        IFNULL(dtd.has_std, 0) isMonitoring,
        aaf.length,
        aaf.width,
        aaf.height,
        csdt.type_id deviceTypeId,
        csdt.tier1 typeTier1,
        csdt.tier2 typeTier2,
        csdt.tier3 typeTier3
        FROM asset.asset aa
        LEFT JOIN asset.asset_type aat ON aa.type_id = aat.id
        LEFT JOIN dcim.sys_device_type_detail dtd  ON dtd.asset_type_id = aat.id
        LEFT JOIN asset.asset_fm aaf ON aa.id = aaf.asset_id
        LEFT JOIN cbms.sys_device_type csdt ON csdt.asset_type_id = aa.type_id
        <where>
            aa.`status` IN (0,1)
            AND aa.`classification`= 1
            AND aaf.`is_delete` = 0
            <if test="typeIds != null and typeIds.size() > 0">
                AND aa.type_id IN
                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">
                    #{typeId}
                </foreach>
            </if>
            <if test="locationIds != null and locationIds.size() > 0">
                AND aa.location_id IN
                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">
                    #{locationId}
                </foreach>
            </if>
        </where>
    </select>

    <select id="listLocationIdsByDcId" resultType="java.lang.Integer">
        SELECT location_id
        FROM geo.dc_location gdl
        WHERE gdl.dc_id = #{dcId}
    </select>

    <select id="listIdentifyCode" resultType="java.lang.String">
        SELECT identify_code
        FROM sys_device sd
        WHERE sd.`status` = 1
        <if test="identifyCodes != null and identifyCodes.size() > 0">
            AND sd.identify_code IN
            <foreach collection="identifyCodes" open="(" close=")" separator="," item="identifyCode">
                #{identifyCode}
            </foreach>
        </if>
    </select>

    <select id="selectModelNameByModeId" resultType="java.lang.String">
        SELECT model
        FROM asset.model
        WHERE `status` = 0
          AND id = #{modelId} LIMIT 1
    </select>

    <select id="selectMnftNameByMuftId" resultType="java.lang.String">
        SELECT name
        FROM asset.manufacture
        WHERE `status` = 0
          AND id = #{mnftId} LIMIT 1
    </select>
    <select id="selectType" resultType="com.gds.jpi.dcim.dto.SysDeviceTypeDTO">
        SELECT type_id deviceTypeId,
               tier1   typeTier1,
               tier2   typeTier2,
               tier3   typeTier3
        FROM cbms.sys_device_type
        WHERE asset_type_id = #{assetTypeId} LIMIT 1
    </select>

    <select id="selectLocationIds" resultType="com.gds.jpi.dcim.model.SysDevice">
        SELECT device_id,location_id FROM cbms.sys_device WHERE  location_id IS NOT NULL
    </select>

    <select id="listAllLocation" resultType="com.gds.jpi.dcim.model.GeoLocation">
        SELECT id, name, `left`, `right`, depth
        FROM geo.location
        WHERE `depth` >= 5
    </select>
    <select id="searchDevicesForGenerateEventInst" resultType="com.gds.jpi.dcim.dto.event.SearchDeviceResponse">
        SELECT sd.*, gl.name as location_name
        FROM cbms.sys_device sd
                 left join geo.location gl on sd.location_id = gl.id
        WHERE sd.dc_id = #{dcId}
          AND sd.asset_type_id = #{assetTypeId}
          AND sd.status = 1
          AND sd.is_monitoring = 1
          <if test="locationId != null">
              AND sd.location_id = #{locationId}
          </if>
    </select>


    <select id="listModelByAssertTypeIds" resultType="com.gds.jpi.dcim.dto.device.DeviceModel">
        SELECT
        am.model modelName,
        am.id modelId,
        am.mnft_id manufactureId,
        am.type_id assetTypeId,
        aman.NAME manufactureName
        FROM
        `asset`.model am
        LEFT JOIN `asset`.manufacture aman ON am.mnft_id = aman.id
        WHERE
        am.STATUS = 0
        AND aman.STATUS = 0
        <if test="assetTypes != null and assetTypes.size() > 0">
            AND am.type_id IN
            <foreach collection="assetTypes" open="(" close=")" separator="," item="assetType">
                #{assetType.id}
            </foreach>
        </if>
    </select>

    <select id="searchSysDeviceDownBox" resultType="com.gds.jpi.dcim.vo.device.DeviceDownBoxRsp">
        SELECT
            d.device_id,
            d.`name`,
            d.short_name,
            d.asset_type_id,
            d.location_id,
            l.path AS location_name,
            d.is_monitoring,
            t.`name` AS asset_type_name
        FROM
            cbms.sys_device d
        LEFT JOIN
            asset.asset_type t ON t.id = d.asset_type_id AND t.is_delete = 0
        LEFT JOIN
            geo.location l ON l.id = d.location_id AND l.status = 1
        WHERE
            d.`status` = 1
        <if test="deviceId != null">
            AND d.device_id = #{deviceId}
        </if>
        <if test="text != null">
            AND d.`name` LIKE #{text}
        </if>
        <if test="dcId != null">
            AND d.dc_id = #{dcId}
        </if>
        <if test="assetTypeId != null">
            AND d.asset_type_id = #{assetTypeId}
        </if>
        <if test="locationIds != null and locationIds.size() > 0">
            AND d.location_id IN
            <foreach collection="locationIds" item="locationId" open="(" separator="," close=")">
                #{locationId}
            </foreach>
        </if>
    </select>


    <insert id="batchInsert" useGeneratedKeys="true" keyProperty="deviceId" parameterType="list">
        Insert INTO sys_device (name,short_name, asset_type_id, location_id, mnft_id, model_id,
        is_monitoring,dc_id,identify_code,ga_code,length,width,height,device_type_id,type_tier1,type_tier2,type_tier3,asset_status)
        VALUES
        <if test="sysDevices != null and sysDevices.size() > 0">
            <foreach collection="sysDevices" separator="," item="sysDevice">
                (#{sysDevice.name},#{sysDevice.shortName}, #{sysDevice.assetTypeId}, #{sysDevice.locationId},
                #{sysDevice.mnftId}, #{sysDevice.modelId},
                #{sysDevice.isMonitoring},#{sysDevice.dcId},
                #{sysDevice.identifyCode},#{sysDevice.gaCode},#{sysDevice.length},#{sysDevice.width},#{sysDevice.height},
                #{sysDevice.deviceTypeId},#{sysDevice.typeTier1},#{sysDevice.typeTier2},#{sysDevice.typeTier3},#{sysDevice.assetStatus}
                )
            </foreach>
        </if>
    </insert>
    <select id="searchDeviceWithPropValue" resultType="com.gds.jpi.dcim.vo.event.DeviceInfoRsp">
        SELECT
               sd.*,p.value AS propValue
        FROM cbms.sys_device sd
        LEFT JOIN cbms.sys_device_prop p ON sd.device_id = p.device_id
        LEFT JOIN dcim.std_temp_prop std ON p.temp_prop_id = std.temp_prop_id
        WHERE
              sd.dc_id = #{dcId}
        AND
              std.code = #{propCode}
        AND
              sd.asset_type_id  = #{assetTypeId}
        AND
              sd.status = 1
        AND
              sd.is_monitoring = 1
    </select>

    <select id="selectByDeviceIds" resultType="com.gds.jpi.dcim.vo.DeviceVO">
        SELECT sd.device_id,
               sd.asset_type_id typeId,
               sd.name,
               sd.name AS device_name,
               sd.location_id ,
               aat.`name` typeName
        FROM cbms.sys_device sd
                 LEFT JOIN asset.asset_type aat ON sd.asset_type_id = aat.id
        WHERE
            sd.`status` = 1 AND sd.dc_id=#{dcId}
        <if test="deviceIds != null and deviceIds.size() > 0">
            AND sd.`device_id` IN
            <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">
                #{deviceId}
            </foreach>
        </if>
    </select>

    <select id="findDeviceProp" resultType="com.gds.jpi.dcim.vo.device.DeviceStdVO">
        SELECT
        d.device_id,
        temp.code,
        `data`.sys_prop_id AS id,
        temp.unit,
        IFNULL(temp.decimal_digit, 0) as decimal_digit,
        ROUND(`data`.`value`, IFNULL(temp.decimal_digit, 0)) AS `value`,
        dict.type_code AS enum_type_code,
        dict.code AS enum_code,
        dict.name AS enum_name
        FROM cbms.sys_device d
        JOIN cbms.sys_device_prop `data` ON `data`.device_id = d.device_id
        JOIN dcim.std_temp_prop temp ON temp.temp_prop_id = `data`.temp_prop_id
        LEFT JOIN dcim.std_enum_dict dict ON dict.type_code = temp.enum_type_code AND dict.value = `data`.value
        <include refid="attrsConditions"/>
    </select>

    <select id="findDeviceAttr" resultType="com.gds.jpi.dcim.vo.device.DeviceStdVO">
        SELECT
        d.device_id,
        temp.code,
        attr.sys_attr_id AS id,
        temp.unit,
        temp.enum_type_code AS enum_type_code,
        IFNULL(temp.decimal_digit, 0) as decimal_digit,
        attr.tag_id as redis_key
        FROM cbms.sys_device d
        JOIN cbms.sys_device_attr attr ON attr.device_id = d.device_id
        JOIN dcim.std_temp_attr temp ON temp.temp_attr_id = attr.temp_attr_id
        <include refid="attrsConditions"/>
    </select>

    <select id="findDeviceIndi" resultType="com.gds.jpi.dcim.vo.device.DeviceStdVO">
        SELECT
        d.device_id,
        temp.code,
        i.id,
        temp.unit,
        temp.enum_type_code AS enum_type_code,
        IFNULL(temp.decimal_digit, 0) as decimal_digit,
        i.id as redis_key
        FROM cbms.sys_device d
        JOIN cbms.sys_device_indi i ON i.device_id = d.device_id
        JOIN dcim.std_temp_indi temp ON temp.id = i.temp_indi_id
        <include refid="attrsConditions"/>
    </select>
    <select id="listOpenApiDevices" resultType="com.gds.jpi.dcim.response.OpenApiDeviceResponse">
        SELECT
            d.device_id AS id,
            d.name,
            d.short_name,
            d.location_id,
            l.name AS location_name,
            t.code AS type_code,
            t.name AS type_name
        FROM
            cbms.sys_device d
        JOIN
            asset.asset_type t ON t.id = d.asset_type_id
        LEFT JOIN
            geo.location l ON l.id = d.location_id AND l.status = 1
        WHERE
            d.status = 1
            AND d.dc_id = #{dcId}
            AND t.code IN
            <foreach collection="typeCodes" item="code" open="(" separator="," close=")">
                #{code}
            </foreach>
    </select>
    <select id="selectDeviceLocation" resultType="com.gds.jpi.dcim.vo.DeviceVO">
        SELECT
            d.device_id,
            IFNULL(l.path, d.name) AS location_name
        FROM
            cbms.sys_device d
        LEFT JOIN
            geo.location l ON d.location_id = l.id AND l.status = 1
        WHERE
            d.device_id IN
        <foreach collection="deviceIds" item="deviceId" open="(" separator="," close=")">
            #{deviceId}
        </foreach>
            AND d.status = 1
    </select>

    <select id="queryLocationDevices" resultType="com.gds.jpi.dcim.vo.layout.AssetDeviceResponse">
        <!-- 根据 location_id 查询对应的资产类型 -->
        <!-- 入参 -->
        <!-- location_id -->
        <!-- 结果 -->
        <!-- device_id -->
        <!-- asset_type_id -->
        <!-- device_name -->
        <!-- device_short_name -->
        <!-- loc_storey -->
        <!-- loc_room -->
        <!-- location_id -->
        <!-- loc_data_center -->
        <!-- width -->
        <!-- length -->
        SELECT
        sd.device_id,
        sd.asset_type_id,
        t.`code` AS asset_type_code,
        sd.name AS `device_name`,
        sd.short_name AS `device_short_name`,
        sd.loc_storey,
        sd.loc_room,
        sd.location_id,
        sd.loc_data_center,
        sd.width,
        sd.length
        FROM cbms.sys_device sd
        LEFT JOIN asset.asset_type t ON t.id = sd.asset_type_id AND t.is_delete = 0
        WHERE sd.location_id IN (
        SELECT
        dl.id
        FROM
        geo.location dl,
        geo.location pl
        WHERE
        pl.id = #{location_id}
        AND pl.`left` &lt;= dl.`left`
        AND pl.`right` &gt;= dl.`right`
        )
        AND sd.asset_type_id IN
        <foreach collection="asset_type_ids" item="asset_type_id" open="(" separator="," close=")">
            #{asset_type_id}
        </foreach>
        AND sd.status = 1
        AND sd.p_device_id IS NULL
        ORDER BY sd.name ASC
    </select>

    <select id="getAssetTypesByLocation" resultType="com.gds.jpi.dcim.vo.layout.AssetTypeResponse">
        <!-- Add a comment to describe the purpose of the query -->
        <!-- Retrieve asset type information based on location ID and asset type IDs -->
        <!-- Input parameters: locationId (Integer), assetTypeIds (List<Integer>) -->
        <!-- Result fields: assetTypeId, assetTypeName, label, parentId, locationId -->

        SELECT DISTINCT d.asset_type_id
        FROM cbms.sys_device d
        JOIN geo.location l ON d.location_id = l.id
        JOIN geo.location l2 ON l2.id = #{location_id}
        WHERE l.`left` &gt;= l2.`left` AND l.`right` &lt;= l2.`right`;

    </select>

</mapper>

```

#### Output:

```json
{
"translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace="com.gds.jpi.dcim.core.mapper.SysDeviceMapper">\n\n    <!--  Attribute conditions  -->\n    <sql id="attrsConditions">\n        WHERE d.status = 1\n        AND d.is_monitoring = 1\n        AND temp.is_del = 0\n        <if test="deviceIdList != null and deviceIdList.size() > 0">\n            AND d.device_id IN\n            <foreach collection="deviceIdList" separator="," open="(" close=")" item="item">\n                #{item}\n            </foreach>\n        </if>\n        <if test="codeList != null and codeList.size() > 0">\n            AND temp.code IN\n            <foreach collection="codeList" separator="," open="(" close=")" item="item">\n                #{item}\n            </foreach>\n        </if>\n    </sql>\n\n    <select id="searchSysDevices" resultType="com.gds.jpi.dcim.model.SysDevice">\n        SELECT device.\n        FROM cbms.sys_device device\n                 LEFT JOIN geo.dc_mapping dc ON dc.id = device.dc_id\n        WHERE device.status = 1\n          AND dc.dc_name_outer_full like CONCAT('%', CONCAT(#{dcName}, '%'))\n\n    </select>\n    <select id="searchLocationByDcs" resultType="com.gds.jpi.dcim.model.GeoLocation">\n        SELECT a.\n        FROM geo.location a\n        LEFT JOIN geo.dc_location b on a.id = b.location_id\n        LEFT JOIN geo.dc_mapping c on b.dc_id = c.id\n        WHERE a.status = 1\n        AND c.id in\n        <foreach collection="dcIds" index="index" item="item" open="(" separator="," close=")">\n            #{item}\n        </foreach>\n    </select>\n    <select id="searchAllChildLocations" resultType="com.gds.jpi.dcim.model.GeoLocation">\n        select a.\n        from geo.location a\n        where a.status = 1 and\n        <foreach collection="parentLocations" item="location" open=" (" close=")" separator="or">\n            ( a.left &gt;=  #{location.left} and a.right &lt;= #{location.right} )\n        </foreach>\n        order by a.left\n    </select>\n\n    <select id="list" resultType="com.gds.jpi.dcim.vo.DeviceVO" parameterType="com.gds.jpi.dcim.vo.DeviceReqVO">\n        SELECT\n        sd.device_id ,\n        sd.name,\n        sd.name AS device_name,\n        sd.short_name,\n        sd.identify_code,\n        sd.ga_code,\n        sd.cus_code,\n        sd.location_id,\n        gl.path AS location_name,\n        sd.length,\n        sd.width,\n        sd.height,\n        sd.remark,\n        sd.dc_id,\n        dm.dc_name_outer_short dcName,\n        aat.id typeId,\n        aat.code typeCode,\n        aat.name typeName,\n        sd.has_attachment,\n        sd.is_monitoring,\n        sd.p_device_id,\n        CASE\n        WHEN sd.p_device_id IS NULL THEN\n        false\n        ELSE true\n        END AS isAttachment,\n        IFNULL(dtd.has_std, 0) has_std,\n        am.id modelId,\n        am.model modelName,\n        amf.id mnftId,\n        amf.name mnftName,\n        sd.asset_status\n        FROM\n        cbms.sys_device sd\n        LEFT JOIN asset.asset_type aat ON sd.asset_type_id = aat.id\n        LEFT JOIN dcim.sys_device_type_detail dtd  ON dtd.asset_type_id = aat.id\n        LEFT JOIN asset.model am ON sd.model_id = am.id\n        LEFT JOIN asset.manufacture amf ON amf.id = sd.mnft_id\n        LEFT JOIN geo.dc_mapping dm ON dm.id = sd.dc_id\n        LEFT JOIN geo.location gl ON sd.location_id = gl.id\n        <where>\n            sd.status = true\n            <if test="dcId != null">\n                AND sd.dc_id = #{dcId}\n            </if>\n            <if test="cusCodes != null and cusCodes.size() > 0">\n                AND sd.cus_code IN\n                <foreach collection="cusCodes" item="cusCode" open="(" separator="," close=")">\n                    #{cusCode}\n                </foreach>\n            </if>\n            <if test="authDcs != null and authDcs.size() > 0">\n                AND sd.dc_id IN\n                <foreach collection="authDcs" item="authDc" open="(" separator="," close=")">\n                    #{authDc}\n                </foreach>\n            </if>\n            <if test="dcIds != null and dcIds.size() > 0">\n                AND sd.dc_id IN\n                <foreach collection="dcIds" item="dc" open="(" separator="," close=")">\n                    #{dc}\n                </foreach>\n            </if>\n            <if test="locationIds != null and locationIds.size() > 0">\n                AND sd.location_id IN\n                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">\n                    #{locationId}\n                </foreach>\n            </if>\n            <if test="typeIds != null and typeIds.size() > 0">\n                AND sd.asset_type_id IN\n                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">\n                    #{typeId}\n                </foreach>\n            </if>\n            <if test="typeCodes != null and typeCodes.size() > 0">\n                AND aat.code IN\n                <foreach collection="typeCodes" open="(" close=")" separator="," item="typeCode">\n                    #{typeCode}\n                </foreach>\n            </if>\n            <if test="mnftIds != null and mnftIds.size() > 0">\n                AND sd.mnft_id IN\n                <foreach collection="mnftIds" open="(" close=")" separator="," item="mnftId">\n                    #{mnftId}\n                </foreach>\n            </if>\n\n            <if test="isMonitoring != null">\n                AND sd.is_monitoring = #{isMonitoring}\n            </if>\n            <if test="isAttachment != null">\n                <choose>\n                    <when test="'true'.toString() == isAttachment.toString()">\n                        AND sd.p_device_id IS NOT NULL\n                    </when>\n                    <when test="'false'.toString() == isAttachment.toString()">\n                        AND sd.p_device_id IS NULL\n                    </when>\n                </choose>\n            </if>\n            <if test="hasAttachment != null">\n                AND sd.has_attachment = #{hasAttachment}\n            </if>\n            <if test="name != null and name != ''">\n                AND (sd.name like "%"#{name}"%" OR sd.short_name like "%"#{name}"%")\n            </if>\n            <if test="keyword != null and keyword != ''">\n                AND (sd.name like "%"#{keyword}"%" OR sd.short_name like "%"#{keyword}"%" OR sd.cus_code like "%"#{keyword}"%")\n            </if>\n            <if test="gaCode != null and gaCode != ''">\n                AND sd.ga_code = #{gaCode}\n            </if>\n            <if test="deviceId != null">\n                AND sd.device_id = #{deviceId}\n            </if>\n            <if test="pDeviceId != null">\n                AND sd.p_device_id = #{pDeviceId}\n            </if>\n            <if test="assetStatuses != null and assetStatuses.size() > 0">\n                AND sd.asset_status IN\n                <foreach collection="assetStatuses" open="(" close=")" separator="," item="assetStatus">\n                    #{assetStatus}\n                </foreach>\n            </if>\n            <if test="deviceIds != null and deviceIds.size() > 0">\n                AND sd.device_id IN\n                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">\n                    #{deviceId}\n                </foreach>\n            </if>\n        </where>\n        ORDER BY\n        <if test="locationOrderFlag != null">\n            gl.left\n            <choose>\n                <when test="locationOrderFlag">\n                    ASC,\n                </when>\n                <otherwise>\n                    DESC,\n                </otherwise>\n            </choose>\n        </if>\n        <if test="nameOrderFlag != null">\n            sd.name\n            <choose>\n                <when test="nameOrderFlag">\n                    ASC,\n                </when>\n                <otherwise>\n                    DESC,\n                </otherwise>\n            </choose>\n        </if>\n        <if test="shortNameOrderFlag != null">\n            sd.short_name\n            <choose>\n                <when test="shortNameOrderFlag">\n                    ASC,\n                </when>\n                <otherwise>\n                    DESC,\n                </otherwise>\n            </choose>\n        </if>\n        <if test="typeOrderFlag != null">\n            aat.left\n            <choose>\n                <when test="typeOrderFlag">\n                    ASC,\n                </when>\n                <otherwise>\n                    DESC,\n                </otherwise>\n            </choose>\n        </if>\n        <if test="assetStatusOrderFlag != null">\n            sd.asset_status\n            <choose>\n                <when test="assetStatusOrderFlag">\n                    ASC,\n                </when>\n                <otherwise>\n                    DESC,\n                </otherwise>\n            </choose>\n        </if>\n        sd.device_id\n        <choose>\n            <when test="deviceIdOrderFlag == null or 'false'.toString() == deviceIdOrderFlag.toString()">\n                DESC\n            </when>\n            <otherwise>\n                ASC\n            </otherwise>\n        </choose>\n    </select>\n\n\n    <select id="remoteList" resultType="com.gds.jpi.dcim.vo.RemoteDeviceVO">\n        SELECT\n        sd.device_id ,\n        sd.name,\n        sd.status,\n        sd.status,\n        sd.short_name,\n        sd.identify_code,\n        sd.ga_code,\n        sd.location_id,\n        sd.length,\n        sd.width,\n        sd.height,\n        sd.remark,\n        sd.dc_id,\n        dm.dc_name_outer_short dcName,\n        aat.id typeId,\n        aat.name typeName,\n        sd.has_attachment,\n        sd.is_monitoring,\n        CASE\n        WHEN sd.p_device_id IS NULL THEN\n        false\n        ELSE true\n        END AS isAttachment,\n        IFNULL(dtd.has_std, 0) has_std,\n        am.id modelId,\n        am.model modelName,\n        amf.id mnftId,\n        amf.name mnftName\n        FROM\n        sys_device sd\n        LEFT JOIN asset.asset_type aat ON sd.asset_type_id = aat.id\n        LEFT JOIN dcim.sys_device_type_detail dtd  ON dtd.asset_type_id = aat.id\n        LEFT JOIN asset.model am ON sd.model_id = am.id\n        LEFT JOIN asset.manufacture amf ON amf.id = sd.mnft_id\n        LEFT JOIN geo.dc_mapping dm ON dm.id = sd.dc_id\n        <where>\n            <if test="status != null">\n                sd.status = #{status}\n            </if>\n            <if test="deviceIds != null and deviceIds.size() > 0">\n                AND sd.device_id IN\n                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">\n                    #{deviceId}\n                </foreach>\n            </if>\n        </where>\n    </select>\n\n    <select id="listAllChildLocationsByIds" resultType="com.gds.jpi.dcim.model.GeoLocation">\n        SELECT gl2.\n        FROM geo.location gl\n        LEFT JOIN geo.location gl2 ON\n        gl.left &lt;= gl2.left AND gl.right  &gt;=  gl2.right\n        <where>\n            gl2.status = 1\n            <if test="locationIds != null and locationIds.size() > 0">\n                AND gl.id IN\n                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">\n                    #{locationId}\n                </foreach>\n            </if>\n        </where>\n    </select>\n\n    <select id="listAllChildAssetTypesByIds" resultType="com.gds.jpi.dcim.model.AssetType">\n        SELECT\n        aat2.\n        FROM asset.asset_type aat\n        LEFT JOIN asset.asset_type aat2 ON\n        aat.left &lt;= aat2.left AND aat.right &gt;= aat2.right\n        <where>\n            aat2.is_delete = 0\n            <if test="typeIds != null and typeIds.size() > 0">\n                AND aat.id IN\n                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">\n                    #{typeId}\n                </foreach>\n            </if>\n\n        </where>\n    </select>\n\n    <select id="listParentLocationsByIds" resultType="com.gds.jpi.dcim.model.GeoLocation">\n        SELECT\n        GROUP_CONCAT(gl2.name ORDER BY gl2.depth SEPARATOR '/') name,\n        gl.id\n        FROM\n        geo.location gl\n        LEFT JOIN geo.location gl2 ON gl.left &gt;= gl2.left\n        AND gl.right &lt;=  gl2.right\n        <where>\n            gl2.status = 1\n            AND gl2.depth &gt;= 5\n            <if test="locationIds != null and locationIds.size() > 0">\n                AND gl.id IN\n                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">\n                    #{locationId}\n                </foreach>\n            </if>\n        </where>\n        GROUP BY gl.id\n    </select>\n\n    <select id="listParentTypesByIds" resultType="com.gds.jpi.dcim.model.AssetType">\n        SELECT\n        GROUP_CONCAT(aat2.name ORDER BY aat2.depth SEPARATOR '/') name,\n        aat.id\n        FROM\n        asset.asset_type aat\n        LEFT JOIN asset.asset_type aat2 ON aat.left &gt;= aat2.left\n        AND aat.right &lt;= aat2.right\n        <where>\n            aat2.is_delete = 0\n            AND aat2.depth &gt;= 2\n            <if test="typeIds != null and typeIds.size() > 0">\n                AND aat.id IN\n                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">\n                    #{typeId}\n                </foreach>\n            </if>\n        </where>\n        GROUP BY aat.id\n    </select>\n\n    <select id="hasAttrDataByDeviceIds" resultType="java.lang.Integer">\n        SELECT 1\n        FROM cbms.sys_device_attr sda\n        <where>\n            <if test="deviceIds != null and deviceIds.size() > 0">\n                sda.device_id IN\n                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">\n                    #{deviceId}\n                </foreach>\n            </if>\n        </where>\n        LIMIT 1\n    </select>\n\n    <select id="checkHasAttrDataByDeviceIds" resultType="java.lang.Long">\n        SELECT\n            distinct device_id\n        FROM\n            cbms.sys_device_attr sda\n        <where>\n            <if test="deviceIds != null and deviceIds.size() > 0">\n                sda.device_id IN\n                <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">\n                    #{deviceId}\n                </foreach>\n            </if>\n        </where>\n    </select>\n\n    <select id="listAssetsByTypeIds" resultType="com.gds.jpi.dcim.dto.AssetDTO"\n            parameterType="com.gds.jpi.dcim.vo.AssetsImportReqVO">\n        SELECT\n        aa.identify_code,\n        aa.identify_code ga_code,\n        aa.name,\n        aa.type_id assetTypeId,\n        aa.location_id,\n        aa.model_id,\n        aa.mnft_id,\n        aa.status AS asset_status,\n        IFNULL(dtd.has_std, 0) isMonitoring,\n        aaf.length,\n        aaf.width,\n        aaf.height,\n        csdt.type_id deviceTypeId,\n        csdt.tier1 typeTier1,\n        csdt.tier2 typeTier2,\n        csdt.tier3 typeTier3\n        FROM asset.asset aa\n        LEFT JOIN asset.asset_type aat ON aa.type_id = aat.id\n        LEFT JOIN dcim.sys_device_type_detail dtd  ON dtd.asset_type_id = aat.id\n        LEFT JOIN asset.asset_fm aaf ON aa.id = aaf.asset_id\n        LEFT JOIN cbms.sys_device_type csdt ON csdt.asset_type_id = aa.type_id\n        <where>\n            aa.status IN (0,1)\n            AND aa.classification= 1\n            AND aaf.is_delete = 0\n            <if test="typeIds != null and typeIds.size() > 0">\n                AND aa.type_id IN\n                <foreach collection="typeIds" open="(" close=")" separator="," item="typeId">\n                    #{typeId}\n                </foreach>\n            </if>\n            <if test="locationIds != null and locationIds.size() > 0">\n                AND aa.location_id IN\n                <foreach collection="locationIds" open="(" close=")" separator="," item="locationId">\n                    #{locationId}\n                </foreach>\n            </if>\n        </where>\n    </select>\n\n    <select id="listLocationIdsByDcId" resultType="java.lang.Integer">\n        SELECT location_id\n        FROM geo.dc_location gdl\n        WHERE gdl.dc_id = #{dcId}\n    </select>\n\n    <select id="listIdentifyCode" resultType="java.lang.String">\n        SELECT identify_code\n        FROM sys_device sd\n        WHERE sd.status = 1\n        <if test="identifyCodes != null and identifyCodes.size() > 0">\n            AND sd.identify_code IN\n            <foreach collection="identifyCodes" open="(" close=")" separator="," item="identifyCode">\n                #{identifyCode}\n            </foreach>\n        </if>\n    </select>\n\n    <select id="selectModelNameByModeId" resultType="java.lang.String">\n        SELECT model\n        FROM asset.model\n        WHERE status = 0\n          AND id = #{modelId} LIMIT 1\n    </select>\n\n    <select id="selectMnftNameByMuftId" resultType="java.lang.String">\n        SELECT name\n        FROM asset.manufacture\n        WHERE status = 0\n          AND id = #{mnftId} LIMIT 1\n    </select>\n    <select id="selectType" resultType="com.gds.jpi.dcim.dto.SysDeviceTypeDTO">\n        SELECT type_id deviceTypeId,\n               tier1   typeTier1,\n               tier2   typeTier2,\n               tier3   typeTier3\n        FROM cbms.sys_device_type\n        WHERE asset_type_id = #{assetTypeId} LIMIT 1\n    </select>\n\n    <select id="selectLocationIds" resultType="com.gds.jpi.dcim.model.SysDevice">\n        SELECT device_id,location_id FROM cbms.sys_device WHERE  location_id IS NOT NULL\n    </select>\n\n    <select id="listAllLocation" resultType="com.gds.jpi.dcim.model.GeoLocation">\n        SELECT id, name, left, right, depth\n        FROM geo.location\n        WHERE depth &gt;= 5\n    </select>\n    <select id="searchDevicesForGenerateEventInst" resultType="com.gds.jpi.dcim.dto.event.SearchDeviceResponse">\n        SELECT sd., gl.name as location_name\n        FROM cbms.sys_device sd\n                 left join geo.location gl on sd.location_id = gl.id\n        WHERE sd.dc_id = #{dcId}\n          AND sd.asset_type_id = #{assetTypeId}\n          AND sd.status = 1\n          AND sd.is_monitoring = 1\n          <if test="locationId != null">\n              AND sd.location_id = #{locationId}\n          </if>\n    </select>\n\n\n    <select id="listModelByAssertTypeIds" resultType="com.gds.jpi.dcim.dto.device.DeviceModel">\n        SELECT\n        am.model modelName,\n        am.id modelId,\n        am.mnft_id manufactureId,\n        am.type_id assetTypeId,\n        aman.NAME manufactureName\n        FROM\n        asset.model am\n        LEFT JOIN asset.manufacture aman ON am.mnft_id = aman.id\n        WHERE\n        am.STATUS = 0\n        AND aman.STATUS = 0\n        <if test="assetTypes != null and assetTypes.size() > 0">\n            AND am.type_id IN\n            <foreach collection="assetTypes" open="(" close=")" separator="," item="assetType">\n                #{assetType.id}\n            </foreach>\n        </if>\n    </select>\n\n    <select id="searchSysDeviceDownBox" resultType="com.gds.jpi.dcim.vo.device.DeviceDownBoxRsp">\n        SELECT\n            d.device_id,\n            d.name,\n            d.short_name,\n            d.asset_type_id,\n            d.location_id,\n            l.path AS location_name,\n            d.is_monitoring,\n            t.name AS asset_type_name\n        FROM\n            cbms.sys_device d\n        LEFT JOIN\n            asset.asset_type t ON t.id = d.asset_type_id AND t.is_delete = 0\n        LEFT JOIN\n            geo.location l ON l.id = d.location_id AND l.status = 1\n        WHERE\n            d.status = 1\n        <if test="deviceId != null">\n            AND d.device_id = #{deviceId}\n        </if>\n        <if test="text != null">\n            AND d.name LIKE #{text}\n        </if>\n        <if test="dcId != null">\n            AND d.dc_id = #{dcId}\n        </if>\n        <if test="assetTypeId != null">\n            AND d.asset_type_id = #{assetTypeId}\n        </if>\n        <if test="locationIds != null and locationIds.size() > 0">\n            AND d.location_id IN\n            <foreach collection="locationIds" item="locationId" open="(" separator="," close=")">\n                #{locationId}\n            </foreach>\n        </if>\n    </select>\n\n\n    <insert id="batchInsert" useGeneratedKeys="true" keyProperty="deviceId" parameterType="list">\n        Insert INTO sys_device (name,short_name, asset_type_id, location_id, mnft_id, model_id,\n        is_monitoring,dc_id,identify_code,ga_code,length,width,height,device_type_id,type_tier1,type_tier2,type_tier3,asset_status)\n        VALUES\n        <if test="sysDevices != null and sysDevices.size() > 0">\n            <foreach collection="sysDevices" separator="," item="sysDevice">\n                (#{sysDevice.name},#{sysDevice.shortName}, #{sysDevice.assetTypeId}, #{sysDevice.locationId},\n                #{sysDevice.mnftId}, #{sysDevice.modelId},\n                #{sysDevice.isMonitoring},#{sysDevice.dcId},\n                #{sysDevice.identifyCode},#{sysDevice.gaCode},#{sysDevice.length},#{sysDevice.width},#{sysDevice.height},\n                #{sysDevice.deviceTypeId},#{sysDevice.typeTier1},#{sysDevice.typeTier2},#{sysDevice.typeTier3},#{sysDevice.assetStatus} \n                )\n            </foreach>\n        </if>\n    </insert>\n    <select id="searchDeviceWithPropValue" resultType="com.gds.jpi.dcim.vo.event.DeviceInfoRsp">\n        SELECT\n               sd.*,p.value AS propValue\n        FROM cbms.sys_device sd\n        LEFT JOIN cbms.sys_device_prop p ON sd.device_id = p.device_id\n        LEFT JOIN dcim.std_temp_prop std ON p.temp_prop_id = std.temp_prop_id\n        WHERE\n              sd.dc_id = #{dcId}\n        AND\n              std.code = #{propCode}\n        AND\n              sd.asset_type_id  = #{assetTypeId}\n        AND\n              sd.status = 1\n        AND\n              sd.is_monitoring = 1\n    </select>\n\n    <select id="selectByDeviceIds" resultType="com.gds.jpi.dcim.vo.DeviceVO">\n        SELECT sd.device_id,\n               sd.asset_type_id typeId,\n               sd.name,\n               sd.name AS device_name,\n               sd.location_id ,\n               aat.name typeName\n        FROM cbms.sys_device sd\n                 LEFT JOIN asset.asset_type aat ON sd.asset_type_id = aat.id\n        WHERE\n            sd.status = 1 AND sd.dc_id=#{dcId}\n        <if test="deviceIds != null and deviceIds.size() > 0">\n            AND sd.device_id IN\n            <foreach collection="deviceIds" open="(" close=")" separator="," item="deviceId">\n                #{deviceId}\n            </foreach>\n        </if>\n    </select>\n\n    <select id="findDeviceProp" resultType="com.gds.jpi.dcim.vo.device.DeviceStdVO">\n        SELECT\n        d.device_id,\n        temp.code,\n        data.sys_prop_id AS id,\n        temp.unit,\n        IFNULL(temp.decimal_digit, 0) as decimal_digit,\n        ROUND(data.value, IFNULL(temp.decimal_digit, 0)) AS value,\n        dict.type_code AS enum_type_code,\n        dict.code AS enum_code,\n        dict.name AS enum_name\n        FROM cbms.sys_device d\n        JOIN cbms.sys_device_prop data ON data.device_id = d.device_id\n        JOIN dcim.std_temp_prop temp ON temp.temp_prop_id = data.temp_prop_id\n        LEFT JOIN dcim.std_enum_dict dict ON dict.type_code = temp.enum_type_code AND dict.value = data.value\n        <include refid="attrsConditions"/>\n    </select>\n\n    <select id="findDeviceAttr" resultType="com.gds.jpi.dcim.vo.device.DeviceStdVO">\n        SELECT\n        d.device_id,\n        temp.code,\n        attr.sys_attr_id AS id,\n        temp.unit,\n        temp.enum_type_code AS enum_type_code,\n        IFNULL(temp.decimal_digit, 0) as decimal_digit,\n        attr.tag_id as redis_key\n        FROM cbms.sys_device d\n        JOIN cbms.sys_device_attr attr ON attr.device_id = d.device_id\n        JOIN dcim.std_temp_attr temp ON temp.temp_attr_id = attr.temp_attr_id\n        <include refid="attrsConditions"/>\n    </select>\n\n    <select id="findDeviceIndi" resultType="com.gds.jpi.dcim.vo.device.DeviceStdVO">\n        SELECT\n        d.device_id,\n        temp.code,\n        i.id,\n        temp.unit,\n        temp.enum_type_code AS enum_type_code,\n        IFNULL(temp.decimal_digit, 0) as decimal_digit,\n        i.id as redis_key\n        FROM cbms.sys_device d\n        JOIN cbms.sys_device_indi i ON i.device_id = d.device_id\n        JOIN dcim.std_temp_indi temp ON temp.id = i.temp_indi_id\n        <include refid="attrsConditions"/>\n    </select>\n    <select id="listOpenApiDevices" resultType="com.gds.jpi.dcim.response.OpenApiDeviceResponse">\n        SELECT\n            d.device_id AS id,\n            d.name,\n            d.short_name,\n            d.location_id,\n            l.name AS location_name,\n            t.code AS type_code,\n            t.name AS type_name\n        FROM\n            cbms.sys_device d\n        JOIN\n            asset.asset_type t ON t.id = d.asset_type_id\n        LEFT JOIN\n            geo.location l ON l.id = d.location_id AND l.status = 1\n        WHERE\n            d.status = 1\n            AND d.dc_id = #{dcId}\n            AND t.code IN\n            <foreach collection="typeCodes" item="code" open="(" separator="," close=")">\n                #{code}\n            </foreach>\n    </select>\n    <select id="selectDeviceLocation" resultType="com.gds.jpi.dcim.vo.DeviceVO">\n        SELECT\n            d.device_id,\n            IFNULL(l.path, d.name) AS location_name\n        FROM\n            cbms.sys_device d\n        LEFT JOIN\n            geo.location l ON d.location_id = l.id AND l.status = 1\n        WHERE\n            d.device_id IN\n        <foreach collection="deviceIds" item="deviceId" open="(" separator="," close=")">\n            #{deviceId}\n        </foreach>\n            AND d.status = 1\n    </select>\n\n    <select id="queryLocationDevices" resultType="com.gds.jpi.dcim.vo.layout.AssetDeviceResponse">\n        <!--  Retrieve device information based on location_id  -->\n        <!--  Input parameters: -->\n        <!--  location_id  -->\n        <!--  Result fields: -->\n        <!-- device_id -->\n        <!-- asset_type_id -->\n        <!-- device_name -->\n        <!-- device_short_name -->\n        <!-- loc_storey -->\n        <!-- loc_room -->\n        <!-- location_id -->\n        <!-- loc_data_center -->\n        <!-- width -->\n        <!-- length -->\n        SELECT\n        sd.device_id,\n        sd.asset_type_id,\n        t.code AS asset_type_code,\n        sd.name AS device_name,\n        sd.short_name AS device_short_name,\n        sd.loc_storey,\n        sd.loc_room,\n        sd.location_id,\n        sd.loc_data_center,\n        sd.width,\n        sd.length\n        FROM cbms.sys_device sd\n        LEFT JOIN asset.asset_type t ON t.id = sd.asset_type_id AND t.is_delete = 0\n        WHERE sd.location_id IN (\n        SELECT\n        dl.id\n        FROM\n        geo.location dl,\n        geo.location pl\n        WHERE\n        pl.id = #{location_id}\n        AND pl.left &lt;= dl.left\n        AND pl.right &gt;= dl.right\n        )\n        AND sd.asset_type_id IN\n        <foreach collection="asset_type_ids" item="asset_type_id" open="(" separator="," close=")">\n            #{asset_type_id}\n        </foreach>\n        AND sd.status = 1\n        AND sd.p_device_id IS NULL\n        ORDER BY sd.name ASC\n    </select>\n\n    <select id="getAssetTypesByLocation" resultType="com.gds.jpi.dcim.vo.layout.AssetTypeResponse">\n        <!-- Add a comment to describe the purpose of the query -->\n        <!-- Retrieve asset type information based on location ID and asset type IDs -->\n        <!-- Input parameters: locationId (Integer), assetTypeIds (List<Integer>) -->\n        <!-- Result fields: assetTypeId, assetTypeName, label, parentId, locationId -->\n\n        SELECT DISTINCT d.asset_type_id\n        FROM cbms.sys_device d\n        JOIN geo.location l ON d.location_id = l.id\n        JOIN geo.location l2 ON l2.id = #{location_id}\n        WHERE l.left &gt;= l2.left AND l.right &lt;= l2.right;\n\n    </select>\n\n</mapper>",
  "details": [
    {
      "lineNumber": 5,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!--  Attribute conditions  -->",
      "translatedText": "<!--  Attribute conditions  -->"
    },
    {
      "lineNumber": 167,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!--  Retrieve device information based on location_id  -->",
      "translatedText": "<!--  Retrieve device information based on location_id  -->"
    },
    {
      "lineNumber": 168,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!--  Input parameters: -->",
      "translatedText": "<!--  Input parameters: -->"
    },
    {
      "lineNumber": 169,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!--  location_id  -->",
      "translatedText": "<!--  location_id  -->"
    },
    {
      "lineNumber": 170,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!--  Result fields: -->",
      "translatedText": "<!--  Result fields: -->"
    },
    {
      "lineNumber": 171,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- device_id -->",
      "translatedText": "<!-- device_id -->"
    },
    {
      "lineNumber": 172,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- asset_type_id -->",
      "translatedText": "<!-- asset_type_id -->"
    },
    {
      "lineNumber": 173,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- device_name -->",
      "translatedText": "<!-- device_name -->"
    },
    {
      "lineNumber": 174,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- device_short_name -->",
      "translatedText": "<!-- device_short_name -->"
    },
    {
      "lineNumber": 175,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- loc_storey -->",
      "translatedText": "<!-- loc_storey -->"
    },
    {
      "lineNumber": 176,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- loc_room -->",
      "translatedText": "<!-- loc_room -->"
    },
    {
      "lineNumber": 177,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- location_id -->",
      "translatedText": "<!-- location_id -->"
    },
    {
      "lineNumber": 178,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- loc_data_center -->",
      "translatedText": "<!-- loc_data_center -->"
    },
    {
      "lineNumber": 179,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- width -->",
      "translatedText": "<!-- width -->"
    },
    {
      "lineNumber": 180,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- length -->",
      "translatedText": "<!-- length -->"
    },
    {
      "lineNumber": 217,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- Add a comment to describe the purpose of the query -->",
      "translatedText": "<!-- Add a comment to describe the purpose of the query -->"
    },
    {
      "lineNumber": 218,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- Retrieve asset type information based on location ID and asset type IDs -->",
      "translatedText": "<!-- Retrieve asset type information based on location ID and asset type IDs -->"
    },
    {
      "lineNumber": 219,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- Input parameters: locationId (Integer), assetTypeIds (List<Integer>) -->",
      "translatedText": "<!-- Input parameters: locationId (Integer), assetTypeIds (List<Integer>) -->"
    },
    {
      "lineNumber": 220,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<!-- Result fields: assetTypeId, assetTypeName, label, parentId, locationId -->",
      "translatedText": "<!-- Result fields: assetTypeId, assetTypeName, label, parentId, locationId -->"
    }
  ]
}
```

### DEMOS4

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

```json
{
"translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace="com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmActMapper">\n <select id="getActiveAlarm" resultType="com.gds.jpi.dcim.model.alarm.AlarmAct">\n SELECT\n id,event_name,dc_id,dc_name,event_id,a.temp_event_id,temp_event_name,location_id,location_name,location_full_name,\n event_target_category_id,event_target_inst_id,event_target_inst_name,seq,event_target_type_id,event_target_type_name,\n event_level_id, last_event_level_id,major_id,event_type_id,message_type,recover_flag,confirm_flag,confirm_account,\n plan_flag,key_flag,reason_type_id,reason_descr,execute_id,incident_id,notice_id,break_time,update_time,alarm_unique\n FROM\n dcim.alarm_act a\n WHERE (recover_flag = 0 or (recover_flag = 1 AND confirm_flag = 0))\n AND\n dc_id IN\n <foreach collection="list" open="(" close=")" separator="," item="dcId">\n #{dcId}\n </foreach>\n <if test="labelId != null">\n AND EXISTS (\n SELECT\n 1\n FROM\n dcim.std_temp_event_label_rel r\n WHERE\n r.temp_event_id = a.temp_event_id\n )\n </if>\n ORDER BY confirm_flag ,event_level_id DESC,update_time DESC\n </select>\n <select id="getActiveAlarmById" resultType="com.gds.jpi.dcim.model.alarm.AlarmAct">\n SELECT\n id,event_name,dc_id,dc_name,event_id,temp_event_id,temp_event_name,location_id,location_name,location_full_name,\n event_target_category_id,event_target_inst_id,event_target_inst_name,seq,event_target_type_id,event_target_type_name,\n event_level_id, last_event_level_id,major_id,event_type_id,message_type,recover_flag,confirm_flag,confirm_account,\n plan_flag,key_flag,reason_type_id,reason_descr,execute_id,incident_id,notice_id,break_time,update_time,alarm_unique\n FROM\n dcim.alarm_act\n WHERE (recover_flag = 0 or (recover_flag = 1 AND confirm_flag = 0))\n AND\n id IN\n <foreach collection="list" open="(" close=")" separator="," item="id">\n #{id}\n </foreach>\n ORDER BY confirm_flag ,event_level_id DESC,update_time DESC\n </select>\n <select id="findAlarmRsp" resultType="com.gds.jpi.dcim.vo.event.AlarmSubscribeRspAlarm">\n SELECT\n a.id AS alarm_id,\n a.event_id,\n a.event_target_category_id,\n dtc.code AS event_target_category_code,\n dtc.name AS event_target_category_name,\n a.event_target_inst_id,\n a.event_target_inst_name,\n a.seq,\n a.temp_event_id,\n a.temp_event_name,\n a.event_name,\n a.dc_id,\n a.dc_name,\n a.location_id,\n a.location_name,\n a.event_level_id,\n dl.code AS event_level_code,\n dl.name AS event_level_name,\n a.major_id,\n m.code AS major_code,\n m.name AS major_name,\n a.event_type_id,\n et.code AS event_type_code,\n et.name AS event_type_name,\n a.message_type AS message_type_id,\n mt.code AS message_type_code,\n mt.name AS message_type_name,\n a.recover_flag,\n a.break_time,\n a.update_time\n FROM\n dcim.alarm_act a\n JOIN\n dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n JOIN\n dcim.dict_event_level dl ON dl.id = a.event_level_id\n JOIN\n dcim.dict_major m ON m.id = a.major_id\n JOIN\n dcim.dict_event_type et ON et.id = a.event_type_id\n JOIN\n dcim.dict_alarm_message_type mt ON mt.id = a.message_type\n WHERE\n a.id = #{alarmId}\n </select>\n <select id="findAlarmExecute" resultType="com.gds.jpi.dcim.core.alarmhandlercenter.request.EmergencyExecuteRequest">\n SELECT\n a.id AS alarm_id,\n a.alarm_content,\n a.event_name,\n a.event_target_category_id,\n dtc.code AS event_target_category_code,\n a.event_target_inst_id,\n a.event_target_inst_name,\n a.temp_event_id AS sceneId,\n a.dc_id,\n a.dc_name,\n a.location_id,\n a.location_name,\n a.execute_id,\n a.simulate_flag\n FROM\n dcim.alarm_act a\n JOIN\n dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n WHERE\n a.id = #{alarmId}\n </select>\n <select id="findRoccAlarmAct" resultType="com.gds.jpi.dcim.response.RoccAlarmActResponse">\n SELECT\n a.id AS alarm_id,\n a.alarm_content,\n a.event_target_category_id,\n dtc.code AS event_target_category_code,\n dtc.name AS event_target_category_name,\n a.event_target_inst_id,\n a.event_target_inst_name,\n a.seq,\n a.event_id,\n a.event_name,\n a.dc_id,\n a.dc_name,\n a.location_id,\n a.location_name,\n a.event_level_id,\n dl.code AS event_level_code,\n dl.name AS event_level_name,\n IF(ea.auto_notice = 0, 3, IF(notice_way &amp; 2, 1, 2)) AS notice_priority,\n IF(ea.auto_notice = 0, '低', IF(notice_way &amp; 2, '高', '中')) AS notice_priority_name,\n a.major_id,\n m.code AS major_code,\n m.name AS major_name,\n a.message_type AS message_type_id,\n mt.code AS message_type_code,\n mt.name AS message_type_name,\n a.recover_flag,\n a.simulate_flag,\n a.plan_flag,\n a.break_time,\n a.update_time,\n a.opt_time\n FROM\n dcim.alarm_act a\n JOIN\n dcim.std_temp_event_notice_cfg n ON n.temp_event_id = a.temp_event_id\n JOIN\n dcim.event_apply ea ON ea.dc_id = a.dc_id AND ea.temp_event_id = a.temp_event_id\n JOIN\n dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n JOIN\n dcim.dict_event_level dl ON dl.id = a.event_level_id\n JOIN\n dcim.dict_major m ON m.id = a.major_id\n JOIN\n dcim.dict_alarm_message_type mt ON mt.id = a.message_type\n WHERE\n a.event_type_id = 1\n AND a.dc_id IN\n <foreach collection="dcIds" item="dcId" open="(" separator="," close=")">\n #{dcId}\n </foreach>\n <if test="fromTime != null">\n AND a.opt_time >= #{fromTime}\n </if>\n <if test="endTime != null">\n AND a.opt_time <= #{endTime}\n </if>\n <if test="recoverFlag != null">\n AND a.recover_flag = #{recoverFlag}\n </if>\n </select>\n <select id="findRoccAlarmActSummary" resultType="com.gds.jpi.dcim.response.RoccAlarmSummaryMajorResponse">\n SELECT\n a.dc_id,\n a.dc_name,\n a.major_id,\n m.code AS major_code,\n m.name AS major_name,\n l.id AS level_id,\n l.code AS level_code,\n l.name AS level_name,\n COUNT(*) AS count\n FROM\n dcim.alarm_act a\n JOIN\n dcim.dict_major m ON m.id = a.major_id\n JOIN\n dcim.dict_event_level l ON l.id = a.event_level_id\n WHERE\n a.event_type_id = 1\n AND a.recover_flag = 0\n AND a.dc_id IN\n <foreach collection="dcIds" item="dcId" open="(" separator="," close=")">\n #{dcId}\n </foreach>\n GROUP BY\n a.dc_id,\n m.id,\n l.id\n </select>\n <select id="getNoticeResult" resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.PhoneResult">\n SELECT r.account, s.position, s.name,\n IF(r.notice_result = 2, 1, 0) AS result\n FROM\n cbms.alarm_notice n\n JOIN\n cbms.alarm_notice_phone_result r ON n.id = r.notice_id\n JOIN\n sms.staff s ON r.account = s.account\n WHERE n.is_new = 1 and alarm_id = #{alarm_id};\n </select>\n <select id="getNoticeResultWithOutPosition" resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.PhoneResult">\n SELECT r.account, s.name,\n IF(r.notice_result = 2, 1, 0) AS result\n FROM\n cbms.alarm_notice n\n JOIN\n cbms.alarm.notice_phone_result r ON n.id = r.notice_id\n JOIN\n sms.staff s ON r.account = s.account\n WHERE n.is_new = 1 and alarm_id = #{alarm_id};\n </select>\n</mapper>",
{ 
"details": []
}
}
```

### DEMOS5

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
"translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace="com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmCallbackMapper">\n <select id="findSubscribeByAlarmDc"\n resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">\n SELECT r.id AS record_id,\n r.business_code,\n r.business_guid,\n b.callback_url\n FROM dcim.alarm_subscribe_dc d\n JOIN\n dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 2\n JOIN\n dcim.alarm_subscribe_event_type t ON t.record_id = r.id\n JOIN\n dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n WHERE d.dc_id = #{alarm.dcId}\n AND t.event_type_id = #{alarm.eventTypeId}\n <if test="emergencyFlag">\n AND b.emergency_callback = 1\n </if>\n </select>\n <select id="findSubscribeByLabel"\n resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">\n SELECT\n DISTINCT\n r.id AS record_id,\n r.business_code,\n r.business_guid,\n b.callback_url\n FROM\n dcim.alarm_subscribe_dc d\n JOIN\n dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 3\n JOIN\n dcim.alarm_subscribe_event_type t ON t.record_id = r.id\n JOIN\n dcim.alarm_subscribe_event_label l ON l.record_id = r.id\n JOIN\n dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n JOIN\n dcim.std_temp_event_label_rel re ON re.temp_event_id = #{alarm.tempEventId} AND l.event_label_id = re.event_label_id\n WHERE\n d.dc_id = #{alarm.dcId}\n AND t.event_type_id = #{alarm.eventTypeId}\n <if test="emergencyFlag">\n AND b.emergency_callback = 1\n </if>\n </select>\n <select id="findSubscribeByAlarmTarget"\n resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">\n SELECT r.id AS record_id,\n r.business_code,\n r.business_guid,\n b.callback_url\n FROM dcim.alarm_subscribe_event_target d\n JOIN\n dcim.alarm_subscribe_record r ON r.id = d.record_id AND r.`status` = 1 AND r.type_id = 1\n JOIN\n dcim.alarm_subscribe_event_type t ON t.record_id = r.id\n JOIN\n dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n WHERE d.event_target_category_id = #{alarm.eventTargetCategoryId}\n AND d.event_target_inst_id = #{alarm.eventTargetInstId}\n AND d.temp_event_id IN (0, #{alarm.tempEventId})\n AND t.event_type_id = #{alarm.eventTypeId}\n <if test="emergencyFlag">\n AND b.emergency_callback = 1\n </if>\n </select>\n <select id="selectAlarmDetail"\n resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmCallbackAlarmDetail">\n SELECT\n a.id AS alarm_id,\n a.alarm_content,\n a.event_id,\n a.event_target_category_id,\n dtc.code AS event_target_category_code,\n dtc.name AS event_target_category_name,\n a.event_target_type_id,\n a.event_target_type_name,\n a.event_target_inst_id,\n a.event_target_inst_name,\n a.seq,\n a.temp_event_id,\n a.temp_event_name,\n a.event_name,\n a.dc_id,\n a.dc_name,\n a.location_id,\n a.location_name,\n a.event_level_id,\n dl.code AS event_level_code,\n dl.name AS event_level_name,\n a.major_id,\n m.code AS major_code,\n m.name AS major_name,\n a.event_type_id,\n et.code AS event_type_code,\n et.name AS event_type_name,\n a.message_type AS message_type_id,\n mt.code AS message_type_code,\n mt.name AS message_type_name,\n a.recover_flag,\n a.simulate_flag,\n a.execute_id,\n a.break_time,\n a.update_time\n FROM\n dcim.alarm_act a\n JOIN\n dcim.dict_event_target_category dtc ON dtc.id = a.event_target_category_id\n JOIN\n dcim.dict_event_level dl ON dl.id = a.event_level_id\n JOIN\n dcim.dict_major m ON m.id = a.major_id\n JOIN\n dcim.dict_event_type et ON et.id = a.event_type_id\n JOIN\n dcim.dict_alarm_message_type mt ON mt.id = a.message_type\n WHERE\n a.id IN\n <foreach collection="alarmIds" item="alarmId" open="(" separator="," close=")">\n #{alarmId}\n </foreach>\n </select>\n <select id="findCallbackRetry"\n resultType="com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail">\n SELECT l.record_id,\n r.business_code,\n l.business_guid,\n b.callback_url,\n l.id AS log_id,\n l.retry,\n l.alarm_id\n FROM dcim.alarm_subscribe_callback_log l\n JOIN\n dcim.alarm_subscribe_record r ON l.record_id = r.id AND r.status = 1\n JOIN\n dcim.alarm_subscribe_business b ON b.business_code = r.business_code\n WHERE l.create_time > #{date}\n AND l.retry < 10\n AND l.status = 1\n </select>\n</mapper>",
{
  "details": [
    {
      "lineNumber": 1,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>",
      "translatedText": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>"
    },
    {
      "lineNumber": 2,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">",
      "translatedText": "<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">"
    },
    {
      "lineNumber": 3,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "<mapper namespace=\"com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmCallbackMapper\">",
      "translatedText": "<mapper namespace=\"com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmCallbackMapper\">"
    },
    {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " <select id=\"findSubscribeByAlarmDc\"",
      "translatedText": " <select id=\"findSubscribeByAlarmDc\""
    },
    {
      "lineNumber": 5,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail\">",
      "translatedText": " resultType=\"com.gds.jpi.dcim.core.alarmhandlercenter.response.AlarmSubscribeDetail\">"
    },
    {
      "lineNumber": 6,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": " SELECT r.id AS record_id,",
      "translatedText": " SELECT r.id AS record_id,"
    }
  ]
}
}
```

### DEMOS6

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.mapper.SysCabinetLogDetailMapper">


    <!--通过实体作为筛选条件查询-->
    <select id="findList" resultType="com.gds.jpi.dcim.vo.CabinetLogDetailVo">
        SELECT
        detail.`id`,
        detail.`log_id`,
        detail.`field_code`,
        mapping.`name` AS field_name,
        detail.`before`,
        detail.`after`,
        mapping.`is_show`
        FROM cbms.sys_cabinet_log_detail detail
        LEFT JOIN cbms.sys_field_mapping mapping ON detail.`field_code` = mapping.`code` AND mapping.`biz_belong` = 'CABINET'
        <where>
            <if test="logId != null">
                AND detail.`log_id` = #{logId}
            </if>
            <if test="logIds != null and logIds.size() > 0">
                AND detail.`log_id` IN
                <foreach collection="logIds" open="(" close=")" separator="," item="item">
                    #{item}
                </foreach>
            </if>
        </where>
    </select>

</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.mapper.SysCabinetLogDetailMapper\">\n\n\n <!--Query using an entity as the filter criteria-->\n <select id=\"findList\" resultType=\"com.gds.jpi.dcim.vo.CabinetLogDetailVo\">\n SELECT\n detail.id,\n detail.log_id,\n detail.field_code,\n mapping.name AS field_name,\n detail.before,\n detail.after,\n mapping.is_show\n FROM cbms.sys_cabinet_log_detail detail\n LEFT JOIN cbms.sys_field_mapping mapping ON detail.field_code = mapping.code AND mapping.biz_belong = 'CABINET'\n <where>\n <if test=\"logId != null\">\n AND detail.log_id = #{logId}\n </if>\n <if test=\"logIds != null and logIds.size() > 0\">\n AND detail.log_id IN\n <foreach collection=\"logIds\" open=\"(\" close=\")\" separator=\",\" item=\"item\">\n #{item}\n </foreach>\n </if>\n </where>\n </select>\n\n</mapper>",
  "details": [
    {
      "lineNumber": 5,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "通过实体作为筛选条件查询",
      "translatedText": "Query using an entity as the filter criteria"
    }
  ]
}

```
### DEMOS7

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.gdh.scm.infra.mapper.PoLineMapper">
    <!-- 可根据自己的需求，是否要使用 -->
    <resultMap id="BaseResultMap" type="com.gdh.scm.domain.entity.PoLine">
        <result column="po_line_id" property="poLineId" jdbcType="DECIMAL"/>
        <result column="po_header_id" property="poHeaderId" jdbcType="DECIMAL"/>
        <result column="line_num" property="lineNum" jdbcType="DECIMAL"/>
        <result column="organization_id" property="organizationId" jdbcType="DECIMAL"/>
        <result column="inventory_id" property="inventoryId" jdbcType="DECIMAL"/>
        <result column="item_id" property="itemId" jdbcType="DECIMAL"/>
        <result column="item_number" property="itemNumber" jdbcType="VARCHAR"/>
        <result column="item_description" property="itemDescription" jdbcType="VARCHAR"/>
        <result column="specifications" property="specifications" jdbcType="VARCHAR"/>
        <result column="uom_id" property="uomId" jdbcType="DECIMAL"/>
        <result column="quantity" property="quantity" jdbcType="DECIMAL"/>
        <result column="shortage_percentage" property="shortagePercentage" jdbcType="DECIMAL"/>
        <result column="excess_percentage" property="excessPercentage" jdbcType="DECIMAL"/>
        <result column="tax_id" property="taxId" jdbcType="DECIMAL"/>
        <result column="tax_rate" property="taxRate" jdbcType="DECIMAL"/>
        <result column="net_price" property="netPrice" jdbcType="DECIMAL"/>
        <result column="tax_included_price" property="taxIncludedPrice" jdbcType="DECIMAL"/>
        <result column="net_amount" property="netAmount" jdbcType="DECIMAL"/>
        <result column="tax_included_amount" property="taxIncludedAmount" jdbcType="DECIMAL"/>
        <result column="tax_amount" property="taxAmount" jdbcType="DECIMAL"/>
        <result column="need_date" property="needDate" jdbcType="DATE"/>
        <result column="remark" property="remark" jdbcType="VARCHAR"/>
        <result column="asset_name" property="assetName" jdbcType="VARCHAR"/>
        <result column="asset_specifications" property="assetSpecifications" jdbcType="VARCHAR"/>
        <result column="quantity_received" property="quantityReceived" jdbcType="DECIMAL"/>
        <result column="quantity_delivered" property="quantityDelivered" jdbcType="DECIMAL"/>
        <result column="receive_flag" property="receiveFlag" jdbcType="DECIMAL"/>
        <result column="po_line_status" property="poLineStatus" jdbcType="VARCHAR"/>
        <result column="demand_detail_flag" property="demandDetailFlag" jdbcType="DECIMAL"/>
        <result column="external_system_code" property="externalSystemCode" jdbcType="VARCHAR"/>
        <result column="source_code" property="sourceCode" jdbcType="VARCHAR"/>
        <result column="tenant_id" property="tenantId" jdbcType="DECIMAL"/>
        <result column="object_version_number" property="objectVersionNumber" jdbcType="DECIMAL"/>
        <result column="creation_date" property="creationDate" jdbcType="DATE"/>
        <result column="created_by" property="createdBy" jdbcType="DECIMAL"/>
        <result column="last_updated_by" property="lastUpdatedBy" jdbcType="DECIMAL"/>
        <result column="last_update_date" property="lastUpdateDate" jdbcType="DATE"/>
        <result column="ATTRIBUTE_CATEGORY" property="attributeCategory" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE1" property="attribute1" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE2" property="attribute2" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE3" property="attribute3" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE4" property="attribute4" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE5" property="attribute5" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE6" property="attribute6" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE7" property="attribute7" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE8" property="attribute8" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE9" property="attribute9" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE10" property="attribute10" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE11" property="attribute11" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE12" property="attribute12" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE13" property="attribute13" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE14" property="attribute14" jdbcType="VARCHAR"/>
        <result column="ATTRIBUTE15" property="attribute15" jdbcType="VARCHAR"/>
        <result column="purchase_category" property="purchaseCategory" jdbcType="VARCHAR"/>
        <result column="po_remark" property="poRemark" jdbcType="VARCHAR"/>
        <result column="settlement_method" property="settlementMethod" jdbcType="VARCHAR" />
        <result column="source_line_id" property="sourceLineId" jdbcType="DECIMAL" />
        <!-- 非数据库字段 -->
        <result column="organization_name" property="organizationName" jdbcType="VARCHAR"/>
        <result column="uom_name" property="uomName" jdbcType="VARCHAR"/>
        <result column="ou_id" property="ouId" jdbcType="DECIMAL"/>
        <result column="supplier_id" property="supplierId" jdbcType="DECIMAL"/>
        <result column="inventory_name" property="inventoryName" jdbcType="VARCHAR"/>
        <result column="header_source_code" property="headerSourceCode" jdbcType="VARCHAR"/>
        <result column="category_segment3" property="categorySegment3" jdbcType="VARCHAR" />
        <result column="requisition_ou_id" property="requisitionOuId" jdbcType="VARCHAR"/>
        <result column="centralized_acquisition_center" property="centralizedAcquisitionCenter" jdbcType="VARCHAR"/>
    </resultMap>

    <sql id="basic_column">
        <!--@sql select -->
        gpl.po_line_id,
        gpl.po_header_id,
        gpl.line_num,
        gpl.organization_id,
        gpl.inventory_id,
        gpl.item_id,
        gpl.item_number,
        gpl.item_description,
        gpl.specifications,
        gpl.uom_id,
        gpl.quantity,
        gpl.shortage_percentage,
        gpl.excess_percentage,
        gpl.tax_id,
        gpl.tax_rate,
        gpl.net_price,
        gpl.tax_included_price,
        gpl.net_amount,
        gpl.tax_included_amount,
        gpl.tax_amount,
        gpl.need_date,
        gpl.remark,
        gpl.quantity_received,
        gpl.quantity_delivered,
        gpl.receive_flag,
        gpl.po_line_status,
        gpl.external_system_code,
        gpl.source_code,
        gpl.project_id,
        gpl.tenant_id,
        gpl.object_version_number,
        gpl.creation_date,
        gpl.created_by,
        gpl.last_updated_by,
        gpl.last_update_date,
        gpl.ATTRIBUTE_CATEGORY,
        gpl.ATTRIBUTE1,
        gpl.ATTRIBUTE2,
        gpl.ATTRIBUTE3,
        gpl.ATTRIBUTE4,
        gpl.ATTRIBUTE5,
        gpl.ATTRIBUTE6,
        gpl.ATTRIBUTE7,
        gpl.ATTRIBUTE8,
        gpl.ATTRIBUTE9,
        gpl.ATTRIBUTE10,
        gpl.ATTRIBUTE11,
        gpl.ATTRIBUTE12,
        gpl.ATTRIBUTE13,
        gpl.ATTRIBUTE14,
        gpl.ATTRIBUTE15,
        gpl.purchase_category,
        gpl.asset_name,
        gpl.asset_specifications,
        gpl.deduct_flag,
        gpl.source_header_id,
        gpl.source_line_id,
        gpl.settlement_method
        <!--@sql from gscm_po_line gpl-->
    </sql>

    <select id="listPoLine" resultType="com.gdh.scm.domain.entity.PoLine">
        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>
        SELECT <include refid="basic_column"/> ,
               hio.organization_name,
               gph.source_code header_source_code,
               hi.inventory_name,
               hut.uom_name,
               case
                   when (select count(pll.line_location_id)
                         from gscm_po_line_location pll
                         where gpl.po_line_id = pll.po_line_id) >
                        0
                       then 1
                   else 0 end  demandDetailFlag,
               CASE
                   WHEN gph.source_code = 'INNER_QO' THEN (SELECT giqh.inner_quotation_num
                                                           from gscm_inner_quotation_header giqh
                                                           where giqh.inner_quotation_header_id = gpl.source_header_id)
                   WHEN gph.source_code = 'OUTSIDE_PO' THEN (SELECT gph2.po_num
                                                             from gscm_po_header gph2
                                                             where gph2.po_header_id =
                                                                   gpl.source_header_id)
                   WHEN gph.source_code = 'SALES_ORDER' THEN (SELECT gsh.so_num
                                                              from gscm_so_header gsh
                                                              where gsh.so_header_id =
                                                                    gpl.source_header_id)
                   ELSE NULL
                   END         source_num,
               CASE
                   WHEN gph.source_code = 'OUTSIDE_PO' THEN (SELECT gpl2.line_num
                                                             from gscm_po_line gpl2
                                                             where gpl2.po_line_id =
                                                                   gpl.source_line_id)
                   WHEN gph.source_code = 'SALES_ORDER' THEN (SELECT gsl.line_num
                                                              from gscm_so_line gsl
                                                              where gsl.so_line_id =
                                                                    gpl.source_line_id)
                   ELSE NULL
                   END         source_line_num,
               gp.project_num,
               gp.project_name
        FROM gscm_po_line gpl
                 LEFT JOIN gdh_mdm.gmdm_project gp on gp.project_id = gpl.project_id
                 LEFT JOIN hpfm_inv_organization hio
                           ON gpl.organization_id = hio.organization_id
                 LEFT JOIN gscm_po_header gph ON gph.po_header_id = gpl.po_header_id
                 LEFT JOIN hpfm_inventory hi
                           ON gpl.inventory_id = hi.inventory_id
                 LEFT JOIN hpfm_uom hu
                           ON gpl.uom_id = hu.uom_id
                 LEFT JOIN hpfm_uom_tl hut
                           ON hu.uom_id = hut.uom_id
                               AND hut.lang = #{lang}
        WHERE gpl.po_header_id = #{poHeaderId}
    </select>

    <select id="listPoLineByShipment" resultMap="BaseResultMap">
        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>
        <bind name="userDetails" value="@io.choerodon.core.oauth.DetailsHelper@getUserDetails()"/>
        <bind name="employee"
              value="@org.hzero.boot.platform.plugin.hr.EmployeeHelper@getEmployee(userDetails.userId,userDetails.tenantId)"/>
        <bind name="ouAuthority" value="@com.gdh.scm.infra.helper.UserAuthorityHelper@userAuthority('OU')"/>
        SELECT
        <include refid="basic_column"/>,
        gi.category_segment3,
        hio.organization_name,
        hi.inventory_name,
        hut.uom_name,
        gph.po_num,
        gph.supplier_id,
        gph.supplier_site_id,
        hou.ou_code,
        gph.ou_id,
        gph.remark po_remark,
        gph.po_category,
        gph.unified_sign_flag,
        gph.source_code header_source_code,
        gph.requisition_ou_id,
        gph.centralized_acquisition_center,
        hunit.unit_name departmentName,
        (CASE WHEN (select count(pll.line_location_id)
                    from gscm_po_line_location pll
                    where gpl.po_line_id = pll.po_line_id) > 0
        THEN 1 ELSE 0 END) demand_detail_flag
        FROM gscm_po_line gpl
        LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
        LEFT JOIN hzero_platform.hpfm_unit hunit on gph.department_id = hunit.unit_id
        LEFT JOIN hzero_platform.hpfm_inv_organization hio ON gpl.organization_id = hio.organization_id
        LEFT JOIN hzero_platform.hpfm_operation_unit hou on hou.ou_id = hio.ou_id
        LEFT JOIN hzero_platform.hpfm_inventory hi ON gpl.inventory_id = hi.inventory_id
        LEFT JOIN hzero_platform.hpfm_uom hu ON gpl.uom_id = hu.uom_id
        LEFT JOIn gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id
        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}
        <where>
            gpl.tenant_id = #{tenantId}
            and gpl.receive_flag = 1
            and gph.po_status = 'APPROVED'
            AND gpl.organization_id = #{organizationId}
            AND (gph.ou_id = #{ouId}
            AND gph.supplier_id = #{supplierId}
            AND gph.supplier_site_id = #{supplierSiteId}
            AND gpl.purchase_category = #{purchaseCategory}
            and (( 1=1
            <if test="ouAuthority.includeAllFlag != true">
                AND gph.ou_id IN
                <foreach collection="ouAuthority.dataIds" item="dataId" open="(" close=")" separator=",">
                    #{dataId}
                </foreach>
            </if>)
            <if test="directlySendFlag != null and directlySendFlag == 1">
                <bind name="employeeIdLike" value="'%'+employee.employeeId+'%'"/>
                OR gph.receipt_id LIKE #{employeeIdLike}
            </if>)
            )
            <if test="directlySendFlag != null and directlySendFlag == 1">
                AND gph.source_code = 'SALES_ORDER'
            </if>
            <if test="directlySendFlag == null or directlySendFlag != 1">
                AND ((gph.unified_sign_flag = 0 AND gph.po_category = 'EXTERNAL')
                         AND gph.source_code != 'SALES_ORDER'
                    OR (gph.unified_sign_flag = 1 AND gph.po_category = 'INTERNAL'))
            </if>
            <if test="poNum != null and poNum != '' ">
                <bind name="poNumLike" value="'%'+ poNum  + '%'"/>
                AND gph.po_num like #{poNumLike}
            </if>
            <if test="itemNumber != null and itemNumber != '' ">
                <bind name="itemNumberLike" value="'%'+ itemNumber  + '%'"/>
                AND gpl.item_number like #{itemNumberLike}
            </if>
            <if test="assetName != null and assetName != '' ">
                <bind name="assetNameLike" value="'%'+ assetName  + '%'"/>
                AND gpl.asset_name like #{assetNameLike}
            </if>
            <if test="assetSpecifications != null and assetSpecifications != '' ">
                <bind name="assetSpecificationsLike" value="'%'+ assetSpecifications  + '%'"/>
                AND gpl.asset_specifications like #{assetSpecificationsLike}
            </if>
            <if test="itemDescription != null and itemDescription != '' ">
                <bind name="itemDescriptionLike" value="'%'+ itemDescription  + '%'"/>
                AND gpl.item_description like #{itemDescriptionLike}
            </if>
            <if test="departmentId != null ">
                AND gph.department_id = #{departmentId}
            </if>
            <if test="requestorId != null ">
                AND gph.agent_id = #{requestorId}
            </if>
            <if test="requisitionOuId != null ">
                AND gph.requisition_ou_id = #{requisitionOuId}
            </if>
            <if test="inventoryId != null ">
                AND hi.inventory_id = #{inventoryId}
            </if>
            <if test="creationDate != null ">
                AND date_format(gpl.creation_date, '%Y-%m-%d') = date_format(#{creationDate}, '%Y-%m-%d')
            </if>
            <if test="poRemark!=null and poRemark!=''">
                <bind name="poRemarkLike" value="'%' + poRemark + '%'"/>
                AND gph.remark LIKE #{poRemarkLike}
            </if>
            <if test="relationPoNum != null">
                <bind name="relationPoNumLike" value="'%'+ relationPoNum +'%'"/>
                AND EXISTS(
                SELECT 1 FROM gscm_po_header gph2 WHERE gph2.po_header_id=gph.relation_po_header_id
                AND gph2.po_num LIKE #{relationPoNumLike}
                )
            </if>
            <if test="projectId">
                AND gpl.project_id = #{projectId}
            </if>
            <if test="requisitionNum != null">
                <bind name="requisitionNumLike" value="'%'+requisitionNum +'%'"/>
                AND EXISTS (SELECT 1
                FROM gscm_po_line_location gpll
                JOIN gscm_po_requisition_header gprh
                ON gpll.requisition_header_id = gprh.requisition_header_id
                WHERE gpll.po_line_id = gpl.po_line_id
                AND gprh.requisition_num like #{requisitionNumLike})
            </if>
        </where>
        order by gph.po_num, gpl.line_num
    </select>

    <select id="getCurrentMaxLineNumNext" resultType="java.lang.Long">
        SELECT IFNULL(MAX(gpl.line_num), 0) + 1
        FROM gscm_po_line gpl
        WHERE gpl.po_header_id = #{poHeaderId}
    </select>

    <select id="sumAmount" resultMap="BaseResultMap">
        SELECT IFNULL(SUM(gpl.net_amount), 0)          AS net_amount,
               IFNULL(SUM(gpl.tax_included_amount), 0) AS tax_included_amount
        FROM gscm_po_line gpl
        WHERE gpl.po_header_id = #{poHeaderId}
    </select>

    <select id="getRecentPoLine" resultMap="BaseResultMap">
        SELECT gpl.*
        FROM gscm_po_line gpl,
             gscm_po_header gph
        WHERE gpl.po_header_id = gph.po_header_id
          AND gph.tenant_id = #{tenantId}
          AND gph.ou_id = #{ouId}
          AND gph.supplier_id = #{supplierId}
          AND gpl.item_id = #{itemId}
          AND gph.po_status = 'APPROVED'
        ORDER BY gph.po_header_id DESC, gpl.po_line_id DESC LIMIT 1
    </select>

    <select id="queryLineByPoNumAndLineNum" resultMap="BaseResultMap">
        SELECT gpl.*
        FROM gscm_po_line gpl
                 LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
        WHERE gph.po_num = #{poNum}
          AND gpl.line_num = #{lineNum}
          AND gpl.tenant_id = #{tenantId}
    </select>

    <select id="executionReport" resultType="com.gdh.scm.domain.vo.PoLineVO">
        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>
        SELECT hou.ou_name,
        hio.organization_name,
        gph.supplier_num,
        gph.supplier_name,
        gph.supplier_site_name,
        gpl.purchase_category,
        he.name agentor,
        gph.po_num,
        gph.po_status,
        gph.po_creation_date,
        gpl.line_num,
        gpl.item_id,
        gpl.item_number,
        gpl.item_description,
        gpl.specifications,
        gpl.quantity,
        gpl.settlement_method,
        hut.uom_name,
        gpl.tax_included_amount,
        gpl.tax_rate,
        gpl.po_line_id,
        gpl.tax_included_price,
        gi.uom_id item_uom_id,
        gpl.uom_id,
        gpl.tenant_id,
        item_hut.uom_name item_uom_name,
        grsl.line_num shipment_line_num,
        grsl.tax_included_price shipment_tax_included_price,
        grsl.shipment_line_id,
        grsh.shipment_num,
        gph.contract_num,
        gph.contract_name
        FROM gscm_po_line gpl
        JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
        JOIN gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id
        JOIN hzero_platform.hpfm_uom item_hu ON item_hu.uom_id = gi.uom_id
        LEFT JOIN hzero_platform.hpfm_uom_tl item_hut
        ON item_hut.uom_id = item_hu.uom_id AND item_hut.lang = #{lang}
        JOIN hzero_platform.hpfm_operation_unit hou ON hou.ou_id = gph.ou_id
        LEFT JOIN hzero_platform.hpfm_inv_organization hio ON hio.organization_id = gpl.organization_id
        LEFT JOIN hzero_platform.hpfm_employee he ON he.employee_id = gph.agent_id
        JOIN hzero_platform.hpfm_uom hu ON hu.uom_id = gpl.uom_id
        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}
        LEFT JOIN gscm_rcv_shipment_line grsl ON gpl.po_line_id = grsl.po_line_id
        LEFT JOIN gscm_rcv_shipment_header grsh ON grsh.shipment_header_id = grsl.shipment_header_id
        WHERE gph.ou_id = #{ouId}
        AND gpl.organization_id = #{organizationId}
        AND (gph.po_status = 'APPROVED' OR gph.po_status = 'CLOSED')
        AND (grsh.shipment_header_id IS NULL OR grsh.document_type = 'INSPECTION')
        <if test="inventoryId != null">
            AND gpl.inventory_id = #{inventoryId}
        </if>
        <if test="agentId != null">
            AND gph.agent_id = #{agentId}
        </if>
        <if test="poNum != null and poNum != ''">
            <bind name="poNumLike" value="'%' + poNum + '%'"/>
            AND gph.po_num LIKE #{poNumLike}
        </if>
        <if test="supplierId != null">
            AND gph.supplier_id = #{supplierId}
        </if>
        <if test="contractNum != null and contractNum != ''">
            <bind name="contractNumLike" value="'%' + contractNum + '%'"/>
            AND gph.contract_num LIKE #{contractNumLike}
        </if>
        <if test="itemNumberFrom != null and itemNumberFrom != ''">
            AND gpl.item_number <![CDATA[>=]]> #{itemNumberFrom}
        </if>
        <if test="itemNumberTo != null and itemNumberTo != ''">
            AND gpl.item_number <![CDATA[<=]]> #{itemNumberTo}
        </if>
        <if test="operationDateFrom != null">
            AND gph.po_creation_date <![CDATA[>=]]> #{operationDateFrom}
        </if>
        <if test="operationDateTo != null">
            AND gph.po_creation_date <![CDATA[<=]]> #{operationDateTo}
        </if>
        <if test="departmentId != null">
            AND EXISTS(SELECT 1
            FROM hzero_platform.hpfm_employee_assign hea
            WHERE hea.employee_id = gph.agent_id
            AND hea.enabled_flag = 1
            AND hea.unit_id = #{departmentId}
            )
        </if>
        ORDER BY gph.supplier_num, gph.po_num, gpl.line_num
    </select>

    <select id="executionReport2" resultType="com.gdh.scm.domain.vo.PoLineVO">
        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>
        SELECT hou.ou_name,
        hio.organization_name,
        gph.supplier_name platform_company_name,
        gph2.supplier_num,
        gph2.supplier_name,
        gph.supplier_site_name,
        gpl.purchase_category,
        he.name agentor,
        gph.po_num,
        gph.po_status,
        gph.po_creation_date,
        gpl.line_num,
        gpl.item_id,
        gpl.item_number,
        gpl.item_description,
        gpl.specifications,
        gpl.quantity,
        gpl.settlement_method,
        hut.uom_name,
        gpl.tax_included_amount,
        gpl.tax_rate,
        gpl.po_line_id,
        gpl.tax_included_price,
        gi.uom_id item_uom_id,
        gpl.uom_id,
        gpl.tenant_id,
        item_hut.uom_name item_uom_name,
        grsl.line_num shipment_line_num,
        grsl.tax_included_price shipment_tax_included_price,
        grsl.shipment_line_id,
        grsh.shipment_num
        FROM gscm_po_line gpl
        JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
        LEFT JOIN gscm_po_header gph2 ON gph.relation_po_header_id = gph2.po_header_id
        LEFT JOIN (select distinct ou_name<if test="platformCompanyId != null">, ou_id</if>
                   from hzero_platform.hpfm_operation_unit<if test="platformCompanyId != null"> where ou_id = #{platformCompanyId}</if>) hou2
            ON hou2.ou_name = gph.supplier_name
        JOIN gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id
        JOIN hzero_platform.hpfm_uom item_hu ON item_hu.uom_id = gi.uom_id
        LEFT JOIN hzero_platform.hpfm_uom_tl item_hut
        ON item_hut.uom_id = item_hu.uom_id AND item_hut.lang = #{lang}
        JOIN hzero_platform.hpfm_operation_unit hou ON hou.ou_id = gph.ou_id
        LEFT JOIN hzero_platform.hpfm_inv_organization hio ON hio.organization_id = gpl.organization_id
        LEFT JOIN hzero_platform.hpfm_employee he ON he.employee_id = gph.agent_id
        JOIN hzero_platform.hpfm_uom hu ON hu.uom_id = gpl.uom_id
        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}
        LEFT JOIN gscm_rcv_shipment_line grsl ON gpl.po_line_id = grsl.po_line_id
        LEFT JOIN gscm_rcv_shipment_header grsh ON grsh.shipment_header_id = grsl.shipment_header_id
        WHERE gph.ou_id = #{ouId} AND gph.source_code = 'OUTSIDE_PO'
        AND gpl.organization_id = #{organizationId}
        AND (gph.po_status = 'APPROVED' OR gph.po_status = 'CLOSED')
        AND (grsh.shipment_header_id IS NULL OR grsh.document_type = 'INSPECTION')
        <if test="agentId != null">
            AND gph.agent_id = #{agentId}
        </if>
        <if test="platformCompanyId != null">
            AND hou2.ou_id = #{platformCompanyId}
        </if>
        <if test="poNum != null and poNum != ''">
            <bind name="poNumLike" value="'%' + poNum + '%'"/>
            AND gph.po_num LIKE #{poNumLike}
        </if>
        <if test="supplierId != null">
            AND gph2.supplier_id = #{supplierId}
        </if>
        <if test="contractNum != null and contractNum != ''">
            <bind name="contractNumLike" value="'%' + contractNum + '%'"/>
            AND gph.contract_num LIKE #{contractNumLike}
        </if>
        <if test="itemNumberFrom != null and itemNumberFrom != ''">
            AND gpl.item_number <![CDATA[>=]]> #{itemNumberFrom}
        </if>
        <if test="itemNumberTo != null and itemNumberTo != ''">
            AND gpl.item_number <![CDATA[<=]]> #{itemNumberTo}
        </if>
        <if test="operationDateFrom != null">
            AND gph.po_creation_date <![CDATA[>=]]> #{operationDateFrom}
        </if>
        <if test="operationDateTo != null">
            AND gph.po_creation_date <![CDATA[<=]]> #{operationDateTo}
        </if>
        <if test="departmentId != null">
            AND EXISTS(SELECT 1
            FROM hzero_platform.hpfm_employee_assign hea
            WHERE hea.employee_id = gph.agent_id
            AND hea.enabled_flag = 1
            AND hea.unit_id = #{departmentId}
            )
        </if>
        ORDER BY gph.supplier_num, gph.po_num, gpl.line_num
    </select>

    <select id="queryPoLinesByItemAndUom" resultMap="BaseResultMap">
        SELECT gpl.tax_included_price,
               gpl.item_id,
               gpl.uom_id,
               gpl.po_line_id,
               gpl.tax_id,
               gpl.tax_rate
        FROM gscm_po_line gpl
                 JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
        WHERE gph.po_status = 'APPROVED'
          AND gpl.item_id = #{itemId}
          AND gpl.uom_id = #{uomId}
        ORDER BY gpl.po_line_id DESC LIMIT 1
    </select>

    <select id="queryRelateInnerPoLine" resultType="com.gdh.scm.domain.entity.PoLine">
        SELECT * FROM gscm_po_line gpl
        WHERE gpl.source_line_id IN
        <foreach collection="poLineIds" open="(" close=")" separator="," item="poLineId">
            #{poLineId}
        </foreach>
    </select>

    <select id="selectOuterPoLines" resultType="com.gdh.scm.api.dto.StockPoLineDTO">
        SELECT inner_gisl.stock_line_id      inner_stock_line_id,
               out_grsl.shipment_header_id,
               out_grsl.shipment_line_id,
               inner_grsl.shipment_header_id inner_shipment_header_id,
               inner_grsl.shipment_line_id   inner_shipment_line_id,
               out_grsh.shipment_num,
               out_grsl.line_num             shipment_line_num,
               out_gph.po_num,
               out_gpl.line_num              po_line_num,
               out_gpl.organization_id,
               out_gph.ou_id,
               out_grsh.supplier_id,
               out_grsh.supplier_site_id,
               out_grsl.item_id,
               goi.batch_flag,
               out_gph.po_category,
               out_gph.unified_sign_flag,
               case out_gpl.deduct_flag when 1 then grt.net_price else grt.tax_included_price end transaction_cost
        FROM gscm_po_line out_gpl,
             gscm_po_header out_gph,
             gscm_rcv_shipment_line out_grsl,
             gscm_rcv_shipment_header out_grsh,
             gscm_rcv_shipment_line inner_grsl,
             gscm_po_line inner_gpl,
             gscm_po_header inner_gph,
             gscm_inv_stock_line inner_gisl,
             gmdm_organization_item goi,
             gscm_rcv_transaction grt
        WHERE out_gpl.po_line_id = out_grsl.po_line_id
          AND out_gpl.po_header_id = out_gph.po_header_id
          AND out_grsl.shipment_header_id = out_grsh.shipment_header_id
          AND out_grsl.orginal_shipment_line_id = inner_grsl.shipment_line_id
          AND inner_gisl.source_document_line_id = inner_grsl.shipment_line_id
          AND out_gpl.item_id = goi.item_id
          AND goi.organization_id = out_gpl.organization_id
          AND inner_gpl.po_line_id = inner_grsl.po_line_id
          AND inner_gph.po_header_id = inner_gpl.po_header_id
          AND inner_gph.unified_sign_flag = 1
          AND inner_gph.po_category = 'INTERNAL'
          AND (grt.shipment_line_id=out_grsl.shipment_line_id AND grt.return_flag=0)
          AND inner_gisl.stock_line_id IN
        <foreach collection="stockLineIds" open="(" close=")" separator="," item="item">
            #{item}
        </foreach>
    </select>
    <select id="getPoLineByPoLineId" resultType="com.gdh.scm.domain.entity.PoLine">
        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>
        <bind name="userDetails" value="@io.choerodon.core.oauth.DetailsHelper@getUserDetails()"/>
        SELECT
            <include refid="basic_column"/>,
            gpl.source_line_id,
            gi.category_segment3,
            hio.organization_name,
            hi.inventory_name,
            hut.uom_name,
            gph.po_num,
            gph.supplier_id,
            hou.ou_code,
            gph.ou_id,
            gph.remark po_remark,
            gph.po_category,
            gph.unified_sign_flag,
            gph.supplier_site_id,
            gph.source_code header_source_code,
            gph.requisition_ou_id,
            gph.centralized_acquisition_center
        FROM gscm_po_line gpl
        LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
        LEFT JOIN hpfm_inv_organization hio ON gpl.organization_id = hio.organization_id
        LEFT JOIN hpfm_operation_unit hou on hou.ou_id = hio.ou_id
        LEFT JOIN hpfm_inventory hi ON gpl.inventory_id = hi.inventory_id
        LEFT JOIN hpfm_uom hu ON gpl.uom_id = hu.uom_id
        LEFT JOIn gmdm_item gi ON gi.item_id = gpl.item_id
        LEFT JOIN hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}
        WHERE gpl.po_line_id = #{poLineId}
    </select>
    <select id="detailReport" resultType="com.gdh.scm.domain.vo.PoLineDetailReportVO">
        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>
        SELECT hou.ou_name,
               hou.ou_code,
               gph.centralized_acquisition_center,
               rhou.ou_name                requisition_ou_name,
               gph.supplier_name,
               gph.supplier_num,
               gph.supplier_site_name,
               gph.po_num,
               he.name                     agentor,
               group_concat(rhe.name)      receipt_name,
               group_concat(rhe.mobile) as receipt_mobile,
               gph.receipt_address,
               gph.po_creation_date,
               gpl.line_num,
               gpl.item_id,
               gpl.item_number,
               gpl.item_description,
               gpl.specifications,
               gpl.uom_id,
               gpl.tenant_id,
               gpl.quantity,
               hut.uom_name,
               gpl.tax_included_amount,
               gpl.po_line_id,
               gpl.tax_included_price,
               gpl.tax_rate,
               gpl.need_date,
               gpl.remark
        FROM gscm_po_line gpl
        LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
        JOIN hzero_platform.hpfm_operation_unit hou ON hou.ou_id = gph.ou_id
        LEFT JOIN hzero_platform.hpfm_operation_unit rhou ON rhou.ou_id = gph.requisition_ou_id
        LEFT JOIN hzero_platform.hpfm_employee he ON he.employee_id = gph.agent_id
        LEFT JOIN hzero_platform.hpfm_employee rhe ON find_in_set(rhe.employee_id, gph.receipt_id)
        JOIN hzero_platform.hpfm_uom hu ON hu.uom_id = gpl.uom_id
        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}
        WHERE
        gph.unified_sign_flag = 1
        AND gph.po_category = 'EXTERNAL'
        AND gph.po_status = 'APPROVED'
        <if test="ouId != null">
            AND gph.ou_id = #{ouId}
        </if>
        <if test="supplierId != null">
            AND gph.supplier_id = #{supplierId}
        </if>
        <if test="supplierSiteId != null">
            AND gph.supplier_site_id = #{supplierSiteId}
        </if>
        <if test="poNum != null">
            <bind name="poNumLike" value="'%' +poNum+ '%'"/>
            AND gph.po_num LIKE #{poNumLike}
        </if>
        <if test="agentId != null">
            AND gph.agent_id = #{agentId}
        </if>
        <if test="centralizedAcqCenter != null">
            AND gph.centralized_acquisition_center = #{centralizedAcqCenter}
        </if>
        <if test="contractNum != null">
            <bind name="contractNumLike" value="'%'+contractNum+'%'"/>
            AND gph.contract_num LIKE #{contractNumLike}
        </if>
        <if test="operationDateFrom != null">
            AND gph.po_creation_date <![CDATA[>=]]> #{operationDateFrom}
        </if>
        <if test="operationDateTo != null">
            AND gph.po_creation_date <![CDATA[<=]]> #{operationDateTo}
        </if>
        <if test="requisitionOuId != null">
            AND gph.requisition_ou_id = #{requisitionOuId}
        </if>
        GROUP BY hou.ou_name,
                 hou.ou_code,
                 gph.centralized_acquisition_center,
                 rhou.ou_name,
                 gph.supplier_name,
                 gph.supplier_num,
                 gph.supplier_site_name,
                 gph.po_num,
                 he.name,
                 gph.receipt_address,
                 gph.po_creation_date,
                 gpl.line_num,
                 gpl.item_id,
                 gpl.item_number,
                 gpl.item_description,
                 gpl.specifications,
                 gpl.uom_id,
                 gpl.tenant_id,
                 gpl.quantity,
                 hut.uom_name,
                 gpl.tax_included_amount,
                 gpl.po_line_id,
                 gpl.tax_included_price,
                 gpl.tax_rate,
                 gpl.need_date
        ORDER BY hou.ou_code,gph.supplier_num,gph.po_num,gpl.line_num
    </select>

    <select id="queryPoLineReport" resultMap="BaseResultMap">
        SELECT
            gpl.line_num,
            hio.organization_name,
            gpl.item_number,
            CASE

                WHEN gpl.purchase_category = 'FIXED_ASSETS' THEN
                    gpl.asset_name ELSE gpl.item_description
                END item_description,
            CASE

                WHEN gpl.purchase_category = 'FIXED_ASSETS' THEN
                    gpl.asset_specifications ELSE gpl.specifications
                END specifications,
            hut.uom_name,
            CONCAT( 0 + CAST( gpl.quantity AS CHAR ), '' ) quantity,
            CONCAT( 0 + CAST( gpl.shortage_percentage AS CHAR ), '' ) shortage_percentage,
            CONCAT( 0 + CAST( gpl.excess_percentage AS CHAR ), '' ) excess_percentage,
            gpl.tax_rate,
            CONCAT( 0 + CAST( gpl.tax_included_price AS CHAR ), '' ) tax_included_price,
            CONCAT( 0 + CAST( gpl.net_price AS CHAR ), '' ) net_price,
            pur2.meaning po_line_status,
            gpl.tax_included_amount,
            gpl.net_amount,
            purt.meaning deduct_flag_meaning,
            pur1.meaning settlement_method,
            DATE_FORMAT( gpl.need_date, '%Y-%m-%d' ) AS line_need_date,
            gpl.remark
        FROM
            gscm_po_line gpl
                LEFT JOIN hzero_platform.hpfm_inv_organization hio ON gpl.organization_id = hio.organization_id
                LEFT JOIN hzero_platform.hpfm_uom hu ON gpl.uom_id = hu.uom_id
                LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id
                AND hut.lang = #{lang}

                AND hut.lang = #{lang}
                LEFT JOIN hzero_platform.hpfm_lov_value pur ON pur.lov_code = 'HPFM.FLAG'

                AND pur.`value` = gpl.deduct_flag
                LEFT JOIN hzero_platform.hpfm_lov_value_tl purt ON purt.lov_value_id = pur.lov_value_id
                AND purt.lang = #{lang}
                LEFT JOIN hzero_platform.hpfm_lov_value pur1 ON pur1.lov_code = 'GSCM.SETTLEMENT_METHOD'
                AND pur1.`value` = gpl.settlement_method
                LEFT JOIN hzero_platform.hpfm_lov_value_tl purt1 ON purt1.lov_value_id = pur1.lov_value_id
                AND purt1.lang = #{lang}
                LEFT JOIN hzero_platform.hpfm_lov_value pur2 ON pur2.lov_code = 'GSCM.PO_LINE_STATUS'
                AND pur2.`value` = gpl.po_line_status
                LEFT JOIN hzero_platform.hpfm_lov_value_tl purt2 ON purt2.lov_value_id = pur2.lov_value_id
                AND purt2.lang = #{lang}

        WHERE
            gpl.po_header_id = #{poHeaderId}

        ORDER BY
            gpl.line_num
    </select>
    <select id="getItemTypePoAmountData" parameterType="com.gdh.scm.api.dto.PoAmountExportDTO" resultType="com.gdh.scm.api.dto.PoAmountExportDTO">
        select
        hou.ou_name,
        gph.supplier_name,
        gi.category_segment1,
        gi.category_segment2,
        gpl.tax_included_amount as tax_include_amount
        from gscm_po_line gpl
        inner join gscm_po_header gph on gpl.po_header_id = gph.po_header_id
        -- 外部po
        inner join gdh_scm.gscm_po_header gph_external
            on gph.relation_po_header_id = gph_external.po_header_id
            and gph_external.unified_sign_flag = 1
            and gph_external.po_category = 'EXTERNAL'
        inner join gdh_mdm.gmdm_item gi on gpl.item_id = gi.item_id
        inner join hzero_platform.hpfm_operation_unit hou on gph.ou_id = hou.ou_id
        left join gdh_mdm.gmdm_supplier gs on gph.supplier_id = gs.supplier_id
        left join hzero_platform.hpfm_company hc on gs.taxpayer_identification_number = hc.unified_social_code
        left join hzero_platform.hpfm_operation_unit hou2 on hc.company_id = hou2.company_id
        where
        gph.source_code = 'OUTSIDE_PO'
        and gph.unified_sign_flag = 1
        and gph.po_category = 'INTERNAL'
        and gpl.po_line_status != 'CANCELED'
        <if test="ouId != null">
            and gph.ou_id = #{ouId}
        </if>
        <if test="centralizedAcquisitionCenter != null">
            and gph.centralized_acquisition_center = #{centralizedAcquisitionCenter}
        </if>
        <if test="agentId != null">
            and gph_external.agent_id = #{agentId}
        </if>
        <if test="supplierOuId != null">
            and hou2.ou_id = #{supplierOuId}
        </if>
        <if test="creationDateFrom != null">
            and cast(gph.creation_date as Date) &gt;= cast(#{creationDateFrom} as Date)
        </if>
        <if test="creationDateTo != null">
            and cast(gph.creation_date as Date) &lt;= cast(#{creationDateTo} as Date)
        </if>
        <if test="categorySegment1 != null">
            and gi.category_segment1 = #{categorySegment1}
        </if>
        <if test="categorySegment2 != null">
            and gi.category_segment2 = #{categorySegment2}
        </if>
        order by gi.category_segment1,gi.category_segment2
    </select>

    <select id="purchaseCategoryDetail" parameterType="com.gdh.scm.api.dto.PurchaseCategoryReportDTO" resultType="com.gdh.scm.api.dto.PurchaseCategoryReportDTO">
        <bind name="ouAuthority" value="@com.gdh.scm.infra.helper.UserAuthorityHelper@userAuthority('OU')"/>
            select gph.ou_id,
                   gagh.group_name      as               area_name,
                   hou.ou_code,
                   hou.ou_name,
                   round(sum(gpl.tax_included_amount) / 10000, 2) amount,
                   gi.category_segment1 as               category_code
            from gdh_scm.gscm_po_header gph
                     join gdh_scm.gscm_po_line gpl on gph.po_header_id = gpl.po_header_id
                     join hzero_platform.hpfm_operation_unit hou on hou.ou_id = gph.ou_id
                     join gdh_mdm.gmdm_item gi on gpl.item_id = gi.item_id
                     join gdh_mdm.gmdm_organization_item goi on gi.item_id = goi.item_id and gpl.organization_id = goi.organization_id
                     left join gdh_mdm.gmdm_area_group_line gagl on gagl.ou_id = gph.ou_id
                     left join gdh_mdm.gmdm_area_group_header gagh on gagh.group_header_id = gagl.group_header_id
            where gph.po_status in ('APPROVED', 'CLOSED')
              and gpl.po_line_status != 'CANCELED'
        <if test="ouAuthority.includeAllFlag != true">
            AND gph.ou_id IN
            <foreach collection="ouAuthority.dataIds" item="dataId" open="(" close=")" separator=",">
                #{dataId}
            </foreach>
        </if>
        <if test="ouId != null">
            AND gph.ou_id = #{ouId}
        </if>
        <if test="purchaseDateFrom != null">
            AND gph.po_creation_date <![CDATA[>=]]> #{purchaseDateFrom}
        </if>
        <if test="purchaseDateTo != null">
            AND gph.po_creation_date <![CDATA[<=]]> #{purchaseDateTo}
        </if>
        <if test="centralized != null and centralized == 1">
            AND goi.centralized_purchasing_flag = '1'
        </if>
        group by gph.ou_id,
                 hou.ou_code,
                 hou.ou_name,
                 gi.category_segment1
    </select>
    <select id="getTrialInfo" resultType="com.gdh.scm.domain.vo.UnitReceivableTrialVO">
        select
            gph.ou_id,
            hou.ou_name,
            gph.requisition_ou_id,
            hou2.ou_name requisition_ou_name,
            gph.supplier_id,
            gph.supplier_name,
            gph.supplier_site_id,
            gph.supplier_site_name,
            gph.po_num,
            gph.centralized_acquisition_center,
            gpl.po_line_id,
            gpl.line_num,
            gpl.item_id,
            gpl.item_number,
            gpl.item_description,
            gpl.specifications,
            gpl.tax_included_price,
            gi.category_segment3,
            hu.uom_name
        FROM
            gscm_po_line gpl
                LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id
                LEFT JOIN gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id
                LEFT JOIN hzero_platform.hpfm_uom hu on gpl.uom_id =hu.uom_id
                LEFT JOIN hzero_platform.hpfm_operation_unit hou on hou.ou_id = gph.ou_id
                LEFT JOIN hzero_platform.hpfm_operation_unit hou2 ON hou2.ou_id = gph.requisition_ou_id
        <where>
            gph.po_category = 'EXTERNAL'
            AND gph.po_status IN ('INCOMPLETE', 'IN_PROCESS', 'REJECTED')
            AND gph.unified_sign_flag = 1
            <if test="ouId != null">
                AND gph.ou_id = #{ouId}
            </if>
            <if test="requisitionOuId != null">
                AND gph.requisition_ou_id =  #{requisitionOuId}
            </if>
            <if test="supplierId != null ">
                AND  gph.supplier_id = #{supplierId}
            </if>
            <if test="agentId != null ">
                AND  gph.agent_id = #{agentId}
            </if>
            <if test="centralizedAcquisitionCenter != null">
                <choose>
                    <when test="centralizedAcquisitionCenter == 'empty'">
                        AND gph.centralized_acquisition_center is null
                    </when>
                    <otherwise>
                        AND gph.centralized_acquisition_center = #{centralizedAcquisitionCenter}
                    </otherwise>
                </choose>

            </if>
            <if test="poNum != null and poNum != '' ">
                <bind name = "poNumLike" value = "'%' + poNum + '%'"/>
                AND  gph.po_num LIKE #{poNumLike}
            </if>
            <if test="poCreationDateFrom != null">
                <bind name = "tempPoCreationDateFrom" value = "poCreationDateFrom + ''"/>
                AND gph.po_creation_date  <![CDATA[>=]]>  #{tempPoCreationDateFrom}
            </if>
            <if test="poCreationDateEnd != null ">
                <bind name = "tempPoCreationDateEnd" value = "poCreationDateEnd + ''"/>
                AND gph.po_creation_date  <![CDATA[<=]]> #{tempPoCreationDateEnd}
            </if>
            <if test="departmentId != null ">
                AND gph.department_id  = #{departmentId}
            </if>
        </where>
        ORDER BY gph.po_num, gph.ou_id
    </select>
    <select id="selectNotCreateRcv" resultType="com.gdh.scm.domain.entity.PoLine">
        SELECT gph.po_header_id,
               gph.po_num,
               gph.source_code,
               heu1.user_id receipt_id,
               heu.user_id agent_id,
               TIMESTAMPDIFF(DAY,gpl.need_date,NOW()) delay_day,
               gpl.line_num
        FROM
            gscm_po_line gpl
            LEFT JOIN gscm_po_header gph on gph.po_header_id = gpl.po_header_id
            LEFT JOIN hzero_platform.hpfm_employee he on gph.agent_id = he.employee_id
            LEFT JOIN hzero_platform.hpfm_employee_user heu on he.employee_id = heu.employee_id
            LEFT JOIN hzero_platform.hpfm_employee he1 on gph.receipt_id = he1.employee_id
            LEFT JOIN hzero_platform.hpfm_employee_user heu1 on he1.employee_id = heu1.employee_id
        WHERE  gph.po_status = 'APPROVED'
          and gpl.receive_flag = 1
          and gpl.quantity_received &lt; gpl.quantity
          AND TIMESTAMPDIFF( DAY, gpl.need_date, NOW()) >= #{deferredDay}
    </select>
</mapper>
```

#### Output:

```json
{
"translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace="com.gdh.scm.infra.mapper.PoLineMapper">\n    <!-- Can be used according to your needs -->\n    <resultMap id="BaseResultMap" type="com.gdh.scm.domain.entity.PoLine">\n        <result column="po_line_id" property="poLineId" jdbcType="DECIMAL"/>\n        <result column="po_header_id" property="poHeaderId" jdbcType="DECIMAL"/>\n        <result column="line_num" property="lineNum" jdbcType="DECIMAL"/>\n        <result column="organization_id" property="organizationId" jdbcType="DECIMAL"/>\n        <result column="inventory_id" property="inventoryId" jdbcType="DECIMAL"/>\n        <result column="item_id" property="itemId" jdbcType="DECIMAL"/>\n        <result column="item_number" property="itemNumber" jdbcType="VARCHAR"/>\n        <result column="item_description" property="itemDescription" jdbcType="VARCHAR"/>\n        <result column="specifications" property="specifications" jdbcType="VARCHAR"/>\n        <result column="uom_id" property="uomId" jdbcType="DECIMAL"/>\n        <result column="quantity" property="quantity" jdbcType="DECIMAL"/>\n        <result column="shortage_percentage" property="shortagePercentage" jdbcType="DECIMAL"/>\n        <result column="excess_percentage" property="excessPercentage" jdbcType="DECIMAL"/>\n        <result column="tax_id" property="taxId" jdbcType="DECIMAL"/>\n        <result column="tax_rate" property="taxRate" jdbcType="DECIMAL"/>\n        <result column="net_price" property="netPrice" jdbcType="DECIMAL"/>\n        <result column="tax_included_price" property="taxIncludedPrice" jdbcType="DECIMAL"/>\n        <result column="net_amount" property="netAmount" jdbcType="DECIMAL"/>\n        <result column="tax_included_amount" property="taxIncludedAmount" jdbcType="DECIMAL"/>\n        <result column="tax_amount" property="taxAmount" jdbcType="DECIMAL"/>\n        <result column="need_date" property="needDate" jdbcType="DATE"/>\n        <result column="remark" property="remark" jdbcType="VARCHAR"/>\n        <result column="asset_name" property="assetName" jdbcType="VARCHAR"/>\n        <result column="asset_specifications" property="assetSpecifications" jdbcType="VARCHAR"/>\n        <result column="quantity_received" property="quantityReceived" jdbcType="DECIMAL"/>\n        <result column="quantity_delivered" property="quantityDelivered" jdbcType="DECIMAL"/>\n        <result column="receive_flag" property="receiveFlag" jdbcType="DECIMAL"/>\n        <result column="po_line_status" property="poLineStatus" jdbcType="VARCHAR"/>\n        <result column="demand_detail_flag" property="demandDetailFlag" jdbcType="DECIMAL"/>\n        <result column="external_system_code" property="externalSystemCode" jdbcType="VARCHAR"/>\n        <result column="source_code" property="sourceCode" jdbcType="VARCHAR"/>\n        <result column="tenant_id" property="tenantId" jdbcType="DECIMAL"/>\n        <result column="object_version_number" property="objectVersionNumber" jdbcType="DECIMAL"/>\n        <result column="creation_date" property="creationDate" jdbcType="DATE"/>\n        <result column="created_by" property="createdBy" jdbcType="DECIMAL"/>\n        <result column="last_updated_by" property="lastUpdatedBy" jdbcType="DECIMAL"/>\n        <result column="last_update_date" property="lastUpdateDate" jdbcType="DATE"/>\n        <result column="ATTRIBUTE_CATEGORY" property="attributeCategory" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE1" property="attribute1" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE2" property="attribute2" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE3" property="attribute3" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE4" property="attribute4" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE5" property="attribute5" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE6" property="attribute6" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE7" property="attribute7" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE8" property="attribute8" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE9" property="attribute9" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE10" property="attribute10" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE11" property="attribute11" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE12" property="attribute12" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE13" property="attribute13" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE14" property="attribute14" jdbcType="VARCHAR"/>\n        <result column="ATTRIBUTE15" property="attribute15" jdbcType="VARCHAR"/>\n        <result column="purchase_category" property="purchaseCategory" jdbcType="VARCHAR"/>\n        <result column="po_remark" property="poRemark" jdbcType="VARCHAR"/>\n        <result column="settlement_method" property="settlementMethod" jdbcType="VARCHAR"/>\n        <result column="source_line_id" property="sourceLineId" jdbcType="DECIMAL"/>\n        <!-- Non-database fields -->\n        <result column="organization_name" property="organizationName" jdbcType="VARCHAR"/>\n        <result column="uom_name" property="uomName" jdbcType="VARCHAR"/>\n        <result column="ou_id" property="ouId" jdbcType="DECIMAL"/>\n        <result column="supplier_id" property="supplierId" jdbcType="DECIMAL"/>\n        <result column="inventory_name" property="inventoryName" jdbcType="VARCHAR"/>\n        <result column="header_source_code" property="headerSourceCode" jdbcType="VARCHAR"/>\n        <result column="category_segment3" property="categorySegment3" jdbcType="VARCHAR"/>\n        <result column="requisition_ou_id" property="requisitionOuId" jdbcType="VARCHAR"/>\n        <result column="centralized_acquisition_center" property="centralizedAcquisitionCenter" jdbcType="VARCHAR"/>\n    </resultMap>\n\n    <sql id="basic_column">\n        <!--@sql select -->\n        gpl.po_line_id,\n        gpl.po_header_id,\n        gpl.line_num,\n        gpl.organization_id,\n        gpl.inventory_id,\n        gpl.item_id,\n        gpl.item_number,\n        gpl.item_description,\n        gpl.specifications,\n        gpl.uom_id,\n        gpl.quantity,\n        gpl.shortage_percentage,\n        gpl.excess_percentage,\n        gpl.tax_id,\n        gpl.tax_rate,\n        gpl.net_price,\n        gpl.tax_included_price,\n        gpl.net_amount,\n        gpl.tax_included_amount,\n        gpl.tax_amount,\n        gpl.need_date,\n        gpl.remark,\n        gpl.quantity_received,\n        gpl.quantity_delivered,\n        gpl.receive_flag,\n        gpl.po_line_status,\n        gpl.external_system_code,\n        gpl.source_code,\n        gpl.project_id,\n        gpl.tenant_id,\n        gpl.object_version_number,\n        gpl.creation_date,\n        gpl.created_by,\n        gpl.last_updated_by,\n        gpl.last_update_date,\n        gpl.ATTRIBUTE_CATEGORY,\n        gpl.ATTRIBUTE1,\n        gpl.ATTRIBUTE2,\n        gpl.ATTRIBUTE3,\n        gpl.ATTRIBUTE4,\n        gpl.ATTRIBUTE5,\n        gpl.ATTRIBUTE6,\n        gpl.ATTRIBUTE7,\n        gpl.ATTRIBUTE8,\n        gpl.ATTRIBUTE9,\n        gpl.ATTRIBUTE10,\n        gpl.ATTRIBUTE11,\n        gpl.ATTRIBUTE12,\n        gpl.ATTRIBUTE13,\n        gpl.ATTRIBUTE14,\n        gpl.ATTRIBUTE15,\n        gpl.purchase_category,\n        gpl.asset_name,\n        gpl.asset_specifications,\n        gpl.deduct_flag,\n        gpl.source_header_id,\n        gpl.source_line_id,\n        gpl.settlement_method\n        <!--@sql from gscm_po_line gpl-->\n    </sql>\n\n    <select id="listPoLine" resultType="com.gdh.scm.domain.entity.PoLine">\n        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>\n        SELECT <include refid="basic_column"/> ,\n               hio.organization_name,\n               gph.source_code header_source_code,\n               hi.inventory_name,\n               hut.uom_name,\n               case\n                   when (select count(pll.line_location_id)\n                         from gscm_po_line_location pll\n                         where gpl.po_line_id = pll.po_line_id) >\n                        0\n                       then 1\n                   else 0 end  demandDetailFlag,\n               CASE\n                   WHEN gph.source_code = 'INNER_QO' THEN (SELECT giqh.inner_quotation_num\n                                                           from gscm_inner_quotation_header giqh\n                                                           where giqh.inner_quotation_header_id = gpl.source_header_id)\n                   WHEN gph.source_code = 'OUTSIDE_PO' THEN (SELECT gph2.po_num\n                                                             from gscm_po_header gph2\n                                                             where gph2.po_header_id =\n                                                                   gpl.source_header_id)\n                   WHEN gph.source_code = 'SALES_ORDER' THEN (SELECT gsh.so_num\n                                                              from gscm_so_header gsh\n                                                              where gsh.so_header_id =\n                                                                    gpl.source_header_id)\n                   ELSE NULL\n                   END         source_num,\n               CASE\n                   WHEN gph.source_code = 'OUTSIDE_PO' THEN (SELECT gpl2.line_num\n                                                             from gscm_po_line gpl2\n                                                             where gpl2.po_line_id =\n                                                                   gpl.source_line_id)\n                   WHEN gph.source_code = 'SALES_ORDER' THEN (SELECT gsl.line_num\n                                                              from gscm_so_line gsl\n                                                              where gsl.so_line_id =\n                                                                    gpl.source_line_id)\n                   ELSE NULL\n                   END         source_line_num,\n               gp.project_num,\n               gp.project_name\n        FROM gscm_po_line gpl\n                 LEFT JOIN gdh_mdm.gmdm_project gp on gp.project_id = gpl.project_id\n                 LEFT JOIN hpfm_inv_organization hio\n                           ON gpl.organization_id = hio.organization_id\n                 LEFT JOIN gscm_po_header gph ON gph.po_header_id = gpl.po_header_id\n                 LEFT JOIN hpfm_inventory hi\n                           ON gpl.inventory_id = hi.inventory_id\n                 LEFT JOIN hpfm_uom hu\n                           ON gpl.uom_id = hu.uom_id\n                 LEFT JOIN hpfm_uom_tl hut\n                           ON hu.uom_id = hut.uom_id\n                               AND hut.lang = #{lang}\n        WHERE gpl.po_header_id = #{poHeaderId}\n    </select>\n\n    <select id="listPoLineByShipment" resultMap="BaseResultMap">\n        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>\n        <bind name="userDetails" value="@io.choerodon.core.oauth.DetailsHelper@getUserDetails()"/>\n        <bind name="employee"\n              value="@org.hzero.boot.platform.plugin.hr.EmployeeHelper@getEmployee(userDetails.userId,userDetails.tenantId)"/>\n        <bind name="ouAuthority" value="@com.gdh.scm.infra.helper.UserAuthorityHelper@userAuthority('OU')"/>\n        SELECT\n        <include refid="basic_column"/>,\n        gi.category_segment3,\n        hio.organization_name,\n        hi.inventory_name,\n        hut.uom_name,\n        gph.po_num,\n        gph.supplier_id,\n        gph.supplier_site_id,\n        hou.ou_code,\n        gph.ou_id,\n        gph.remark po_remark,\n        gph.po_category,\n        gph.unified_sign_flag,\n        gph.source_code header_source_code,\n        gph.requisition_ou_id,\n        gph.centralized_acquisition_center,\n        hunit.unit_name departmentName,\n        (CASE WHEN (select count(pll.line_location_id)\n                    from gscm_po_line_location pll\n                    where gpl.po_line_id = pll.po_line_id) > 0\n        THEN 1 ELSE 0 END) demand_detail_flag\n        FROM gscm_po_line gpl\n        LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n        LEFT JOIN hzero_platform.hpfm_unit hunit on gph.department_id = hunit.unit_id\n        LEFT JOIN hzero_platform.hpfm_inv_organization hio ON gpl.organization_id = hio.organization_id\n        LEFT JOIN hzero_platform.hpfm_operation_unit hou on hou.ou_id = hio.ou_id\n        LEFT JOIN hzero_platform.hpfm_inventory hi ON gpl.inventory_id = hi.inventory_id\n        LEFT JOIN hzero_platform.hpfm_uom hu ON gpl.uom_id = hu.uom_id\n        LEFT JOIn gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id\n        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}\n        <where>\n            gpl.tenant_id = #{tenantId}\n            and gpl.receive_flag = 1\n            and gph.po_status = 'APPROVED'\n            AND gpl.organization_id = #{organizationId}\n            AND (gph.ou_id = #{ouId}\n            AND gph.supplier_id = #{supplierId}\n            AND gph.supplier_site_id = #{supplierSiteId}\n            AND gpl.purchase_category = #{purchaseCategory}\n            and (( 1=1\n            <if test="ouAuthority.includeAllFlag != true">\n                AND gph.ou_id IN\n                <foreach collection="ouAuthority.dataIds" item="dataId" open="(" close=")" separator=",">\n                    #{dataId}\n                </foreach>\n            </if>) \n            <if test="directlySendFlag != null and directlySendFlag == 1">\n                <bind name="employeeIdLike" value="'%'+employee.employeeId+'%'"/>\n                OR gph.receipt_id LIKE #{employeeIdLike}\n            </if>) \n            )\n            <if test="directlySendFlag != null and directlySendFlag == 1">\n                AND gph.source_code = 'SALES_ORDER'\n            </if>\n            <if test="directlySendFlag == null or directlySendFlag != 1">\n                AND ((gph.unified_sign_flag = 0 AND gph.po_category = 'EXTERNAL')\n                         AND gph.source_code != 'SALES_ORDER'\n                    OR (gph.unified_sign_flag = 1 AND gph.po_category = 'INTERNAL'))\n            </if>\n            <if test="poNum != null and poNum != '' ">\n                <bind name="poNumLike" value="'%'+ poNum  + '%'"/>\n                AND gph.po_num like #{poNumLike}\n            </if>\n            <if test="itemNumber != null and itemNumber != '' ">\n                <bind name="itemNumberLike" value="'%'+ itemNumber  + '%'"/>\n                AND gpl.item_number like #{itemNumberLike}\n            </if>\n            <if test="assetName != null and assetName != '' ">\n                <bind name="assetNameLike" value="'%'+ assetName  + '%'"/>\n                AND gpl.asset_name like #{assetNameLike}\n            </if>\n            <if test="assetSpecifications != null and assetSpecifications != '' ">\n                <bind name="assetSpecificationsLike" value="'%'+ assetSpecifications  + '%'"/>\n                AND gpl.asset_specifications like #{assetSpecificationsLike}\n            </if>\n            <if test="itemDescription != null and itemDescription != '' ">\n                <bind name="itemDescriptionLike" value="'%'+ itemDescription  + '%'"/>\n                AND gpl.item_description like #{itemDescriptionLike}\n            </if>\n            <if test="departmentId != null ">\n                AND gph.department_id = #{departmentId}\n            </if>\n            <if test="requestorId != null ">\n                AND gph.agent_id = #{requestorId}\n            </if>\n            <if test="requisitionOuId != null ">\n                AND gph.requisition_ou_id = #{requisitionOuId}\n            </if>\n            <if test="inventoryId != null ">\n                AND hi.inventory_id = #{inventoryId}\n            </if>\n            <if test="creationDate != null ">\n                AND date_format(gpl.creation_date, '%Y-%m-%d') = date_format(#{creationDate}, '%Y-%m-%d')\n            </if>\n            <if test="poRemark!=null and poRemark!=''">\n                <bind name="poRemarkLike" value="'%' + poRemark + '%'"/>\n                AND gph.remark LIKE #{poRemarkLike}\n            </if>\n            <if test="relationPoNum != null">\n                <bind name="relationPoNumLike" value="'%'+ relationPoNum +'%'"/>\n                AND EXISTS(\n                SELECT 1 FROM gscm_po_header gph2 WHERE gph2.po_header_id=gph.relation_po_header_id\n                AND gph2.po_num LIKE #{relationPoNumLike}\n                )\n            </if>\n            <if test="projectId">\n                AND gpl.project_id = #{projectId}\n            </if>\n            <if test="requisitionNum != null">\n                <bind name="requisitionNumLike" value="'%'+requisitionNum +'%'"/>\n                AND EXISTS (SELECT 1\n                FROM gscm_po_line_location gpll\n                JOIN gscm_po_requisition_header gprh\n                ON gpll.requisition_header_id = gprh.requisition_header_id\n                WHERE gpll.po_line_id = gpl.po_line_id\n                AND gprh.requisition_num like #{requisitionNumLike})\n            </if>\n        </where>\n        order by gph.po_num, gpl.line_num\n    </select>\n\n    <select id="getCurrentMaxLineNumNext" resultType="java.lang.Long">\n        SELECT IFNULL(MAX(gpl.line_num), 0) + 1\n        FROM gscm_po_line gpl\n        WHERE gpl.po_header_id = #{poHeaderId}\n    </select>\n\n    <select id="sumAmount" resultMap="BaseResultMap">\n        SELECT IFNULL(SUM(gpl.net_amount), 0)          AS net_amount,\n               IFNULL(SUM(gpl.tax_included_amount), 0) AS tax_included_amount\n        FROM gscm_po_line gpl\n        WHERE gpl.po_header_id = #{poHeaderId}\n    </select>\n\n    <select id="getRecentPoLine" resultMap="BaseResultMap">\n        SELECT gpl.\n        FROM gscm_po_line gpl,\n             gscm_po_header gph\n        WHERE gpl.po_header_id = gph.po_header_id\n          AND gph.tenant_id = #{tenantId}\n          AND gph.ou_id = #{ouId}\n          AND gph.supplier_id = #{supplierId}\n          AND gpl.item_id = #{itemId}\n          AND gph.po_status = 'APPROVED'\n        ORDER BY gph.po_header_id DESC, gpl.po_line_id DESC LIMIT 1\n    </select>\n\n    <select id="queryLineByPoNumAndLineNum" resultMap="BaseResultMap">\n        SELECT gpl.\n        FROM gscm_po_line gpl\n                 LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n        WHERE gph.po_num = #{poNum}\n          AND gpl.line_num = #{lineNum}\n          AND gpl.tenant_id = #{tenantId}\n    </select>\n\n    <select id="executionReport" resultType="com.gdh.scm.domain.vo.PoLineVO">\n        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>\n        SELECT hou.ou_name,\n        hio.organization_name,\n        gph.supplier_num,\n        gph.supplier_name,\n        gph.supplier_site_name,\n        gpl.purchase_category,\n        he.name agentor,\n        gph.po_num,\n        gph.po_status,\n        gph.po_creation_date,\n        gpl.line_num,\n        gpl.item_id,\n        gpl.item_number,\n        gpl.item_description,\n        gpl.specifications,\n        gpl.quantity,\n        gpl.settlement_method,\n        hut.uom_name,\n        gpl.tax_included_amount,\n        gpl.tax_rate,\n        gpl.po_line_id,\n        gpl.tax_included_price,\n        gi.uom_id item_uom_id,\n        gpl.uom_id,\n        gpl.tenant_id,\n        item_hut.uom_name item_uom_name,\n        grsl.line_num shipment_line_num,\n        grsl.tax_included_price shipment_tax_included_price,\n        grsl.shipment_line_id,\n        grsh.shipment_num,\n        gph.contract_num,\n        gph.contract_name\n        FROM gscm_po_line gpl\n        JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n        JOIN gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id\n        JOIN hzero_platform.hpfm_uom item_hu ON item_hu.uom_id = gi.uom_id\n        LEFT JOIN hzero_platform.hpfm_uom_tl item_hut\n        ON item_hut.uom_id = item_hu.uom_id AND item_hut.lang = #{lang}\n        JOIN hzero_platform.hpfm_operation_unit hou ON hou.ou_id = gph.ou_id\n        LEFT JOIN hzero_platform.hpfm_inv_organization hio ON hio.organization_id = gpl.organization_id\n        LEFT JOIN hzero_platform.hpfm_employee he ON he.employee_id = gph.agent_id\n        JOIN hzero_platform.hpfm_uom hu ON hu.uom_id = gpl.uom_id\n        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}\n        LEFT JOIN gscm_rcv_shipment_line grsl ON gpl.po_line_id = grsl.po_line_id\n        LEFT JOIN gscm_rcv_shipment_header grsh ON grsh.shipment_header_id = grsl.shipment_header_id\n        WHERE gph.ou_id = #{ouId}\n        AND gpl.organization_id = #{organizationId}\n        AND (gph.po_status = 'APPROVED' OR gph.po_status = 'CLOSED')\n        AND (grsh.shipment_header_id IS NULL OR grsh.document_type = 'INSPECTION')\n        <if test="inventoryId != null">\n            AND gpl.inventory_id = #{inventoryId}\n        </if>\n        <if test="agentId != null">\n            AND gph.agent_id = #{agentId}\n        </if>\n        <if test="poNum != null and poNum != ''">\n            <bind name="poNumLike" value="'%' + poNum + '%'"/>\n            AND gph.po_num LIKE #{poNumLike}\n        </if>\n        <if test="supplierId != null">\n            AND gph.supplier_id = #{supplierId}\n        </if>\n        <if test="contractNum != null and contractNum != ''">\n            <bind name="contractNumLike" value="'%' + contractNum + '%'"/>\n            AND gph.contract_num LIKE #{contractNumLike}\n        </if>\n        <if test="itemNumberFrom != null and itemNumberFrom != ''">\n            AND gpl.item_number >= #{itemNumberFrom}\n        </if>\n        <if test="itemNumberTo != null and itemNumberTo != ''">\n            AND gpl.item_number <= #{itemNumberTo}\n        </if>\n        <if test="operationDateFrom != null">\n            AND gph.po_creation_date >= #{operationDateFrom}\n        </if>\n        <if test="operationDateTo != null">\n            AND gph.po_creation_date <= #{operationDateTo}\n        </if>\n        <if test="departmentId != null">\n            AND EXISTS(SELECT 1\n            FROM hzero_platform.hpfm_employee_assign hea\n            WHERE hea.employee_id = gph.agent_id\n            AND hea.enabled_flag = 1\n            AND hea.unit_id = #{departmentId}\n            )\n        </if>\n        ORDER BY gph.supplier_num, gph.po_num, gpl.line_num\n    </select>\n\n    <select id="executionReport2" resultType="com.gdh.scm.domain.vo.PoLineVO">\n        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>\n        SELECT hou.ou_name,\n        hio.organization_name,\n        gph.supplier_name platform_company_name,\n        gph2.supplier_num,\n        gph2.supplier_name,\n        gph.supplier_site_name,\n        gpl.purchase_category,\n        he.name agentor,\n        gph.po_num,\n        gph.po_status,\n        gph.po_creation_date,\n        gpl.line_num,\n        gpl.item_id,\n        gpl.item_number,\n        gpl.item_description,\n        gpl.specifications,\n        gpl.quantity,\n        gpl.settlement_method,\n        hut.uom_name,\n        gpl.tax_included_amount,\n        gpl.tax_rate,\n        gpl.po_line_id,\n        gpl.tax_included_price,\n        gi.uom_id item_uom_id,\n        gpl.uom_id,\n        gpl.tenant_id,\n        item_hut.uom_name item_uom_name,\n        grsl.line_num shipment_line_num,\n        grsl.tax_included_price shipment_tax_included_price,\n        grsl.shipment_line_id,\n        grsh.shipment_num\n        FROM gscm_po_line gpl\n        JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n        LEFT JOIN gscm_po_header gph2 ON gph.relation_po_header_id = gph2.po_header_id\n        LEFT JOIN (select distinct ou_name<if test="platformCompanyId != null">, ou_id</if>\n                   from hzero_platform.hpfm_operation_unit<if test="platformCompanyId != null"> where ou_id = #{platformCompanyId}</if>) hou2\n            ON hou2.ou_name = gph.supplier_name\n        JOIN gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id\n        JOIN hzero_platform.hpfm_uom item_hu ON item_hu.uom_id = gi.uom_id\n        LEFT JOIN hzero_platform.hpfm_uom_tl item_hut\n        ON item_hut.uom_id = item_hu.uom_id AND item_hut.lang = #{lang}\n        JOIN hzero_platform.hpfm_operation_unit hou ON hou.ou_id = gph.ou_id\n        LEFT JOIN hzero_platform.hpfm_inv_organization hio ON hio.organization_id = gpl.organization_id\n        LEFT JOIN hzero_platform.hpfm_employee he ON he.employee_id = gph.agent_id\n        JOIN hzero_platform.hpfm_uom hu ON hu.uom_id = gpl.uom_id\n        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}\n        LEFT JOIN gscm_rcv_shipment_line grsl ON gpl.po_line_id = grsl.po_line_id\n        LEFT JOIN gscm_rcv_shipment_header grsh ON grsh.shipment_header_id = grsl.shipment_header_id\n        WHERE gph.ou_id = #{ouId} AND gph.source_code = 'OUTSIDE_PO'\n        AND gpl.organization_id = #{organizationId}\n        AND (gph.po_status = 'APPROVED' OR gph.po_status = 'CLOSED')\n        AND (grsh.shipment_header_id IS NULL OR grsh.document_type = 'INSPECTION')\n        <if test="agentId != null">\n            AND gph.agent_id = #{agentId}\n        </if>\n        <if test="platformCompanyId != null">\n            AND hou2.ou_id = #{platformCompanyId}\n        </if>\n        <if test="poNum != null and poNum != ''">\n            <bind name="poNumLike" value="'%' + poNum + '%'"/>\n            AND gph.po_num LIKE #{poNumLike}\n        </if>\n        <if test="supplierId != null">\n            AND gph2.supplier_id = #{supplierId}\n        </if>\n        <if test="contractNum != null and contractNum != ''">\n            <bind name="contractNumLike" value="'%' + contractNum + '%'"/>\n            AND gph.contract_num LIKE #{contractNumLike}\n        </if>\n        <if test="itemNumberFrom != null and itemNumberFrom != ''">\n            AND gpl.item_number >= #{itemNumberFrom}\n        </if>\n        <if test="itemNumberTo != null and itemNumberTo != ''">\n            AND gpl.item_number <= #{itemNumberTo}\n        </if>\n        <if test="operationDateFrom != null">\n            AND gph.po_creation_date >= #{operationDateFrom}\n        </if>\n        <if test="operationDateTo != null">\n            AND gph.po_creation_date <= #{operationDateTo}\n        </if>\n        <if test="departmentId != null">\n            AND EXISTS(SELECT 1\n            FROM hzero_platform.hpfm_employee_assign hea\n            WHERE hea.employee_id = gph.agent_id\n            AND hea.enabled_flag = 1\n            AND hea.unit_id = #{departmentId}\n            )\n        </if>\n        ORDER BY gph.supplier_num, gph.po_num, gpl.line_num\n    </select>\n\n    <select id="queryPoLinesByItemAndUom" resultMap="BaseResultMap">\n        SELECT gpl.tax_included_price,\n               gpl.item_id,\n               gpl.uom_id,\n               gpl.po_line_id,\n               gpl.tax_id,\n               gpl.tax_rate\n        FROM gscm_po_line gpl\n                 JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n        WHERE gph.po_status = 'APPROVED'\n          AND gpl.item_id = #{itemId}\n          AND gpl.uom_id = #{uomId}\n        ORDER BY gpl.po_line_id DESC LIMIT 1\n    </select>\n\n    <select id="queryRelateInnerPoLine" resultType="com.gdh.scm.domain.entity.PoLine">\n        SELECT * FROM gscm_po_line gpl\n        WHERE gpl.source_line_id IN\n        <foreach collection="poLineIds" open="(" close=")" separator="," item="poLineId">\n            #{poLineId}\n        </foreach>\n    </select>\n\n    <select id="selectOuterPoLines" resultType="com.gdh.scm.api.dto.StockPoLineDTO">\n        SELECT inner_gisl.stock_line_id      inner_stock_line_id,\n               out_grsl.shipment_header_id,\n               out_grsl.shipment_line_id,\n               inner_grsl.shipment_header_id inner_shipment_header_id,\n               inner_grsl.shipment_line_id   inner_shipment_line_id,\n               out_grsh.shipment_num,\n               out_grsl.line_num             shipment_line_num,\n               out_gph.po_num,\n               out_gpl.line_num              po_line_num,\n               out_gpl.organization_id,\n               out_gph.ou_id,\n               out_grsh.supplier_id,\n               out_grsh.supplier_site_id,\n               out_grsl.item_id,\n               goi.batch_flag,\n               out_gph.po_category,\n               out_gph.unified_sign_flag,\n               case out_gpl.deduct_flag when 1 then grt.net_price else grt.tax_included_price end transaction_cost\n        FROM gscm_po_line out_gpl,\n             gscm_po_header out_gph,\n             gscm_rcv_shipment_line out_grsl,\n             gscm_rcv_shipment_header out_grsh,\n             gscm_rcv_shipment_line inner_grsl,\n             gscm_po_line inner_gpl,\n             gscm_po_header inner_gph,\n             gscm_inv_stock_line inner_gisl,\n             gmdm_organization_item goi,\n             gscm_rcv_transaction grt\n        WHERE out_gpl.po_line_id = out_grsl.po_line_id\n          AND out_gpl.po_header_id = out_gph.po_header_id\n          AND out_grsl.shipment_header_id = out_grsh.shipment_header_id\n          AND out_grsl.orginal_shipment_line_id = inner_grsl.shipment_line_id\n          AND inner_gisl.source_document_line_id = inner_grsl.shipment_line_id\n          AND out_gpl.item_id = goi.item_id\n          AND goi.organization_id = out_gpl.organization_id\n          AND inner_gpl.po_line_id = inner_grsl.po_line_id\n          AND inner_gph.po_header_id = inner_gpl.po_header_id\n          AND inner_gph.unified_sign_flag = 1\n          AND inner_gph.po_category = 'INTERNAL'\n          AND (grt.shipment_line_id=out_grsl.shipment_line_id AND grt.return_flag=0)\n          AND inner_gisl.stock_line_id IN\n        <foreach collection="stockLineIds" open="(" close=")" separator="," item="item">\n            #{item}\n        </foreach>\n    </select>\n    <select id="getPoLineByPoLineId" resultType="com.gdh.scm.domain.entity.PoLine">\n        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>\n        <bind name="userDetails" value="@io.choerodon.core.oauth.DetailsHelper@getUserDetails()"/>\n        SELECT\n            <include refid="basic_column"/>,\n            gpl.source_line_id,\n            gi.category_segment3,\n            hio.organization_name,\n            hi.inventory_name,\n            hut.uom_name,\n            gph.po_num,\n            gph.supplier_id,\n            hou.ou_code,\n            gph.ou_id,\n            gph.remark po_remark,\n            gph.po_category,\n            gph.unified_sign_flag,\n            gph.supplier_site_id,\n            gph.source_code header_source_code,\n            gph.requisition_ou_id,\n            gph.centralized_acquisition_center\n        FROM gscm_po_line gpl\n        LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n        LEFT JOIN hpfm_inv_organization hio ON gpl.organization_id = hio.organization_id\n        LEFT JOIN hpfm_operation_unit hou on hou.ou_id = hio.ou_id\n        LEFT JOIN hpfm_inventory hi ON gpl.inventory_id = hi.inventory_id\n        LEFT JOIN hpfm_uom hu ON gpl.uom_id = hu.uom_id\n        LEFT JOIn gmdm_item gi ON gi.item_id = gpl.item_id\n        LEFT JOIN hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}\n        WHERE gpl.po_line_id = #{poLineId}\n    </select>\n    <select id="detailReport" resultType="com.gdh.scm.domain.vo.PoLineDetailReportVO">\n        <bind name="lang" value="@io.choerodon.mybatis.helper.LanguageHelper@language()"/>\n        SELECT hou.ou_name,\n               hou.ou_code,\n               gph.centralized_acquisition_center,\n               rhou.ou_name                requisition_ou_name,\n               gph.supplier_name,\n               gph.supplier_num,\n               gph.supplier_site_name,\n               gph.po_num,\n               he.name                     agentor,\n               group_concat(rhe.name)      receipt_name,\n               group_concat(rhe.mobile) as receipt_mobile,\n               gph.receipt_address,\n               gph.po_creation_date,\n               gpl.line_num,\n               gpl.item_id,\n               gpl.item_number,\n               gpl.item_description,\n               gpl.specifications,\n               gpl.uom_id,\n               gpl.tenant_id,\n               gpl.quantity,\n               hut.uom_name,\n               gpl.tax_included_amount,\n               gpl.po_line_id,\n               gpl.tax_included_price,\n               gpl.tax_rate,\n               gpl.need_date,\n               gpl.remark\n        FROM gscm_po_line gpl\n        LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n        JOIN hzero_platform.hpfm_operation_unit hou ON hou.ou_id = gph.ou_id\n        LEFT JOIN hzero_platform.hpfm_operation_unit rhou ON rhou.ou_id = gph.requisition_ou_id\n        LEFT JOIN hzero_platform.hpfm_employee he ON he.employee_id = gph.agent_id\n        LEFT JOIN hzero_platform.hpfm_employee rhe ON find_in_set(rhe.employee_id, gph.receipt_id)\n        JOIN hzero_platform.hpfm_uom hu ON hu.uom_id = gpl.uom_id\n        LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id AND hut.lang = #{lang}\n        WHERE\n        gph.unified_sign_flag = 1\n        AND gph.po_category = 'EXTERNAL'\n        AND gph.po_status = 'APPROVED'\n        <if test="ouId != null">\n            AND gph.ou_id = #{ouId}\n        </if>\n        <if test="supplierId != null">\n            AND gph.supplier_id = #{supplierId}\n        </if>\n        <if test="supplierSiteId != null">\n            AND gph.supplier_site_id = #{supplierSiteId}\n        </if>\n        <if test="poNum != null">\n            <bind name="poNumLike" value="'%' +poNum+ '%'"/>\n            AND gph.po_num LIKE #{poNumLike}\n        </if>\n        <if test="agentId != null">\n            AND gph.agent_id = #{agentId}\n        </if>\n        <if test="centralizedAcqCenter != null">\n            AND gph.centralized_acquisition_center = #{centralizedAcqCenter}\n        </if>\n        <if test="contractNum != null">\n            <bind name="contractNumLike" value="'%'+contractNum+'%'"/>\n            AND gph.contract_num LIKE #{contractNumLike}\n        </if>\n        <if test="operationDateFrom != null">\n            AND gph.po_creation_date >= #{operationDateFrom}\n        </if>\n        <if test="operationDateTo != null">\n            AND gph.po_creation_date <= #{operationDateTo}\n        </if>\n        <if test="requisitionOuId != null">\n            AND gph.requisition_ou_id = #{requisitionOuId}\n        </if>\n        GROUP BY hou.ou_name,\n                 hou.ou_code,\n                 gph.centralized_acquisition_center,\n                 rhou.ou_name,\n                 gph.supplier_name,\n                 gph.supplier_num,\n                 gph.supplier_site_name,\n                 gph.po_num,\n                 he.name,\n                 gph.receipt_address,\n                 gph.po_creation_date,\n                 gpl.line_num,\n                 gpl.item_id,\n                 gpl.item_number,\n                 gpl.item_description,\n                 gpl.specifications,\n                 gpl.uom_id,\n                 gpl.tenant_id,\n                 gpl.quantity,\n                 hut.uom_name,\n                 gpl.tax_included_amount,\n                 gpl.po_line_id,\n                 gpl.tax_included_price,\n                 gpl.tax_rate,\n                 gpl.need_date\n        ORDER BY hou.ou_code,gph.supplier_num,gph.po_num,gpl.line_num\n    </select>\n\n    <select id="queryPoLineReport" resultMap="BaseResultMap">\n        SELECT\n            gpl.line_num,\n            hio.organization_name,\n            gpl.item_number,\n            CASE\n\n                WHEN gpl.purchase_category = 'FIXED_ASSETS' THEN\n                    gpl.asset_name ELSE gpl.item_description\n                END item_description,\n            CASE\n\n                WHEN gpl.purchase_category = 'FIXED_ASSETS' THEN\n                    gpl.asset_specifications ELSE gpl.specifications\n                END specifications,\n            hut.uom_name,\n            CONCAT( 0 + CAST( gpl.quantity AS CHAR ), '' ) quantity,\n            CONCAT( 0 + CAST( gpl.shortage_percentage AS CHAR ), '' ) shortage_percentage,\n            CONCAT( 0 + CAST( gpl.excess_percentage AS CHAR ), '' ) excess_percentage,\n            gpl.tax_rate,\n            CONCAT( 0 + CAST( gpl.tax_included_price AS CHAR ), '' ) tax_included_price,\n            CONCAT( 0 + CAST( gpl.net_price AS CHAR ), '' ) net_price,\n            pur2.meaning po_line_status,\n            gpl.tax_included_amount,\n            gpl.net_amount,\n            purt.meaning deduct_flag_meaning,\n            pur1.meaning settlement_method,\n            DATE_FORMAT( gpl.need_date, '%Y-%m-%d' ) AS line_need_date,\n            gpl.remark\n        FROM\n            gscm_po_line gpl\n                LEFT JOIN hzero_platform.hpfm_inv_organization hio ON gpl.organization_id = hio.organization_id\n                LEFT JOIN hzero_platform.hpfm_uom hu ON gpl.uom_id = hu.uom_id\n                LEFT JOIN hzero_platform.hpfm_uom_tl hut ON hu.uom_id = hut.uom_id\n                AND hut.lang = #{lang}\n\n                AND hut.lang = #{lang}\n                LEFT JOIN hzero_platform.hpfm_lov_value pur ON pur.lov_code = 'HPFM.FLAG'\n\n                AND pur.value = gpl.deduct_flag\n                LEFT JOIN hzero_platform.hpfm_lov_value_tl purt ON purt.lov_value_id = pur.lov_value_id\n                AND purt.lang = #{lang}\n                LEFT JOIN hzero_platform.hpfm_lov_value pur1 ON pur1.lov_code = 'GSCM.SETTLEMENT_METHOD'\n                AND pur1.value = gpl.settlement_method\n                LEFT JOIN hzero_platform.hpfm_lov_value_tl purt1 ON purt1.lov_value_id = pur1.lov_value_id\n                AND purt1.lang = #{lang}\n                LEFT JOIN hzero_platform.hpfm_lov_value pur2 ON pur2.lov_code = 'GSCM.PO_LINE_STATUS'\n                AND pur2.value = gpl.po_line_status\n                LEFT JOIN hzero_platform.hpfm_lov_value_tl purt2 ON purt2.lov_value_id = pur2.lov_value_id\n                AND purt2.lang = #{lang}\n        WHERE\n            gpl.po_header_id = #{poHeaderId}\n\n        ORDER BY\n            gpl.line_num\n    </select>\n    <select id="getItemTypePoAmountData" parameterType="com.gdh.scm.api.dto.PoAmountExportDTO" resultType="com.gdh.scm.api.dto.PoAmountExportDTO">\n        select\n        hou.ou_name,\n        gph.supplier_name,\n        gi.category_segment1,\n        gi.category_segment2,\n        gpl.tax_included_amount as tax_include_amount\n        from gscm_po_line gpl\n        inner join gscm_po_header gph on gpl.po_header_id = gph.po_header_id\n        -- 外部po\n        inner join gdh_scm.gscm_po_header gph_external\n            on gph.relation_po_header_id = gph_external.po_header_id\n            and gph_external.unified_sign_flag = 1\n            and gph_external.po_category = 'EXTERNAL'\n        inner join gdh_mdm.gmdm_item gi on gpl.item_id = gi.item_id\n        inner join hzero_platform.hpfm_operation_unit hou on gph.ou_id = hou.ou_id\n        left join gdh_mdm.gmdm_supplier gs on gph.supplier_id = gs.supplier_id\n        left join hzero_platform.hpfm_company hc on gs.taxpayer_identification_number = hc.unified_social_code\n        left join hzero_platform.hpfm_operation_unit hou2 on hc.company_id = hou2.company_id\n        where\n        gph.source_code = 'OUTSIDE_PO'\n        and gph.unified_sign_flag = 1\n        and gph.po_category = 'INTERNAL'\n        and gpl.po_line_status != 'CANCELED'\n        <if test="ouId != null">\n            and gph.ou_id = #{ouId}\n        </if>\n        <if test="centralizedAcquisitionCenter != null">\n            and gph.centralized_acquisition_center = #{centralizedAcquisitionCenter}\n        </if>\n        <if test="agentId != null">\n            and gph_external.agent_id = #{agentId}\n        </if>\n        <if test="supplierOuId != null">\n            and hou2.ou_id = #{supplierOuId}\n        </if>\n        <if test="creationDateFrom != null">\n            and cast(gph.creation_date as Date) &gt;= cast(#{creationDateFrom} as Date)\n        </if>\n        <if test="creationDateTo != null">\n            and cast(gph.creation_date as Date) &lt;= cast(#{creationDateTo} as Date)\n        </if>\n        <if test="categorySegment1 != null">\n            and gi.category_segment1 = #{categorySegment1}\n        </if>\n        <if test="categorySegment2 != null">\n            and gi.category_segment2 = #{categorySegment2}\n        </if>\n        order by gi.category_segment1,gi.category_segment2\n    </select>\n\n    <select id="purchaseCategoryDetail" parameterType="com.gdh.scm.api.dto.PurchaseCategoryReportDTO" resultType="com.gdh.scm.api.dto.PurchaseCategoryReportDTO">\n        <bind name="ouAuthority" value="@com.gdh.scm.infra.helper.UserAuthorityHelper@userAuthority('OU')"/>\n            select gph.ou_id,\n                   gagh.group_name      as               area_name,\n                   hou.ou_code,\n                   hou.ou_name,\n                   round(sum(gpl.tax_included_amount) / 10000, 2) amount,\n                   gi.category_segment1 as               category_code\n            from gdh_scm.gscm_po_header gph\n                     join gdh_scm.gscm_po_line gpl on gph.po_header_id = gpl.po_header_id\n                     join hzero_platform.hpfm_operation_unit hou on hou.ou_id = gph.ou_id\n                     join gdh_mdm.gmdm_item gi on gpl.item_id = gi.item_id\n                     join gdh_mdm.gmdm_organization_item goi on gi.item_id = goi.item_id and gpl.organization_id = goi.organization_id\n                     left join gdh_mdm.gmdm_area_group_line gagl on gagl.ou_id = gph.ou_id\n                     left join gdh_mdm.gmdm_area_group_header gagh on gagh.group_header_id = gagl.group_header_id\n            where gph.po_status in ('APPROVED', 'CLOSED')\n              and gpl.po_line_status != 'CANCELED'\n        <if test="ouAuthority.includeAllFlag != true">\n            AND gph.ou_id IN\n            <foreach collection="ouAuthority.dataIds" item="dataId" open="(" close=")" separator=",">\n                #{dataId}\n            </foreach>\n        </if>\n        <if test="ouId != null">\n            AND gph.ou_id = #{ouId}\n        </if>\n        <if test="purchaseDateFrom != null">\n            AND gph.po_creation_date >= #{purchaseDateFrom}\n        </if>\n        <if test="purchaseDateTo != null">\n            AND gph.po_creation_date <= #{purchaseDateTo}\n        </if>\n        <if test="centralized != null and centralized == 1">\n            AND goi.centralized_purchasing_flag = '1'\n        </if>\n        group by gph.ou_id,\n                 hou.ou_code,\n                 hou.ou_name,\n                 gi.category_segment1\n    </select>\n    <select id="getTrialInfo" resultType="com.gdh.scm.domain.vo.UnitReceivableTrialVO">\n        select\n            gph.ou_id,\n            hou.ou_name,\n            gph.requisition_ou_id,\n            hou2.ou_name requisition_ou_name,\n            gph.supplier_id,\n            gph.supplier_name,\n            gph.supplier_site_id,\n            gph.supplier_site_name,\n            gph.po_num,\n            gph.centralized_acquisition_center,\n            gpl.po_line_id,\n            gpl.line_num,\n            gpl.item_id,\n            gpl.item_number,\n            gpl.item_description,\n            gpl.specifications,\n            gpl.tax_included_price,\n            gi.category_segment3,\n            hu.uom_name\n        FROM\n            gscm_po_line gpl\n                LEFT JOIN gscm_po_header gph ON gpl.po_header_id = gph.po_header_id\n                LEFT JOIN gdh_mdm.gmdm_item gi ON gi.item_id = gpl.item_id\n                LEFT JOIN hzero_platform.hpfm_uom hu on gpl.uom_id =hu.uom_id\n                LEFT JOIN hzero_platform.hpfm_operation_unit hou on hou.ou_id = gph.ou_id\n                LEFT JOIN hzero_platform.hpfm_operation_unit hou2 ON hou2.ou_id = gph.requisition_ou_id\n        <where>\n            gph.po_category = 'EXTERNAL'\n            AND gph.po_status IN ('INCOMPLETE', 'IN_PROCESS', 'REJECTED')\n            AND gph.unified_sign_flag = 1\n            <if test="ouId != null">\n                AND gph.ou_id = #{ouId}\n            </if>\n            <if test="requisitionOuId != null">\n                AND gph.requisition_ou_id =  #{requisitionOuId}\n            </if>\n            <if test="supplierId != null ">\n                AND  gph.supplier_id = #{supplierId}\n            </if>\n            <if test="agentId != null ">\n                AND  gph.agent_id = #{agentId}\n            </if>\n            <if test="centralizedAcquisitionCenter != null">\n                <choose>\n                    <when test="centralizedAcquisitionCenter == 'empty'">\n                        AND gph.centralized_acquisition_center is null\n                    </when>\n                    <otherwise>\n                        AND gph.centralized_acquisition_center = #{centralizedAcquisitionCenter}\n                    </otherwise>\n                </choose>\n            </if>\n            <if test="poNum != null and poNum != '' ">\n                <bind name = "poNumLike" value = "'%' + poNum + '%'"/>\n                AND  gph.po_num LIKE #{poNumLike}\n            </if>\n            <if test="poCreationDateFrom != null">\n                <bind name = "tempPoCreationDateFrom" value = "poCreationDateFrom + ''"/>\n                AND gph.po_creation_date  >= #{tempPoCreationDateFrom}\n            </if>\n            <if test="poCreationDateEnd != null ">\n                <bind name = "tempPoCreationDateEnd" value = "poCreationDateEnd + ''"/>\n                AND gph.po_creation_date  <= #{tempPoCreationDateEnd}\n            </if>\n            <if test="departmentId != null ">\n                AND gph.department_id  = #{departmentId}\n            </if>\n        </where>\n        ORDER BY gph.po_num, gph.ou_id\n    </select>\n    <select id="selectNotCreateRcv" resultType="com.gdh.scm.domain.entity.PoLine">\n        SELECT gph.po_header_id,\n               gph.po_num,\n               gph.source_code,\n               heu1.user_id receipt_id,\n               heu.user_id agent_id,\n               TIMESTAMPDIFF(DAY,gpl.need_date,NOW()) delay_day,\n               gpl.line_num\n        FROM\n            gscm_po_line gpl\n            LEFT JOIN gscm_po_header gph on gph.po_header_id = gpl.po_header_id\n            LEFT JOIN hzero_platform.hpfm_employee he on gph.agent_id = he.employee_id\n            LEFT JOIN hzero_platform.hpfm_employee_user heu on he.employee_id = heu.employee_id\n            LEFT JOIN hzero_platform.hpfm_employee he1 on gph.receipt_id = he1.employee_id\n            LEFT JOIN hzero_platform.hpfm_employee_user heu1 on he1.employee_id = heu1.employee_id\n        WHERE  gph.po_status = 'APPROVED'\n          and gpl.receive_flag = 1\n          and gpl.quantity_received &lt; gpl.quantity\n          AND TIMESTAMPDIFF( DAY, gpl.need_date, NOW()) >= #{deferredDay}\n    </select>\n</mapper>",
"details": [
     {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "可根据自己的需求，是否要使用",
      "translatedText": "Can be used according to your needs"
    }
  ]
}
```
### DEMOS8

#### Input:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmEsSyncFailureMapper">
    <insert id="replaceSyncLog">
        REPLACE INTO
            dcim.alarm_es_sync_failure (alarm_id, create_time)
        VALUES
        <foreach collection="alarmIds" item="alarmId" separator=",">
            (#{alarmId}, NOW())
        </foreach>
    </insert>
    <delete id="clearAlarmAct">
        DELETE
            a.*
        FROM
            dcim.alarm_act a
        LEFT JOIN
            dcim.alarm_es_sync_failure f ON f.alarm_id = a.id
        WHERE
            (a.recover_flag = 1 OR a.event_type_id = 2)
            AND a.confirm_flag = 1
            AND f.alarm_id IS NULL
            AND a.update_time &lt; #{time}
    </delete>
</mapper>
```

#### Output:

```json
{
  "translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"com.gds.jpi.dcim.core.alarmhandlercenter.mapper.AlarmEsSyncFailureMapper\">\n    <insert id=\"replaceSyncLog\">\n        REPLACE INTO\n            dcim.alarm_es_sync_failure (alarm_id, create_time)\n        VALUES\n        <foreach collection=\"alarmIds\" item=\"alarmId\" separator=\",\" >\n            (#{alarmId}, NOW())\n        </foreach>\n    </insert>\n    <delete id=\"clearAlarmAct\">\n        DELETE\n            a.*\n        FROM\n            dcim.alarm_act a\n        LEFT JOIN\n            dcim.alarm_es_sync_failure f ON f.alarm_id = a.id\n        WHERE\n            (a.recover_flag = 1 OR a.event_type_id = 2)\n            AND a.confirm_flag = 1\n            AND f.alarm_id IS NULL\n            AND a.update_time &lt; #{time}\n    </delete>\n</mapper>",
  "details": []
}

```

### DEMOS9

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
"translatedCode": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace="com.gds.jpi.dcim.core.mapper.alarm.AlarmInfluenceCustomerMapper">\n    <select id="getRoomLocIds" resultType="java.lang.Integer">\n        SELECT\n            ll.id\n        FROM\n            geo.location l\n        JOIN\n            geo.location_type lt ON lt.type_id = l.sp_type\n        JOIN\n            geo.location_category lc ON lc.id = lt.category_id AND lc.code = 'gsac.location_category.modules'\n        LEFT JOIN\n            geo.location ll ON ll.`left` &gt;= l.`left` AND ll.`right` &lt;= l.`right` AND ll.status = 1\n        WHERE\n            l.status = 1\n            AND l.id IN\n            <foreach collection="locationIds" item="locationId" open="(" separator="," close=")">\n                #{locationId}\n            </foreach>\n    </select>\n    <select id="getAssetTypeByRoot" resultType="com.gds.jpi.dcim.vo.alarm.AssetTypeRoot">\n        SELECT\n            t.id AS root_type_id,\n            t.code AS root_type_code,\n            tc.id AS type_id\n        FROM\n            asset.asset_type t\n        JOIN\n            asset.asset_type tc ON tc.`left` &gt;= t.`left` AND tc.`right` &lt;= t.`right` AND tc.is_delete = 0\n        WHERE\n            t.is_delete = 0\n            AND t.code IN\n            <foreach collection="typeCodes" item="typeCode" open="(" separator="," close=")">\n                #{typeCode}\n            </foreach>\n    </select>\n</mapper>",
"details": []
}
```
