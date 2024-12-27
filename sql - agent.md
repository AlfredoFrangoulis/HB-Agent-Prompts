## SQL

### INSTRUCTION

> _You are a professional code translator specializing in translating SQL source code, documentation, and comments.Ensure the line numbers in "details" align exactly with the original input file (do not add new line). Do not include markdown formatting (e.g., ```json) in the response. Return the JSON as plain text. When processing SQL code, ensure all backticks (``) are preserved as plain text. Do not interpret, format, or bold any text enclosed in backticks. For example:
> 
> `table_name` should remain exactly as-is, with the backticks intact and the text table_name treated as regular text. The final JSON output must preserve all backticks as they appear in the input without applying any markdown-style formatting to the enclosed text.
> 
> Translate only Chinese comments or text within the provided SQL code into English while keeping the SQL code structure, formatting, and syntax unchanged. Retain the comment format (e.g., single-line -- or block-style /* */) and ensure comments remain clearly distinguishable from the code. Do not modify SQL keywords, identifiers, or formatting. Return a JSON object as plain text. The structure of the JSON should be as follows:
> 
> "translatedCode": "The complete SQL code translated into English, with all backticks preserved and formatting intact.",
> "details": "lineNumber": "The exact line number of the translation, corresponding to the line break in input code", "lineType": "Whether the line is a comment, annotation, or other.", "jobType": "The type of transformation, e.g., 'Text Translation'.", "originalText": "The text before translation.", "translatedText": "The translated text."
> 
> Align line numbers in the details section with the corresponding lines in the translatedCode. Ensure the translatedCode contains only the English translation for Chinese words and comments, while preserving all other elements of the SQL code, including formatting and syntax. Count empty return lines as a line._
  
### DEMOS1

#### Input:

```sql
CREATE TABLE `cbms`.`sys_device_enum_mapping`  (
     `id` int NOT NULL AUTO_INCREMENT,
     `dc_id` int NOT NULL COMMENT '数据中心id',
     `device_id` int NOT NULL COMMENT '设备ID',
     `temp_attr_id` int NOT NULL COMMENT '弱电标准ID',
     `temp_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '弱电标准编码',
     `seq` int NOT NULL COMMENT '复数测点支路数',
     `enum_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '枚举值Code',
     `value` int NOT NULL COMMENT '枚举值',
     `mapping_value` int NOT NULL COMMENT '枚举映射值',
     `is_del` int NOT NULL DEFAULT 0 COMMENT '是否删除,0未删除，1为已删除',
     PRIMARY KEY (`id`) USING BTREE,
     UNIQUE INDEX `device_seq` (`device_id`, `temp_attr_id`,`seq`,`enum_code`,`mapping_value`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_unicode_ci COMMENT = '枚举映射表' ROW_FORMAT = DYNAMIC;

```

#### Output:

```json
{
  "translatedCode": "CREATE TABLE `cbms`.`sys_device_enum_mapping`  (\n     `id` int NOT NULL AUTO_INCREMENT,\n     `dc_id` int NOT NULL COMMENT 'Data Center ID',\n     `device_id` int NOT NULL COMMENT 'Device ID',\n     `temp_attr_id` int NOT NULL COMMENT 'Weak Electric Standard ID',\n     `temp_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Weak Electric Standard Code',\n     `seq` int NOT NULL COMMENT 'Number of Branch Points for Multiple Measurement Points',\n     `enum_code` varchar(50) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Enumeration Code',\n     `value` int NOT NULL COMMENT 'Enumeration Value',\n     `mapping_value` int NOT NULL COMMENT 'Enumeration Mapping Value',\n     `is_del` int NOT NULL DEFAULT 0 COMMENT 'Deletion Status, 0 for not deleted, 1 for deleted',\n     PRIMARY KEY (`id`) USING BTREE,\n     UNIQUE INDEX `device_seq` (`device_id`, `temp_attr_id`,`seq`,`enum_code`,`mapping_value`) USING BTREE\n) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_unicode_ci COMMENT = 'Enumeration Mapping Table' ROW_FORMAT = DYNAMIC;",
  "details": [
    {
      "lineNumber": 3,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '数据中心id'",
      "translatedText": "COMMENT 'Data Center ID'"
    },
    {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '设备ID'",
      "translatedText": "COMMENT 'Device ID'"
    },
    {
      "lineNumber": 5,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '弱电标准ID'",
      "translatedText": "COMMENT 'Weak Electric Standard ID'"
    },
    {
      "lineNumber": 6,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '弱电标准编码'",
      "translatedText": "COMMENT 'Weak Electric Standard Code'"
    },
    {
      "lineNumber": 7,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '复数测点支路数'",
      "translatedText": "COMMENT 'Number of Branch Points for Multiple Measurement Points'"
    },
    {
      "lineNumber": 8,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '枚举值Code'",
      "translatedText": "COMMENT 'Enumeration Code'"
    },
    {
      "lineNumber": 9,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '枚举值'",
      "translatedText": "COMMENT 'Enumeration Value'"
    },
    {
      "lineNumber": 10,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '枚举映射值'",
      "translatedText": "COMMENT 'Enumeration Mapping Value'"
    },
    {
      "lineNumber": 11,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '是否删除,0未删除，1为已删除'",
      "translatedText": "COMMENT 'Deletion Status, 0 for not deleted, 1 for deleted'"
    },
    {
      "lineNumber": 14,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT = '枚举映射表'",
      "translatedText": "COMMENT = 'Enumeration Mapping Table'"
    }
  ]
}
```

### DEMOS2

#### Input:

```sql
INSERT INTO
  `dcim`.`record_field_category` (`id`, `code`, `name`)
VALUES
  (1, 'adj_factor_related_measurement_parameters', '调节因子相关测量参数'),
  (2, 'dc_energy_consumption', '数据中心能源消耗量');

INSERT INTO
  `dcim`.`record_dc_monthly_temp_field` (`id`, `field_code`, `field_name`, `unit`, `decimal_digit`, `required_flag`, `category_id`, `order_by`)
VALUES
  (1, 'FILL_STORAGE_STATION_DISCHARGE_CAPACITY', '蓄能电站放电量', 'kW·h', 2, 1, 1, 1),
  (2, 'FILL_TOTAL_POWER', '总用电量', 'kW·h', 2, 1, 1, 2),
  (3, 'FILL_STAGGERED_PEAK_COOLING_CAPACITY', '错峰蓄冷量', 't', 2, 1, 1, 3),
  (4, 'FILL_EXTERNAL_COOLING_CAPACITY', '利用外供冷量', 't', 2, 1, 1, 4),
  (5, 'FILL_TOTAL_COOLING', '总用冷量', 'kW', 2, 1, 1, 5),
  (6, 'FILL_RENEWABLE_ENERGY_GENERATION', '可再生能源发电量', 'kW·h', 2, 1, 1, 6),
  (7, 'FILL_CONSUMPTION_GAS', '燃气消耗量', 'm³', 2, 1, 2, 7),
  (8, 'FILL_CONSUMPTION_DIESELOIL', '柴油消耗量', 't', 2, 1, 2, 8),
  (9, 'FILL_CONSUMPTION_WATER', '水资源消耗量', 'm³', 2, 1, 2, 9);
```

#### Output:

```json
{
  "translatedCode": "INSERT INTO\n  `dcim`.`record_field_category` (`id`, `code`, `name`)\nVALUES\n  (1, 'adj_factor_related_measurement_parameters', 'Adjustment factor related measurement parameters'),\n  (2, 'dc_energy_consumption', 'Data center energy consumption');\n\nINSERT INTO\n  `dcim`.`record_dc_monthly_temp_field` (`id`, `field_code`, `field_name`, `unit`, `decimal_digit`, `required_flag`, `category_id`, `order_by`)\nVALUES\n  (1, 'FILL_STORAGE_STATION_DISCHARGE_CAPACITY', 'Energy storage station discharge capacity', 'kW·h', 2, 1, 1, 1),\n  (2, 'FILL_TOTAL_POWER', 'Total power consumption', 'kW·h', 2, 1, 1, 2),\n  (3, 'FILL_STAGGERED_PEAK_COOLING_CAPACITY', 'Peak shaving cooling capacity', 't', 2, 1, 1, 3),\n  (4, 'FILL_EXTERNAL_COOLING_CAPACITY', 'External cooling capacity utilized', 't', 2, 1, 1, 4),\n  (5, 'FILL_TOTAL_COOLING', 'Total cooling consumption', 'kW', 2, 1, 1, 5),\n  (6, 'FILL_RENEWABLE_ENERGY_GENERATION', 'Renewable energy generation', 'kW·h', 2, 1, 1, 6),\n  (7, 'FILL_CONSUMPTION_GAS', 'Natural gas consumption', 'm³', 2, 1, 2, 7),\n  (8, 'FILL_CONSUMPTION_DIESELOIL', 'Diesel consumption', 't', 2, 1, 2, 8),\n  (9, 'FILL_CONSUMPTION_WATER', 'Water consumption', 'm³', 2, 1, 2, 9);",
  "details": [
    {
      "lineNumber": 4,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (1, 'adj_factor_related_measurement_parameters', '调节因子相关测量参数'),",
      "translatedText": "  (1, 'adj_factor_related_measurement_parameters', 'Adjustment factor related measurement parameters'),"
    },
    {
      "lineNumber": 5,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (2, 'dc_energy_consumption', '数据中心能源消耗量');",
      "translatedText": "  (2, 'dc_energy_consumption', 'Data center energy consumption');"
    },
    {
      "lineNumber": 10,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (1, 'FILL_STORAGE_STATION_DISCHARGE_CAPACITY', '蓄能电站放电量', 'kW·h', 2, 1, 1, 1),",
      "translatedText": "  (1, 'FILL_STORAGE_STATION_DISCHARGE_CAPACITY', 'Energy storage station discharge capacity', 'kW·h', 2, 1, 1, 1),"
    },
    {
      "lineNumber": 11,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (2, 'FILL_TOTAL_POWER', '总用电量', 'kW·h', 2, 1, 1, 2),",
      "translatedText": "  (2, 'FILL_TOTAL_POWER', 'Total power consumption', 'kW·h', 2, 1, 1, 2),"
    },
    {
      "lineNumber": 12,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (3, 'FILL_STAGGERED_PEAK_COOLING_CAPACITY', '错峰蓄冷量', 't', 2, 1, 1, 3),",
      "translatedText": "  (3, 'FILL_STAGGERED_PEAK_COOLING_CAPACITY', 'Peak shaving cooling capacity', 't', 2, 1, 1, 3),"
    },
    {
      "lineNumber": 13,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (4, 'FILL_EXTERNAL_COOLING_CAPACITY', '利用外供冷量', 't', 2, 1, 1, 4),",
      "translatedText": "  (4, 'FILL_EXTERNAL_COOLING_CAPACITY', 'External cooling capacity utilized', 't', 2, 1, 1, 4),"
    },
    {
      "lineNumber": 14,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (5, 'FILL_TOTAL_COOLING', '总用冷量', 'kW', 2, 1, 1, 5),",
      "translatedText": "  (5, 'FILL_TOTAL_COOLING', 'Total cooling consumption', 'kW', 2, 1, 1, 5),"
    },
    {
      "lineNumber": 15,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (6, 'FILL_RENEWABLE_ENERGY_GENERATION', '可再生能源发电量', 'kW·h', 2, 1, 1, 6),",
      "translatedText": "  (6, 'FILL_RENEWABLE_ENERGY_GENERATION', 'Renewable energy generation', 'kW·h', 2, 1, 1, 6),"
    },
    {
      "lineNumber": 16,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (7, 'FILL_CONSUMPTION_GAS', '燃气消耗量', 'm³', 2, 1, 2, 7),",
      "translatedText": "  (7, 'FILL_CONSUMPTION_GAS', 'Natural gas consumption', 'm³', 2, 1, 2, 7),"
    },
    {
      "lineNumber": 17,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (8, 'FILL_CONSUMPTION_DIESELOIL', '柴油消耗量', 't', 2, 1, 2, 8),",
      "translatedText": "  (8, 'FILL_CONSUMPTION_DIESELOIL', 'Diesel consumption', 't', 2, 1, 2, 8),"
    },
    {
      "lineNumber": 18,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (9, 'FILL_CONSUMPTION_WATER', '水资源消耗量', 'm³', 2, 1, 2, 9);",
      "translatedText": "  (9, 'FILL_CONSUMPTION_WATER', 'Water consumption', 'm³', 2, 1, 2, 9);"
    }
  ]
}
```

### DEMOS3

#### Input:

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : 测试库-192.168.242.31
 Source Server Type    : MySQL
 Source Server Version : 100312
 Source Host           : 192.168.242.31:3306
 Source Schema         : cbms

 Target Server Type    : MySQL
 Target Server Version : 100312
 File Encoding         : 65001

 Date: 02/04/2021 15:54:44
*/

use cbms;

SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for sys_cabinet_attr_log
-- ----------------------------
DROP TABLE IF EXISTS `sys_cabinet_attr_log`;
CREATE TABLE `sys_cabinet_attr_log`
(
    `id`         int(11)    NOT NULL AUTO_INCREMENT COMMENT '详情id',
    `log_id`     int(11)    NOT NULL COMMENT 'log主键',
    `rel_id`     bigint(20) NULL DEFAULT NULL COMMENT '机柜与PDU关系表',
    `cabinet_id` int(11)    NULL DEFAULT NULL COMMENT '机柜主键',
    `device_id`  bigint(20) NULL DEFAULT NULL COMMENT '类型为PDU的设备id',
    `elec_type`  int(11)    NULL DEFAULT NULL COMMENT '电路线路类型，0：A路  1：B路',
    `elec_sn`    int(11)    NULL DEFAULT NULL COMMENT '电路线路序列号',
    `branch`     int(11)    NULL DEFAULT NULL COMMENT '空开支路',
    PRIMARY KEY (`id`) USING BTREE,
    INDEX `idx_log_id` (`log_id`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '操作日志 '
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_cabinet_excel_tmp
-- ----------------------------
DROP TABLE IF EXISTS `sys_cabinet_excel_tmp`;
CREATE TABLE `sys_cabinet_excel_tmp`
(
    `tmp_id`            int(11)                                                 NOT NULL AUTO_INCREMENT COMMENT '主键',
    `record_id`         int(11)                                                 NOT NULL COMMENT '导入记录主键',
    `level`             tinyint(1)                                              NOT NULL DEFAULT 0 COMMENT '错误等级 0-无 1-warning 2- error 3- all',
    `warning_msg`       varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT '警告信息',
    `error_msg`         varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT '错误信息',
    `excel_location`    varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'excel中填写的位置',
    `location_id`       int(11)                                                 NULL     DEFAULT NULL COMMENT '位置id',
    `name`              varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT '机柜编号',
    `excel_width`       varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'excel中填写的宽度 例: 20cm',
    `width`             double                                                  NULL     DEFAULT NULL COMMENT '宽度(单位：cm)',
    `excel_length`      varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'excel中填写长度 例: 80cm',
    `length`            double                                                  NULL     DEFAULT NULL COMMENT '长度(深度，单位：cm)',
    `excel_capacity_u`  varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'excel中填写U位',
    `capacity_u`        int(11)                                                 NULL     DEFAULT NULL COMMENT 'U位',
    `excel_attr_status` varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'excel_单电源设备情况',
    `attr_status`       tinyint(1)                                              NULL     DEFAULT 0 COMMENT '单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在',
    `description`       varchar(400) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT '描述',
    PRIMARY KEY (`tmp_id`) USING BTREE,
    INDEX `idx_record_id` (`record_id`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '机柜excel数据临时表'
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_cabinet_log
-- ----------------------------
DROP TABLE IF EXISTS `sys_cabinet_log`;
CREATE TABLE `sys_cabinet_log`
(
    `log_id`      int(11)                                                NOT NULL AUTO_INCREMENT COMMENT 'log主键',
    `record_id`   int(11)                                                NULL DEFAULT NULL COMMENT '导入记录',
    `operator`    varchar(45) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '操作人(账号)',
    `ope_time`    datetime(0)                                            NULL DEFAULT current_timestamp() COMMENT '操作时间',
    `source_code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '操作来源 sys_operate_source_dic.code',
    `type_code`   varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '操作类型 sys_operate_type_dic.code',
    `module`      tinyint(1)                                             NULL DEFAULT NULL COMMENT '模块内容 0-基本信息 1-配电信息',
    `cabinet_id`  int(11)                                                NOT NULL COMMENT '机柜主键',
    PRIMARY KEY (`log_id`) USING BTREE,
    INDEX `idx_cabinet_id` (`cabinet_id`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '操作日志 '
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_cabinet_log_detail
-- ----------------------------
DROP TABLE IF EXISTS `sys_cabinet_log_detail`;
CREATE TABLE `sys_cabinet_log_detail`
(
    `id`         int(11)                                                  NOT NULL AUTO_INCREMENT COMMENT '详情id',
    `log_id`     int(11)                                                  NOT NULL COMMENT 'log主键',
    `field_code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci   NULL DEFAULT NULL COMMENT '变更字段 表字段 sys_field_mapping.code',
    `before`     varchar(2048) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '变更前内容',
    `after`      varchar(2048) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '变更后的内容',
    PRIMARY KEY (`id`) USING BTREE,
    INDEX `idx_log_id` (`log_id`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '操作详情 '
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_elec_excel_tmp
-- ----------------------------
DROP TABLE IF EXISTS `sys_elec_excel_tmp`;
CREATE TABLE `sys_elec_excel_tmp`
(
    `tmp_id`              int(11)                                                 NOT NULL AUTO_INCREMENT COMMENT '主键',
    `record_id`           int(11)                                                 NOT NULL COMMENT '导入记录主键',
    `level`               tinyint(1)                                              NULL DEFAULT 0 COMMENT '错误等级 0-无 1-warning 2- error 3- all',
    `warning_msg`         varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '警告信息',
    `error_msg`           varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '错误信息',
    `excel_location`      varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'excel中填写的机房位置',
    `excel_cabinet_name`  varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'excel中填写的机柜编号',
    `excel_device`        varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Excel中填写的设备',
    `excel_elec_type`     varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'excel中填写的 电路类型',
    `excel_elec_sn`       varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'excel中 电路线路序列号',
    `excel_branch`        varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'excel中 空开支路',
    `excel_safe_elec_amp` varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'excel中安全电流上限',
    PRIMARY KEY (`tmp_id`) USING BTREE,
    INDEX `idx_record_id` (`record_id`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '配电excel数据临时表'
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_field_mapping
-- ----------------------------
DROP TABLE IF EXISTS `sys_field_mapping`;
CREATE TABLE `sys_field_mapping`
(
    `id`         int(11)                                                NOT NULL AUTO_INCREMENT COMMENT '主键',
    `code`       varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '表字段 例: width',
    `name`       varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '字段名称 例: 宽度',
    `biz_belong` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT '所属业务 例: COMMON,CABINET,DEVICE',
    `is_show`    tinyint(1)                                             NOT NULL DEFAULT 1 COMMENT '是否展示默认1 0-否1-是',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE INDEX `uk_biz_code` (`biz_belong`, `code`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '字段映射表 '
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_import_record
-- ----------------------------
DROP TABLE IF EXISTS `sys_import_record`;
CREATE TABLE `sys_import_record`
(
    `record_id`      int(11)                                                NOT NULL AUTO_INCREMENT COMMENT '主键',
    `dc_id`          int(11)                                                NOT NULL COMMENT '数据中心',
    `module`         tinyint(1)                                             NOT NULL COMMENT '导入模块 0-机柜 1-配电',
    `create_time`    datetime(0)                                            NULL DEFAULT current_timestamp() COMMENT '创建时间',
    `create_account` varchar(45) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT '创建人',
    `update_time`    datetime(0)                                            NULL DEFAULT current_timestamp() COMMENT '更新时间',
    `status`         tinyint(1)                                             NULL DEFAULT 2 COMMENT '状态 0-取消 1-成功 2-进行中.',
    PRIMARY KEY (`record_id`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '导入记录'
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_operate_source_dict
-- ----------------------------
DROP TABLE IF EXISTS `sys_operate_source_dict`;
CREATE TABLE `sys_operate_source_dict`
(
    `id`   int(11)                                                NOT NULL AUTO_INCREMENT COMMENT '字典主键',
    `code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '标识 例如: IMPORT, EDIT',
    `name` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '名称 例如: 导入, 页面编辑',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE INDEX `uk_code` (`code`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 3
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '操作来源字典表 '
  ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for sys_operate_type_dict
-- ----------------------------
DROP TABLE IF EXISTS `sys_operate_type_dict`;
CREATE TABLE `sys_operate_type_dict`
(
    `id`   int(11)                                                NOT NULL AUTO_INCREMENT COMMENT '字典主键',
    `code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '标识 例如: INSERT,EDIT,DELETE',
    `name` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '字典主键',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE INDEX `uk_code` (`code`) USING BTREE
) ENGINE = InnoDB
  AUTO_INCREMENT = 4
  CHARACTER SET = utf8
  COLLATE = utf8_unicode_ci COMMENT = '操作类型字典表 '
  ROW_FORMAT = Dynamic;

SET FOREIGN_KEY_CHECKS = 1;


-- sys_cabinet表增加dc_id, room字段和索引
ALTER TABLE `cbms`.`sys_cabinet`
    ADD COLUMN `dc_id` int(11) NULL COMMENT '数据中心id',
    ADD INDEX `idx_dc_id` (`dc_id`) USING BTREE;
ALTER TABLE `cbms`.`sys_cabinet`
    ADD COLUMN `room_id` int(11) NULL COMMENT '机房id',
    ADD INDEX `idx_room_id` (`room_id`) USING BTREE;
-- sys_cabinet增加单电源设备情况, 操作配电信息时需要维护此字段.
ALTER TABLE `cbms`.`sys_cabinet`
    ADD COLUMN `attr_status` tinyint(1) DEFAULT '0' COMMENT '单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在';
ALTER TABLE `cbms`.`sys_cabinet`
    ADD COLUMN `attr_count` int(11) DEFAULT '0' COMMENT '配电数量';
ALTER TABLE `cbms`.`sys_cabinet`
    ADD COLUMN `description` VARCHAR(400) DEFAULT NULL COMMENT '备注';
ALTER TABLE `cbms`.`sys_cabinet`
    ADD INDEX `idx_name` (`name`) USING BTREE;

-- 配电关系添加唯一索引.
ALTER TABLE `cbms`.`sys_cabinet_attr`
    ADD UNIQUE INDEX `uk_device_id_branch` (`device_id`, `branch`) USING BTREE,
    ADD UNIQUE INDEX `uk_cabinet_id_elec_sn_elec_type` (`cabinet_id`, `elec_sn`, `elec_type`) USING BTREE;

-- 维护配电数量字段.
UPDATE cbms.sys_cabinet cab
    JOIN ( SELECT cabinet_id, IFNULL(COUNT(0),0) AS num FROM cbms.sys_cabinet_attr GROUP BY cabinet_id ) t1 ON cab.cabinet_id = t1.cabinet_id
SET cab.attr_count = t1.num;
UPDATE cbms.`sys_cabinet` SET attr_count = 0 WHERE attr_count is NULL;

-- 批量编辑cabinet.dc_id
UPDATE cbms.sys_cabinet cab
    JOIN geo.location l ON l.id = cab.location_id
    JOIN geo.location l2 ON l2.left < l.left AND l2.right > l.right
    JOIN geo.dc_location dl ON dl.location_id = l2.id
SET cab.dc_id = dl.dc_id
WHERE dl.dc_id is not null;

-- 批量编辑cabinet.room_id
UPDATE cbms.sys_cabinet cab
    JOIN geo.location l ON l.id = cab.location_id
    JOIN geo.location l2 ON l2.left <= l.left AND l2.right >= l.right
    JOIN dcrm.room r ON r.location_id = l2.id
SET cab.room_id = r.id
WHERE l2.`sp_type` = 1 AND r.id is not null;
```

#### Output:

```json
{
  "translatedCode": "/*\n Navicat Premium Data Transfer\n\n Source Server         : Test Database-192.168.242.31\n Source Server Type    : MySQL\n Source Server Version : 100312\n Source Host           : 192.168.242.31:3306\n Source Schema         : cbms\n\n Target Server Type    : MySQL\n Target Server Version : 100312\n File Encoding         : 65001\n\n Date: 02/04/2021 15:54:44\n*/\n\nuse cbms;\n\nSET NAMES utf8;\nSET FOREIGN_KEY_CHECKS = 0;\n\n-- ----------------------------\n-- Table structure for sys_cabinet_attr_log\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_cabinet_attr_log`;\nCREATE TABLE `sys_cabinet_attr_log`\n(\n    `id`         int(11)    NOT NULL AUTO_INCREMENT COMMENT 'Detail ID',\n    `log_id`     int(11)    NOT NULL COMMENT 'Log primary key',\n    `rel_id`     bigint(20) NULL DEFAULT NULL COMMENT 'Relationship table between cabinet and PDU',\n    `cabinet_id` int(11)    NULL DEFAULT NULL COMMENT 'Cabinet primary key',\n    `device_id`  bigint(20) NULL DEFAULT NULL COMMENT 'Device ID for PDU type',\n    `elec_type`  int(11)    NULL DEFAULT NULL COMMENT 'Electrical circuit type, 0: A-line  1: B-line',\n    `elec_sn`    int(11)    NULL DEFAULT NULL COMMENT 'Electrical circuit serial number',\n    `branch`     int(11)    NULL DEFAULT NULL COMMENT 'Circuit breaker branch',\n    PRIMARY KEY (`id`) USING BTREE,\n    INDEX `idx_log_id` (`log_id`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 1\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Operation log '\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_cabinet_excel_tmp\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_cabinet_excel_tmp`;\nCREATE TABLE `sys_cabinet_excel_tmp`\n(\n    `tmp_id`            int(11)                                                 NOT NULL AUTO_INCREMENT COMMENT 'Primary key',\n    `record_id`         int(11)                                                 NOT NULL COMMENT 'Import record primary key',\n    `level`             tinyint(1)                                              NOT NULL DEFAULT 0 COMMENT 'Error level 0-None 1-Warning 2-Error 3-All',\n    `warning_msg`       varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT 'Warning message',\n    `error_msg`         varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT 'Error message',\n    `excel_location`    varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'Location filled in Excel',\n    `location_id`       int(11)                                                 NULL     DEFAULT NULL COMMENT 'Location ID',\n    `name`              varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'Cabinet number',\n    `excel_width`       varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'Width filled in Excel, e.g., 20cm',\n    `width`             double                                                  NULL     DEFAULT NULL COMMENT 'Width (unit: cm)',\n    `excel_length`      varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'Length filled in Excel, e.g., 80cm',\n    `length`            double                                                  NULL     DEFAULT NULL COMMENT 'Length (depth, unit: cm)',\n    `excel_capacity_u`  varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'U position filled in Excel',\n    `capacity_u`        int(11)                                                 NULL     DEFAULT NULL COMMENT 'U position',\n    `excel_attr_status` varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL     DEFAULT NULL COMMENT 'Single power supply device status in Excel',\n    `attr_status`       tinyint(1)                                              NULL     DEFAULT 0 COMMENT 'Single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist',\n    `description`       varchar(400) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT 'Description',\n    PRIMARY KEY (`tmp_id`) USING BTREE,\n    INDEX `idx_record_id` (`record_id`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 1\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Cabinet Excel data temporary table'\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_cabinet_log\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_cabinet_log`;\nCREATE TABLE `sys_cabinet_log`\n(\n    `log_id`      int(11)                                                NOT NULL AUTO_INCREMENT COMMENT 'Log primary key',\n    `record_id`   int(11)                                                NULL DEFAULT NULL COMMENT 'Import record',\n    `operator`    varchar(45) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Operator (account)',\n    `ope_time`    datetime(0)                                            NULL DEFAULT current_timestamp() COMMENT 'Operation time',\n    `source_code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Operation source sys_operate_source_dic.code',\n    `type_code`   varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Operation type sys_operate_type_dic.code',\n    `module`      tinyint(1)                                             NULL DEFAULT NULL COMMENT 'Module content 0-Basic information 1-Power distribution information',\n    `cabinet_id`  int(11)                                                NOT NULL COMMENT 'Cabinet primary key',\n    PRIMARY KEY (`log_id`) USING BTREE,\n    INDEX `idx_cabinet_id` (`cabinet_id`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 1\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Operation log '\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_cabinet_log_detail\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_cabinet_log_detail`;\nCREATE TABLE `sys_cabinet_log_detail`\n(\n    `id`         int(11)                                                  NOT NULL AUTO_INCREMENT COMMENT 'Detail ID',\n    `log_id`     int(11)                                                  NOT NULL COMMENT 'Log primary key',\n    `field_code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci   NULL DEFAULT NULL COMMENT 'Changed field table field sys_field_mapping.code',\n    `before`     varchar(2048) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Content before change',\n    `after`      varchar(2048) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Content after change',\n    PRIMARY KEY (`id`) USING BTREE,\n    INDEX `idx_log_id` (`log_id`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 1\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Operation details '\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_elec_excel_tmp\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_elec_excel_tmp`;\nCREATE TABLE `sys_elec_excel_tmp`\n(\n    `tmp_id`              int(11)                                                 NOT NULL AUTO_INCREMENT COMMENT 'Primary key',\n    `record_id`           int(11)                                                 NOT NULL COMMENT 'Import record primary key',\n    `level`               tinyint(1)                                              NULL DEFAULT 0 COMMENT 'Error level 0-None 1-Warning 2-Error 3-All',\n    `warning_msg`         varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Warning message',\n    `error_msg`           varchar(300) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Error message',\n    `excel_location`      varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Machine room location filled in Excel',\n    `excel_cabinet_name`  varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Cabinet number filled in Excel',\n    `excel_device`        varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Device filled in Excel',\n    `excel_elec_type`     varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Electrical circuit type filled in Excel',\n    `excel_elec_sn`       varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Electrical circuit serial number in Excel',\n    `excel_branch`        varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Circuit breaker branch in Excel',\n    `excel_safe_elec_amp` varchar(40) CHARACTER SET utf8 COLLATE utf8_unicode_ci  NULL DEFAULT NULL COMMENT 'Safe current upper limit in Excel',\n    PRIMARY KEY (`tmp_id`) USING BTREE,\n    INDEX `idx_record_id` (`record_id`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 1\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Power distribution Excel data temporary table'\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_field_mapping\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_field_mapping`;\nCREATE TABLE `sys_field_mapping`\n(\n    `id`         int(11)                                                NOT NULL AUTO_INCREMENT COMMENT 'Primary key',\n    `code`       varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Table field, e.g., width',\n    `name`       varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Field name, e.g., Width',\n    `biz_belong` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL     DEFAULT NULL COMMENT 'Business category, e.g., COMMON,CABINET,DEVICE',\n    `is_show`    tinyint(1)                                             NOT NULL DEFAULT 1 COMMENT 'Display status, default 1. 0-No 1-Yes',\n    PRIMARY KEY (`id`) USING BTREE,\n    UNIQUE INDEX `uk_biz_code` (`biz_belong`, `code`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 1\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Field mapping table '\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_import_record\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_import_record`;\nCREATE TABLE `sys_import_record`\n(\n    `record_id`      int(11)                                                NOT NULL AUTO_INCREMENT COMMENT 'Primary key',\n    `dc_id`          int(11)                                                NOT NULL COMMENT 'Data center',\n    `module`         tinyint(1)                                             NOT NULL COMMENT 'Import module 0-Cabinet 1-Power distribution',\n    `create_time`    datetime(0)                                            NULL DEFAULT current_timestamp() COMMENT 'Creation time',\n    `create_account` varchar(45) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL COMMENT 'Creator',\n    `update_time`    datetime(0)                                            NULL DEFAULT current_timestamp() COMMENT 'Update time',\n    `status`         tinyint(1)                                             NULL DEFAULT 2 COMMENT 'Status 0-Canceled 1-Successful 2-In progress.',\n    PRIMARY KEY (`record_id`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 1\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Import record'\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_operate_source_dict\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_operate_source_dict`;\nCREATE TABLE `sys_operate_source_dict`\n(\n    `id`   int(11)                                                NOT NULL AUTO_INCREMENT COMMENT 'Dictionary primary key',\n    `code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Identifier, e.g., IMPORT, EDIT',\n    `name` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Name, e.g., Import, Page edit',\n    PRIMARY KEY (`id`) USING BTREE,\n    UNIQUE INDEX `uk_code` (`code`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 3\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Operation source dictionary table '\n  ROW_FORMAT = Dynamic;\n\n-- ----------------------------\n-- Table structure for sys_operate_type_dict\n-- ----------------------------\nDROP TABLE IF EXISTS `sys_operate_type_dict`;\nCREATE TABLE `sys_operate_type_dict`\n(\n    `id`   int(11)                                                NOT NULL AUTO_INCREMENT COMMENT 'Dictionary primary key',\n    `code` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Identifier, e.g., INSERT,EDIT,DELETE',\n    `name` varchar(20) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT 'Dictionary primary key',\n    PRIMARY KEY (`id`) USING BTREE,\n    UNIQUE INDEX `uk_code` (`code`) USING BTREE\n) ENGINE = InnoDB\n  AUTO_INCREMENT = 4\n  CHARACTER SET = utf8\n  COLLATE = utf8_unicode_ci COMMENT = 'Operation type dictionary table '\n  ROW_FORMAT = Dynamic;\n\nSET FOREIGN_KEY_CHECKS = 1;\n\n\n--Add dc_id to sys_cabinet table, room fields and indexes\nALTER TABLE `cbms`.`sys_cabinet`\n    ADD COLUMN `dc_id` int(11) NULL COMMENT 'Data center ID',\n    ADD INDEX `idx_dc_id` (`dc_id`) USING BTREE;\nALTER TABLE `cbms`.`sys_cabinet`\n    ADD COLUMN `room_id` int(11) NULL COMMENT 'Machine room ID',\n    ADD INDEX `idx_room_id` (`room_id`) USING BTREE;\n-- sys_cabinet adds the case of single power supply equipment, and this field needs to be maintained when operating power distribution information.\nALTER TABLE `cbms`.`sys_cabinet`\n    ADD COLUMN `attr_status` tinyint(1) DEFAULT '0' COMMENT 'Single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist';\nALTER TABLE `cbms`.`sys_cabinet`\n    ADD COLUMN `attr_count` int(11) DEFAULT '0' COMMENT 'Power distribution quantity';\nALTER TABLE `cbms`.`sys_cabinet`\n    ADD COLUMN `description` VARCHAR(400) DEFAULT NULL COMMENT 'Remarks';\nALTER TABLE `cbms`.`sys_cabinet`\n    ADD INDEX `idx_name` (`name`) USING BTREE;\n\n-- Add a unique index to the power distribution relationship.\nALTER TABLE `cbms`.`sys_cabinet_attr`\n    ADD UNIQUE INDEX `uk_device_id_branch` (`device_id`, `branch`) USING BTREE,\n    ADD UNIQUE INDEX `uk_cabinet_id_elec_sn_elec_type` (`cabinet_id`, `elec_sn`, `elec_type`) USING BTREE;\n\n-- Maintain the power distribution quantity field.\nUPDATE cbms.sys_cabinet cab\n    JOIN ( SELECT cabinet_id, IFNULL(COUNT(0),0) AS num FROM cbms.sys_cabinet_attr GROUP BY cabinet_id ) t1 ON cab.cabinet_id = t1.cabinet_id\nSET cab.attr_count = t1.num;\nUPDATE cbms.`sys_cabinet` SET attr_count = 0 WHERE attr_count is NULL;\n\n-- Batch edit cabinet.dc_id\nUPDATE cbms.sys_cabinet cab\n    JOIN geo.location l ON l.id = cab.location_id\n    JOIN geo.location l2 ON l2.left < l.left AND l2.right > l.right\n    JOIN geo.dc_location dl ON dl.location_id = l2.id\nSET cab.dc_id = dl.dc_id\nWHERE dl.dc_id is not null;\n\n-- Batch edit cabinet.room_id\nUPDATE cbms.sys_cabinet cab\n    JOIN geo.location l ON l.id = cab.location_id\n    JOIN geo.location l2 ON l2.left <= l.left AND l2.right >= l.right\n    JOIN dcrm.room r ON r.location_id = l2.id\nSET cab.room_id = r.id\nWHERE l2.`sp_type` = 1 AND r.id is not null;",
  "details": [
    {
      "lineNumber": 4,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "Source Server         : 测试库-192.168.242.31",
      "translatedText": "Source Server         : Test Database-192.168.242.31"
    },
    {
      "lineNumber": 28,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "详情id",
      "translatedText": "Detail ID"
    },
    {
      "lineNumber": 29,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "log主键",
      "translatedText": "Log primary key"
    },
    {
      "lineNumber": 30,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜与PDU关系表",
      "translatedText": "Relationship table between cabinet and PDU"
    },
    {
      "lineNumber": 31,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜主键",
      "translatedText": "Cabinet primary key"
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "类型为PDU的设备id",
      "translatedText": "Device ID for PDU type"
    },
    {
      "lineNumber": 33,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电路线路类型，0：A路  1：B路",
      "translatedText": "Electrical circuit type, 0: A-line  1: B-line"
    },
    {
      "lineNumber": 34,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "电路线路序列号",
      "translatedText": "Electrical circuit serial number"
    },
    {
      "lineNumber": 35
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "空开支路",
      "translatedText": "Circuit breaker branch"
    },
    {
      "lineNumber": 41,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作日志 ",
      "translatedText": "Operation log "
    },
    {
      "lineNumber": 50,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "主键",
      "translatedText": "Primary key"
    },
    {
      "lineNumber": 51,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "导入记录主键",
      "translatedText": "Import record primary key"
    },
    {
      "lineNumber": 52,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "错误等级 0-无 1-warning 2- error 3- all",
      "translatedText": "Error level 0-None 1-Warning 2-Error 3-All"
    },
    {
      "lineNumber": 53,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "警告信息",
      "translatedText": "Warning message"
    },
    {
      "lineNumber": 54,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "错误信息",
      "translatedText": "Error message"
    },
    {
      "lineNumber": 55,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中填写的位置",
      "translatedText": "Location filled in Excel"
    },
    {
      "lineNumber": 56,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "位置id",
      "translatedText": "Location ID"
    },
    {
      "lineNumber": 57,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜编号",
      "translatedText": "Cabinet number"
    },
    {
      "lineNumber": 58,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中填写的宽度 例: 20cm",
      "translatedText": "Width filled in Excel, e.g., 20cm"
    },
    {
      "lineNumber": 59,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "宽度(单位：cm)",
      "translatedText": "Width (unit: cm)"
    },
    {
      "lineNumber": 60,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中填写长度 例: 80cm",
      "translatedText": "Length filled in Excel, e.g., 80cm"
    },
    {
      "lineNumber": 61,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "长度(深度，单位：cm)",
      "translatedText": "Length (depth, unit: cm)"
    },
    {
      "lineNumber": 62,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中填写U位",
      "translatedText": "U position filled in Excel"
    },
    {
      "lineNumber": 63,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "U位",
      "translatedText": "U position"
    },
    {
      "lineNumber": 64,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel_单电源设备情况",
      "translatedText": "Single power supply device status in Excel"
    },
    {
      "lineNumber": 65,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在",
      "translatedText": "Single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist"
    },
    {
      "lineNumber": 66,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "描述",
      "translatedText": "Description"
    },
    {
      "lineNumber": 72,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜excel数据临时表",
      "translatedText": "Cabinet Excel data temporary table"
    },
    {
      "lineNumber": 81,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "log主键",
      "translatedText": "Log primary key"
    },
    {
      "lineNumber": 82,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "导入记录",
      "translatedText": "Import record"
    },
    {
      "lineNumber": 83,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作人(账号)",
      "translatedText": "Operator (account)"
    },
    {
      "lineNumber": 84,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作时间",
      "translatedText": "Operation time"
    },
    {
      "lineNumber": 85,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作来源 sys_operate_source_dic.code",
      "translatedText": "Operation source sys_operate_source_dic.code"
    },
    {
      "lineNumber": 86,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作类型 sys_operate_type_dic.code",
      "translatedText": "Operation type sys_operate_type_dic.code"
    },
    {
      "lineNumber": 87,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "模块内容 0-基本信息 1-配电信息",
      "translatedText": "Module content 0-Basic information 1-Power distribution information"
    },
    {
      "lineNumber": 88,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机柜主键",
      "translatedText": "Cabinet primary key"
    },
    {
      "lineNumber": 94,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作日志 ",
      "translatedText": "Operation log "
    },
    {
      "lineNumber": 103,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "详情id",
      "translatedText": "Detail ID"
    },
    {
      "lineNumber": 104,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "log主键",
      "translatedText": "Log primary key"
    },
    {
      "lineNumber": 105,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "变更字段 表字段 sys_field_mapping.code",
      "translatedText": "Changed field table field sys_field_mapping.code"
    },
    {
      "lineNumber": 106,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "变更前内容",
      "translatedText": "Content before change"
    },
    {
      "lineNumber": 107,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "变更后的内容",
      "translatedText": "Content after change"
    },
    {
      "lineNumber": 113,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作详情 ",
      "translatedText": "Operation details "
    },
    {
      "lineNumber": 122,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "主键",
      "translatedText": "Primary key"
    },
    {
      "lineNumber": 123,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "导入记录主键",
      "translatedText": "Import record primary key"
    },
    {
      "lineNumber": 124,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "错误等级 0-无 1-warning 2- error 3- all",
      "translatedText": "Error level 0-None 1-Warning 2-Error 3-All"
    },
    {
      "lineNumber": 125,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "警告信息",
      "translatedText": "Warning message"
    },
    {
      "lineNumber": 126,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "错误信息",
      "translatedText": "Error message"
    },
    {
      "lineNumber": 127,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中填写的机房位置",
      "translatedText": "Machine room location filled in Excel"
    },
    {
      "lineNumber": 128,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中填写的机柜编号",
      "translatedText": "Cabinet number filled in Excel"
    },
    {
      "lineNumber": 129,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "Excel中填写的设备",
      "translatedText": "Device filled in Excel"
    },
    {
      "lineNumber": 130,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中填写的 电路类型",
      "translatedText": "Electrical circuit type filled in Excel"
    },
    {
      "lineNumber": 131,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中 电路线路序列号",
      "translatedText": "Electrical circuit serial number in Excel"
    },
    {
      "lineNumber": 132,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中 空开支路",
      "translatedText": "Circuit breaker branch in Excel"
    },
    {
      "lineNumber": 133,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "excel中安全电流上限",
      "translatedText": "Safe current upper limit in Excel"
    },
    {
      "lineNumber": 139,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "配电excel数据临时表",
      "translatedText": "Power distribution Excel data temporary table"
    },
    {
      "lineNumber": 148,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "主键",
      "translatedText": "Primary key"
    },
    {
      "lineNumber": 149,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "表字段 例: width",
      "translatedText": "Table field, e.g., width"
    },
    {
      "lineNumber": 150,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "字段名称 例: 宽度",
      "translatedText": "Field name, e.g., Width"
    },
    {
      "lineNumber": 151,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "所属业务 例: COMMON,CABINET,DEVICE",
      "translatedText": "Business category, e.g., COMMON,CABINET,DEVICE"
    },
    {
      "lineNumber": 152,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "是否展示默认1 0-否1-是",
      "translatedText": "Display status, default 1. 0-No 1-Yes"
    },
    {
      "lineNumber": 158,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "字段映射表 ",
      "translatedText": "Field mapping table "
    },
    {
      "lineNumber": 167,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "主键",
      "translatedText": "Primary key"
    },
    {
      "lineNumber": 168,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "数据中心",
      "translatedText": "Data center"
    },
    {
      "lineNumber": 169,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "导入模块 0-机柜 1-配电",
      "translatedText": "Import module 0-Cabinet 1-Power distribution"
    },
    {
      "lineNumber": 170,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "创建时间",
      "translatedText": "Creation time"
    },
    {
      "lineNumber": 171,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "创建人",
      "translatedText": "Creator"
    },
    {
      "lineNumber": 172,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "更新时间",
      "translatedText": "Update time"
    },
    {
      "lineNumber": 173,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "状态 0-取消 1-成功 2-进行中.",
      "translatedText": "Status 0-Canceled 1-Successful 2-In progress."
    },
    {
      "lineNumber": 178,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "导入记录",
      "translatedText": "Import record"
    },
    {
      "lineNumber": 187,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "字典主键",
      "translatedText": "Dictionary primary key"
    },
    {
      "lineNumber": 188,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "标识 例如: IMPORT, EDIT",
      "translatedText": "Identifier, e.g., IMPORT, EDIT"
    },
    {
      "lineNumber": 189,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "名称 例如: 导入, 页面编辑",
      "translatedText": "Name, e.g., Import, Page edit"
    },
    {
      "lineNumber": 195,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作来源字典表 ",
      "translatedText": "Operation source dictionary table "
    },
    {
      "lineNumber": 204,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "字典主键",
      "translatedText": "Dictionary primary key"
    },
    {
      "lineNumber": 205,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "标识 例如: INSERT,EDIT,DELETE",
      "translatedText": "Identifier, e.g., INSERT,EDIT,DELETE"
    },
    {
      "lineNumber": 206,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "字典主键",
      "translatedText": "Dictionary primary key"
    },
    {
      "lineNumber": 212,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "操作类型字典表 ",
      "translatedText": "Operation type dictionary table "
    },
    {
      "lineNumber": 218,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- sys_cabinet表增加dc_id, room字段和索引",
      "translatedText": "-- Add dc_id, room fields and indexes to the sys_cabinet table"
    },
    {
      "lineNumber": 220,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "数据中心id",
      "translatedText": "Data center ID"
    },
    {
      "lineNumber": 223,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "机房id",
      "translatedText": "Machine room ID"
    },
    {
      "lineNumber": 225,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- sys_cabinet增加单电源设备情况, 操作配电信息时需要维护此字段.",
      "translatedText": "-- sys_cabinet adds the case of single power supply equipment. This field needs to be maintained when operating power distribution information."
    },

    {
      "lineNumber": 227,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在",
      "translatedText": "Single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist"
    },
    {
      "lineNumber": 229,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "配电数量",
      "translatedText": "Power distribution quantity"
    },
    {
      "lineNumber": 231,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "备注",
      "translatedText": "Remarks"
    },
    {
      "lineNumber": 235,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 配电关系添加唯一索引.",
      "translatedText": "-- Add a unique index to the power distribution relationship."
    },
    {
      "lineNumber": 240,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 维护配电数量字段.",
      "translatedText": "-- Maintain the power distribution quantity field."
    },
    {
      "lineNumber": 246,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 批量编辑cabinet.dc_id",
      "translatedText": "-- Batch edit cabinet.dc_id"
    },

    {
      "lineNumber": 254,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 批量编辑cabinet.room_id",
      "translatedText": "-- Batch edit cabinet.room_id"
    },
  ]
}
```

### DEMOS4

#### Input:

```sql
-- 上线数据中心配置
CREATE TABLE `dcim`.`park_online_dc` (
  `dc_id` int(11) NOT NULL COMMENT '上线数据中心id',
  PRIMARY KEY (`dc_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='dcim园区侧上线数据中心';

INSERT INTO
  `dcim`.`park_online_dc` (`dc_id`)
VALUES
  (47),
  (98),
  (102),
  (124),
  (67),
  (86),
  (118);

-- 上线数据中心的完整性统计数据表
CREATE PROCEDURE `cbms`.`proc_collect_integrity`(in arr VARCHAR(150))
BEGIN
    DECLARE len INT;
    DECLARE i INT;
    DECLARE sql_text VARCHAR(5000);
    DECLARE dc_id INT;

    SET len = LENGTH(arr) - LENGTH(REPLACE(arr, ',', '')) + 1;
    SET i = 1;
    SET sql_text = '
      CREATE TABLE `cbms`.sys_attr_collect_integrity_${dc_id} (
      `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT "主键",
      `collect_start_time` datetime NOT NULL COMMENT "采集周期开始时间，周期时长为半小时",
      `collect_end_time` datetime NOT NULL COMMENT "采集周期结束时间，显示用冗余字段",
      `sys_attr_id` bigint(20) NOT NULL COMMENT "运行参数id，cbms.sys_device_attr.sys_attr_id",
      `attr_temp_code` varchar(50) COLLATE utf8_unicode_ci NOT NULL COMMENT "标准运行参数code",
      `sys_attr_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "运行参数名",
      `collect_cnt` int(11) NOT NULL COMMENT "采集周期内总采集次数",
      `error_cnt` int(11) NOT NULL COMMENT "采集周期内异常次数",
      `error_frequency` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "异常采集频次示例",
      `bms_agent_id` int(11) NOT NULL COMMENT "bms产品id",
      `bms_tag_id` bigint(20) NOT NULL COMMENT "测点id",
      `bms_tag_no` varchar(64) COLLATE utf8_unicode_ci NOT NULL COMMENT "测点编号",
      `bms_tag_name` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "测点名",
      `sys_device_id` bigint(20) DEFAULT NULL COMMENT "设备id",
      `sys_device_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "设备名",
      `seq` int(11) NOT NULL COMMENT "运行参数序号",
      `asset_type_id` int(11) DEFAULT NULL COMMENT "设备类型id",
      `asset_type_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "设备类型名",
      `dc_id` int(11) NOT NULL COMMENT "数据中心id",
      `dc_name` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "数据中心名",
      `create_time` datetime DEFAULT NULL COMMENT "记录创建时间，无业务含义",
      PRIMARY KEY (`id`),
      UNIQUE KEY `idx_collect_time_attr_id_device_id` (`collect_start_time`,`sys_attr_id`,`sys_device_id`) USING BTREE COMMENT "device_id冗余，提高统计效率用，需要保证数据正确（attr和device多对一）",
      KEY `idx_sys_attr_id` (`sys_attr_id`) USING BTREE,
      KEY `idx_attr_temp_code` (`attr_temp_code`) USING BTREE,
      KEY `idx_tag_id` (`bms_tag_id`) USING BTREE,
      KEY `idx_device_id` (`sys_device_id`),
      KEY `idx_asset_type_id` (`asset_type_id`)) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT="完整性运行参数统计信息表";';

  WHILE
      i <= len DO
    SET dc_id = SUBSTRING_INDEX(SUBSTRING_INDEX(arr, ',', i), ',', -1);
    PREPARE h_Sql from REPLACE(sql_text, '${dc_id}', dc_id);
    EXECUTE h_Sql;

    SET i = i + 1;
  END WHILE;
END;
CALL cbms.proc_collect_integrity('47,98,102,124,67,86,118');

-- 上线数据中心的有效性统计数据表
CREATE PROCEDURE `cbms`.`proc_collect_validity`(in arr VARCHAR(150))
BEGIN
    DECLARE len INT;
    DECLARE i INT;
    DECLARE sql_text VARCHAR(5000);
    DECLARE dc_id INT;

    SET len = LENGTH(arr) - LENGTH(REPLACE(arr, ',', '')) + 1;
    SET i = 1;
    SET sql_text = '
  CREATE TABLE `cbms`.sys_attr_collect_validity_${dc_id} (
    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT "主键",
    `collect_start_time` datetime NOT NULL COMMENT "采集周期开始时间，周期时长为半小时",
    `collect_end_time` datetime NOT NULL COMMENT "采集周期结束时间，显示用冗余字段",
    `sys_attr_id` bigint(20) NOT NULL COMMENT "运行参数id，cbms.sys_device_attr.sys_attr_id",
    `attr_temp_code` varchar(50) COLLATE utf8_unicode_ci NOT NULL COMMENT "标准运行参数code",
    `sys_attr_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "运行参数名",
    `error_cnt` int(11) NOT NULL COMMENT "采集周期内异常次数",
    `error_value` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "异常值内容示例",
    `bms_agent_id` int(11) NOT NULL COMMENT "bms产品id",
    `bms_tag_id` bigint(20) NOT NULL COMMENT "测点id",
    `bms_tag_no` varchar(64) COLLATE utf8_unicode_ci NOT NULL COMMENT "测点编号",
    `bms_tag_name` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "测点名",
    `sys_device_id` bigint(20) DEFAULT NULL COMMENT "设备id",
    `sys_device_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "设备名",
    `seq` int(11) NOT NULL COMMENT "运行参数序号",
    `asset_type_id` int(11) DEFAULT NULL COMMENT "设备类型id",
    `asset_type_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "设备类型名",
    `dc_id` int(11) NOT NULL COMMENT "数据中心id",
    `dc_name` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT "数据中心名",
    `create_time` datetime DEFAULT NULL COMMENT "记录创建时间，无业务含义",
    PRIMARY KEY (`id`),
    UNIQUE KEY `idx_collect_time_attr_id_device_id` (`collect_start_time`,`sys_attr_id`,`sys_device_id`) USING BTREE COMMENT "device_id冗余，提高统计效率用，需要保证数据正确（attr和device多对一）",
    KEY `idx_sys_attr_id` (`sys_attr_id`) USING BTREE,
    KEY `idx_attr_temp_code` (`attr_temp_code`) USING BTREE,
    KEY `idx_tag_id` (`bms_tag_id`) USING BTREE,
    KEY `idx_device_id` (`sys_device_id`),
    KEY `idx_asset_type_id` (`asset_type_id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT="有效性运行参数统计信息表";';

  WHILE
      i <= len DO
    SET dc_id = SUBSTRING_INDEX(SUBSTRING_INDEX(arr, ',', i), ',', -1);
    prepare h_Sql from REPLACE(sql_text, '${dc_id}', dc_id);
    execute h_Sql;

    SET i = i + 1;
  END WHILE;
END;
CALL cbms.proc_collect_validity('47,98,102,124,67,86,118');

-- 台账导出相关
CREATE TABLE `cbms`.`sys_attr_collect_export_record` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `account` varchar(45) COLLATE utf8_unicode_ci NOT NULL COMMENT '导出人',
  `file_name` varchar(64) COLLATE utf8_unicode_ci NOT NULL COMMENT '文件名',
  `file_ext` varchar(16) COLLATE utf8_unicode_ci NOT NULL COMMENT '文件扩展名',
  `obj_name` varchar(45) COLLATE utf8_unicode_ci NOT NULL COMMENT '文件保存在minio中的objectName',
  `status` int(11) NOT NULL COMMENT '状态',
  `type` int(11) NOT NULL COMMENT '类型，1：完整性问题；2：有效性问题，3：异常测点数据；4：所有测点数据',
  `request_json` varchar(1000) COLLATE utf8_unicode_ci NOT NULL COMMENT '筛选条件入参json',
  `request_descr` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT '筛选条件入参描述',
  `create_time` datetime NOT NULL COMMENT '导出记录创建时间',
  `ready_time` datetime DEFAULT NULL COMMENT '文件生成时间',
  `download_time` datetime DEFAULT NULL COMMENT '下载时间',
  PRIMARY KEY (`id`),
  KEY `idx_account` (`account`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='运行参数采集导出记录表';

CREATE TABLE `cbms`.`sys_attr_collect_export_status_dict` (
  `id` int(11) NOT NULL,
  `code` varchar(45) COLLATE utf8_unicode_ci NOT NULL,
  `name` varchar(45) COLLATE utf8_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='运行参数采集数据导出状态';

INSERT INTO
  `cbms`.`sys_attr_collect_export_status_dict` (`id`, `code`, `name`)
VALUES
  (-1, 'generate_failure', '生成失败'),
  (1, 'to_generate', '待生成'),
  (2, 'to_download', '待下载'),
  (3, 'have_downloaded', '已下载');
```

#### Output:

```json
{
  "translatedCode": "-- Online data center configuration\nCREATE TABLE `dcim`.`park_online_dc` (\n  `dc_id` int(11) NOT NULL COMMENT 'Online data center id',\n  PRIMARY KEY (`dc_id`)\n) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='dcim park side online data center';\n\nINSERT INTO\n  `dcim`.`park_online_dc` (`dc_id`)\nVALUES\n  (47),\n  (98),\n  (102),\n  (124),\n  (67),\n  (86),\n  (118);\n\n-- Completeness statistics table for online data centers\nCREATE PROCEDURE `cbms`.`proc_collect_integrity`(in arr VARCHAR(150))\nBEGIN\n    DECLARE len INT;\n    DECLARE i INT;\n    DECLARE sql_text VARCHAR(5000);\n    DECLARE dc_id INT;\n\n    SET len = LENGTH(arr) - LENGTH(REPLACE(arr, ',', '')) + 1;\n    SET i = 1;\n    SET sql_text = ' \n      CREATE TABLE `cbms`.sys_attr_collect_integrity_${dc_id} (\n      `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT \"Primary key\",\n      `collect_start_time` datetime NOT NULL COMMENT \"Collection period start time, period duration is half an hour\",\n      `collect_end_time` datetime NOT NULL COMMENT \"Collection period end time, display redundant field\",\n      `sys_attr_id` bigint(20) NOT NULL COMMENT \"Operating parameter id, cbms.sys_device_attr.sys_attr_id\",\n      `attr_temp_code` varchar(50) COLLATE utf8_unicode_ci NOT NULL COMMENT \"Standard operating parameter code\",\n      `sys_attr_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Operating parameter name\",\n      `collect_cnt` int(11) NOT NULL COMMENT \"Total number of collections in the collection period\",\n      `error_cnt` int(11) NOT NULL COMMENT \"Number of exceptions in the collection period\",\n      `error_frequency` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Example of exceptional collection frequency\",\n      `bms_agent_id` int(11) NOT NULL COMMENT \"bms product id\",\n      `bms_tag_id` bigint(20) NOT NULL COMMENT \"Measurement point id\",\n      `bms_tag_no` varchar(64) COLLATE utf8_unicode_ci NOT NULL COMMENT \"Measurement point number\",\n      `bms_tag_name` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Measurement point name\",\n      `sys_device_id` bigint(20) DEFAULT NULL COMMENT \"Device id\",\n      `sys_device_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Device name\",\n      `seq` int(11) NOT NULL COMMENT \"Operating parameter serial number\",\n      `asset_type_id` int(11) DEFAULT NULL COMMENT \"Device type id\",\n      `asset_type_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Device type name\",\n      `dc_id` int(11) NOT NULL COMMENT \"Data center id\",\n      `dc_name` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Data center name\",\n      `create_time` datetime DEFAULT NULL COMMENT \"Record creation time, no business meaning\",\n      PRIMARY KEY (`id`),\n      UNIQUE KEY `idx_collect_time_attr_id_device_id` (`collect_start_time`,`sys_attr_id`,`sys_device_id`) USING BTREE COMMENT \"device_id redundancy, used to improve statistical efficiency, need to ensure data correctness (attr and device are one-to-many)\",\n      KEY `idx_sys_attr_id` (`sys_attr_id`) USING BTREE,\n      KEY `idx_attr_temp_code` (`attr_temp_code`) USING BTREE,\n      KEY `idx_tag_id` (`bms_tag_id`) USING BTREE,\n      KEY `idx_device_id` (`sys_device_id`),\n      KEY `idx_asset_type_id` (`asset_type_id`)) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT=\"Completeness operating parameter statistics table\";';\n\n  WHILE\n      i <= len DO\n    SET dc_id = SUBSTRING_INDEX(SUBSTRING_INDEX(arr, ',', i), ',', -1);\n    PREPARE h_Sql from REPLACE(sql_text, '${dc_id}', dc_id);\n    EXECUTE h_Sql;\n\n    SET i = i + 1;\n  END WHILE;\nEND;\nCALL cbms.proc_collect_integrity('47,98,102,124,67,86,118');\n\n-- Validity statistics table for online data centers\nCREATE PROCEDURE `cbms`.`proc_collect_validity`(in arr VARCHAR(150))\nBEGIN\n    DECLARE len INT;\n    DECLARE i INT;\n    DECLARE sql_text VARCHAR(5000);\n    DECLARE dc_id INT;\n\n    SET len = LENGTH(arr) - LENGTH(REPLACE(arr, ',', '')) + 1;\n    SET i = 1;\n    SET sql_text = ' \n  CREATE TABLE `cbms`.sys_attr_collect_validity_${dc_id} (\n    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT \"Primary key\",\n    `collect_start_time` datetime NOT NULL COMMENT \"Collection period start time, period duration is half an hour\",\n    `collect_end_time` datetime NOT NULL COMMENT \"Collection period end time, display redundant field\",\n    `sys_attr_id` bigint(20) NOT NULL COMMENT \"Operating parameter id, cbms.sys_device_attr.sys_attr_id\",\n    `attr_temp_code` varchar(50) COLLATE utf8_unicode_ci NOT NULL COMMENT \"Standard operating parameter code\",\n    `sys_attr_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Operating parameter name\",\n    `error_cnt` int(11) NOT NULL COMMENT \"Number of exceptions in the collection period\",\n    `error_value` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Example of exceptional value content\",\n    `bms_agent_id` int(11) NOT NULL COMMENT \"bms product id\",\n    `bms_tag_id` bigint(20) NOT NULL COMMENT \"Measurement point id\",\n    `bms_tag_no` varchar(64) COLLATE utf8_unicode_ci NOT NULL COMMENT \"Measurement point number\",\n    `bms_tag_name` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Measurement point name\",\n    `sys_device_id` bigint(20) DEFAULT NULL COMMENT \"Device id\",\n    `sys_device_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Device name\",\n    `seq` int(11) NOT NULL COMMENT \"Operating parameter serial number\",\n    `asset_type_id` int(11) DEFAULT NULL COMMENT \"Device type id\",\n    `asset_type_name` varchar(50) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Device type name\",\n    `dc_id` int(11) NOT NULL COMMENT \"Data center id\",\n    `dc_name` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT \"Data center name\",\n    `create_time` datetime DEFAULT NULL COMMENT \"Record creation time, no business meaning\",\n    PRIMARY KEY (`id`),\n    UNIQUE KEY `idx_collect_time_attr_id_device_id` (`collect_start_time`,`sys_attr_id`,`sys_device_id`) USING BTREE COMMENT \"device_id redundancy, used to improve statistical efficiency, need to ensure data correctness (attr and device are one-to-many)\",\n    KEY `idx_sys_attr_id` (`sys_attr_id`) USING BTREE,\n    KEY `idx_attr_temp_code` (`attr_temp_code`) USING BTREE,\n    KEY `idx_tag_id` (`bms_tag_id`) USING BTREE,\n    KEY `idx_device_id` (`sys_device_id`),\n    KEY `idx_asset_type_id` (`asset_type_id`) \n  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT=\"Validity operating parameter statistics table\";';\n\n  WHILE\n      i <= len DO\n    SET dc_id = SUBSTRING_INDEX(SUBSTRING_INDEX(arr, ',', i), ',', -1);\n    prepare h_Sql from REPLACE(sql_text, '${dc_id}', dc_id);\n    execute h_Sql;\n\n    SET i = i + 1;\n  END WHILE;\nEND;\nCALL cbms.proc_collect_validity('47,98,102,124,67,86,118');\n\n-- Ledger export related\nCREATE TABLE `cbms`.`sys_attr_collect_export_record` (\n  `id` int(11) NOT NULL AUTO_INCREMENT,\n  `account` varchar(45) COLLATE utf8_unicode_ci NOT NULL COMMENT 'Exporter',\n  `file_name` varchar(64) COLLATE utf8_unicode_ci NOT NULL COMMENT 'File name',\n  `file_ext` varchar(16) COLLATE utf8_unicode_ci NOT NULL COMMENT 'File extension',\n  `obj_name` varchar(45) COLLATE utf8_unicode_ci NOT NULL COMMENT 'The objectName of the file saved in minio',\n  `status` int(11) NOT NULL COMMENT 'Status',\n  `type` int(11) NOT NULL COMMENT 'Type, 1: Completeness issues; 2: Validity issues, 3: Abnormal measurement point data; 4: All measurement point data',\n  `request_json` varchar(1000) COLLATE utf8_unicode_ci NOT NULL COMMENT 'Filter condition input parameter json',\n  `request_descr` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT 'Filter condition input parameter description',\n  `create_time` datetime NOT NULL COMMENT 'Export record creation time',\n  `ready_time` datetime DEFAULT NULL COMMENT 'File generation time',\n  `download_time` datetime DEFAULT NULL COMMENT 'Download time',\n  PRIMARY KEY (`id`),\n  KEY `idx_account` (`account`) USING BTREE\n) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='Operating parameter collection export record table';\n\nCREATE TABLE `cbms`.`sys_attr_collect_export_status_dict` (\n  `id` int(11) NOT NULL,\n  `code` varchar(45) COLLATE utf8_unicode_ci NOT NULL,\n  `name` varchar(45) COLLATE utf8_unicode_ci DEFAULT NULL,\n  PRIMARY KEY (`id`)\n) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='Operating parameter collection data export status';\n\nINSERT INTO\n  `cbms`.`sys_attr_collect_export_status_dict` (`id`, `code`, `name`)\nVALUES\n  (-1, 'generate_failure', 'Generation failed'),\n  (1, 'to_generate', 'To be generated'),\n  (2, 'to_download', 'To be downloaded'),\n  (3, 'have_downloaded', 'Downloaded');",
  "details": [
    {
      "lineNumber": 1,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 上线数据中心配置",
      "translatedText": "-- Online data center configuration"
    },
    {
      "lineNumber": 3,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '上线数据中心id'",
      "translatedText": "COMMENT 'Online data center id'"
    },
    {
      "lineNumber": 5,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT='dcim园区侧上线数据中心'",
      "translatedText": "COMMENT='dcim park side online data center'"
    },
    {
      "lineNumber": 18,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 上线数据中心的完整性统计数据表",
      "translatedText": "-- Completeness statistics table for online data centers"
    },
    {
      "lineNumber": 30,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"主键\"",
      "translatedText": "COMMENT \"Primary key\""
    },
    {
      "lineNumber": 31,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"采集周期开始时间，周期时长为半小时\"",
      "translatedText": "COMMENT \"Collection period start time, period duration is half an hour\""
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"采集周期结束时间，显示用冗余字段\"",
      "translatedText": "COMMENT \"Collection period end time, display redundant field\""
    },
    {
      "lineNumber": 33,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"运行参数id，cbms.sys_device_attr.sys_attr_id\"",
      "translatedText": "COMMENT \"Operating parameter id, cbms.sys_device_attr.sys_attr_id\""
    },
    {
      "lineNumber": 34,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"标准运行参数code\"",
      "translatedText": "COMMENT \"Standard operating parameter code\""
    },
    {
      "lineNumber": 35,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"运行参数名\"",
      "translatedText": "COMMENT \"Operating parameter name\""
    },
    {
      "lineNumber": 36,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"采集周期内总采集次数\"",
      "translatedText": "COMMENT \"Total number of collections in the collection period\""
    },
    {
      "lineNumber": 37,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"采集周期内异常次数\"",
      "translatedText": "COMMENT \"Number of exceptions in the collection period\""
    },
    {
      "lineNumber": 38,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"异常采集频次示例\"",
      "translatedText": "COMMENT \"Example of exceptional collection frequency\""
    },
    {
      "lineNumber": 39,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"bms产品id\"",
      "translatedText": "COMMENT \"bms product id\""
    },
    {
      "lineNumber": 40,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"测点id\"",
      "translatedText": "COMMENT \"Measurement point id\""
    },
    {
      "lineNumber": 41,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"测点编号\"",
      "translatedText": "COMMENT \"Measurement point number\""
    },
    {
      "lineNumber": 42,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"测点名\"",
      "translatedText": "COMMENT \"Measurement point name\""
    },
    {
      "lineNumber": 43,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备id\"",
      "translatedText": "COMMENT \"Device id\""
    },
    {
      "lineNumber": 44,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备名\"",
      "translatedText": "COMMENT \"Device name\""
    },
    {
      "lineNumber": 45,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"运行参数序号\"",
      "translatedText": "COMMENT \"Operating parameter serial number\""
    },
    {
      "lineNumber": 46,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备类型id\"",
      "translatedText": "COMMENT \"Device type id\""
    },
    {
      "lineNumber": 47,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备类型名\"",
      "translatedText": "COMMENT \"Device type name\""
    },
    {
      "lineNumber": 48,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"数据中心id\"",
      "translatedText": "COMMENT \"Data center id\""
    },
    {
      "lineNumber": 49,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"数据中心名\"",
      "translatedText": "COMMENT \"Data center name\""
    },
    {
      "lineNumber": 50,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"记录创建时间，无业务含义\"",
      "translatedText": "COMMENT \"Record creation time, no business meaning\""
    },
    {
      "lineNumber": 52,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"device_id冗余，提高统计效率用，需要保证数据正确（attr和device多对一）\"",
      "translatedText": "COMMENT \"device_id redundancy, used to improve statistical efficiency, need to ensure data correctness (attr and device are one-to-many)\""
    },
    {
      "lineNumber": 57,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT=\"完整性运行参数统计信息表\"",
      "translatedText": "COMMENT=\"Completeness operating parameter statistics table\""
    },
    {
      "lineNumber": 70,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 上线数据中心的有效性统计数据表",
      "translatedText": "-- Validity statistics table for online data centers"
    },
    {
      "lineNumber": 82,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"主键\"",
      "translatedText": "COMMENT \"Primary key\""
    },
    {
      "lineNumber": 83,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"采集周期开始时间，周期时长为半小时\"",
      "translatedText": "COMMENT \"Collection period start time, period duration is half an hour\""
    },
    {
      "lineNumber": 84,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"采集周期结束时间，显示用冗余字段\"",
      "translatedText": "COMMENT \"Collection period end time, display redundant field\""
    },
    {
      "lineNumber": 85,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"运行参数id，cbms.sys_device_attr.sys_attr_id\"",
      "translatedText": "COMMENT \"Operating parameter id, cbms.sys_device_attr.sys_attr_id\""
    },
    {
      "lineNumber": 86,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"标准运行参数code\"",
      "translatedText": "COMMENT \"Standard operating parameter code\""
    },
    {
      "lineNumber": 87,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"运行参数名\"",
      "translatedText": "COMMENT \"Operating parameter name\""
    },
    {
      "lineNumber": 88,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"采集周期内异常次数\"",
      "translatedText": "COMMENT \"Number of exceptions in the collection period\""
    },
    {
      "lineNumber": 89,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"异常值内容示例\"",
      "translatedText": "COMMENT \"Example of exceptional value content\""
    },
    {
      "lineNumber": 90,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"bms产品id\"",
      "translatedText": "COMMENT \"bms product id\""
    },
    {
      "lineNumber": 91,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"测点id\"",
      "translatedText": "COMMENT \"Measurement point id\""
    },
    {
      "lineNumber": 92,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"测点编号\"",
      "translatedText": "COMMENT \"Measurement point number\""
    },
    {
      "lineNumber": 93,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"测点名\"",
      "translatedText": "COMMENT \"Measurement point name\""
    },
    {
      "lineNumber": 94,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备id\"",
      "translatedText": "COMMENT \"Device id\""
    },
    {
      "lineNumber": 95,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备名\"",
      "translatedText": "COMMENT \"Device name\""
    },
    {
      "lineNumber": 96,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"运行参数序号\"",
      "translatedText": "COMMENT \"Operating parameter serial number\""
    },
    {
      "lineNumber": 97,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备类型id\"",
      "translatedText": "COMMENT \"Device type id\""
    },
    {
      "lineNumber": 98,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"设备类型名\"",
      "translatedText": "COMMENT \"Device type name\""
    },
    {
      "lineNumber": 99,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"数据中心id\"",
      "translatedText": "COMMENT \"Data center id\""
    },
    {
      "lineNumber": 100,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"数据中心名\"",
      "translatedText": "COMMENT \"Data center name\""
    },
    {
      "lineNumber": 101,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"记录创建时间，无业务含义\"",
      "translatedText": "COMMENT \"Record creation time, no business meaning\""
    },
    {
      "lineNumber": 103,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT \"device_id冗余，提高统计效率用，需要保证数据正确（attr和device多对一）\"",
      "translatedText": "COMMENT \"device_id redundancy, used to improve statistical efficiency, need to ensure data correctness (attr and device are one-to-many)\""
    },
    {
      "lineNumber": 109,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT=\"有效性运行参数统计信息表\"",
      "translatedText": "COMMENT=\"Validity operating parameter statistics table\""
    },
    {
      "lineNumber": 122,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 台账导出相关",
      "translatedText": "-- Ledger export related"
    },
    {
      "lineNumber": 125,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '导出人'",
      "translatedText": "COMMENT 'Exporter'"
    },
    {
      "lineNumber": 126,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '文件名'",
      "translatedText": "COMMENT 'File name'"
    },
    {
      "lineNumber": 127,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '文件扩展名'",
      "translatedText": "COMMENT 'File extension'"
    },
    {
      "lineNumber": 128,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '文件保存在minio中的objectName'",
      "translatedText": "COMMENT 'The objectName of the file saved in minio'"
    },
    {
      "lineNumber": 129,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '状态'",
      "translatedText": "COMMENT 'Status'"
    },
    {
      "lineNumber": 130,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '类型，1：完整性问题；2：有效性问题，3：异常测点数据；4：所有测点数据'",
      "translatedText": "COMMENT 'Type, 1: Completeness issues; 2: Validity issues, 3: Abnormal measurement point data; 4: All measurement point data'"
    },
    {
      "lineNumber": 131,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '筛选条件入参json'",
      "translatedText": "COMMENT 'Filter condition input parameter json'"
    },
    {
      "lineNumber": 132,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '筛选条件入参描述'",
      "translatedText": "COMMENT 'Filter condition input parameter description'"
    },
    {
      "lineNumber": 133,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '导出记录创建时间'",
      "translatedText": "COMMENT 'Export record creation time'"
    },
    {
      "lineNumber": 134,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '文件生成时间'",
      "translatedText": "COMMENT 'File generation time'"
    },
    {
      "lineNumber": 135,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '下载时间'",
      "translatedText": "COMMENT 'Download time'"
    },
    {
      "lineNumber": 138,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT='运行参数采集导出记录表'",
      "translatedText": "COMMENT='Operating parameter collection export record table'"
    },
    {
      "lineNumber": 145,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT='运行参数采集数据导出状态'",
      "translatedText": "COMMENT='Operating parameter collection data export status'"
    },
    {
      "lineNumber": 150,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES\n  (-1, 'generate_failure', '生成失败'),",
      "translatedText": "VALUES\n  (-1, 'generate_failure', 'Generation failed'),"
    },
    {
      "lineNumber": 151,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (1, 'to_generate', '待生成'),",
      "translatedText": "  (1, 'to_generate', 'To be generated'),"
    },
    {
      "lineNumber": 152,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (2, 'to_download', '待下载'),",
      "translatedText": "  (2, 'to_download', 'To be downloaded'),"
    },
    {
      "lineNumber": 153,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "  (3, 'have_downloaded', '已下载');",
      "translatedText": "  (3, 'have_downloaded', 'Downloaded');"
    }
  ]
}
```

### DEMOS5

#### Input:

```sql
-- ----------------------------
-- Records of sys_field_mapping
-- ----------------------------
DELETE FROM cbms.`sys_field_mapping` WHERE `id` <= 10;
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (1, 'locationId', '机柜位置', 'CABINET', 0);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (2, 'name', '机柜编号', 'CABINET', 1);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (3, 'width', '机柜宽', 'CABINET', 1);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (4, 'length', '机柜深', 'CABINET', 1);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (5, 'capacityU', '机柜U数', 'CABINET', 1);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (6, 'description', '备注', 'CABINET', 1);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (7, 'attrStatusName', '单电源设备情况', 'CABINET', 1);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (8, 'locationFullName', '机柜位置', 'CABINET', 1);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (9, 'roomId', '机房', 'CABINET', 0);
INSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (10, 'dcId', '数据中心', 'CABINET', 0);

-- ----------------------------
-- Records of sys_operate_source_dict
-- ----------------------------
DELETE FROM cbms.`sys_operate_source_dict` WHERE `id` <= 2;
INSERT INTO cbms.`sys_operate_source_dict` VALUES (1, 'IMPORT', '导入');
INSERT INTO cbms.`sys_operate_source_dict` VALUES (2, 'UI_EDIT', '页面编辑');

-- ----------------------------
-- Records of sys_operate_type_dict
-- ----------------------------
DELETE FROM cbms.`sys_operate_type_dict` WHERE `id` <= 3;
INSERT INTO cbms.`sys_operate_type_dict` VALUES (1, 'CREATE', '新建');
INSERT INTO cbms.`sys_operate_type_dict` VALUES (2, 'UPDATE', '编辑');
INSERT INTO cbms.`sys_operate_type_dict` VALUES (3, 'DELETE', '删除');

-- ----------------------------
-- Records of auth_function
-- ----------------------------
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3001, 'dcim.function.dataedit.cabinet.add', '新建机柜', 1, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3002, 'dcim.function.dataedit.cabinet.delete', '删除机柜', 2, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3003, 'dcim.function.dataedit.cabinet.edit', '机柜编辑', 3, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3004, 'dcim.function.dataedit.cabinet.export', '机柜导出', 4, 2120, 2);

INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3005, 'dcim.function.dataedit.cabinet.import', '机柜导入', 5, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3006, 'dcim.function.dataedit.cabinet.import.template', '机柜导入模板下载', 6, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3007, 'dcim.function.dataedit.cabinet.import.upload', '机柜模板上传', 7, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3008, 'dcim.function.dataedit.cabinet.import.download', '机柜导入预览数据下载', 8, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3009, 'dcim.function.dataedit.cabinet.import.cancel', '机柜导入取消', 9, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3010, 'dcim.function.dataedit.cabinet.import.commit', '机柜导入提交', 10, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3011, 'dcim.function.dataedit.cabinet.import.edit', '机柜导入数据编辑', 11, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3012, 'dcim.function.dataedit.cabinet.import.delete', '机柜导入数据删除', 12, 2120, 2);

INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3013, 'dcim.function.dataedit.cabinet.attr.add', '添加机柜配电', 13, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3014, 'dcim.function.dataedit.cabinet.attr.delete', '删除机柜配电', 14, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3015, 'dcim.function.dataedit.cabinet.attr.edit', '编辑机柜配电', 15, 2120, 2);

INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3016, 'dcim.function.dataedit.cabinet.attr.import', '机柜配电导入', 16, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3017, 'dcim.function.dataedit.cabinet.attr.import.template', '机柜配电导入模板下载', 17, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3018, 'dcim.function.dataedit.cabinet.attr.import.upload', '机柜配电模板上传', 18, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3019, 'dcim.function.dataedit.cabinet.attr.import.download', '机柜配电导入预览数据下载', 19, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3020, 'dcim.function.dataedit.cabinet.attr.import.cancel', '机柜配电导入取消', 20, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3021, 'dcim.function.dataedit.cabinet.attr.import.commit', '机柜配电导入提交', 21, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3022, 'dcim.function.dataedit.cabinet.attr.import.edit', '机柜配电导入数据编辑', 22, 2120, 2);
INSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3023, 'dcim.function.dataedit.cabinet.attr.import.delete', '机柜配电导入数据删除', 23, 2120, 2);

-- ----------------------------
-- Records of auth_authority
-- ----------------------------
DELETE FROM dcim.`auth_authority` WHERE `authority_id` >= 40 AND `authority_id` <= 43;
INSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (40, '机柜配置查看权限', 40);
INSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (41, '机柜配置编辑权限', 41);
INSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (42, '机柜批量导入权限', 42);
INSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (43, '机柜配电批量导入权限', 43);

-- ----------------------------
-- Records of auth_role_authority
-- ----------------------------
-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 40); 原本就有
INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 41);
INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 42);
INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 43);
INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (1, 40);
-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 40); 原本就有
INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 41);
INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 42);


-- ----------------------------
-- Records of auth_authority_function
-- ----------------------------
-- auth-function
-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2000);   原本就有
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2100);
-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2120);  原本就有
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 3004);

INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 2120);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3001);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3002);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3003);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3004);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3013);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3014);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3015);

INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 2120);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3004);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3005);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3006);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3007);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3008);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3009);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3010);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3011);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3012);

INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 2120);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3004);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3016);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3017);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3018);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3019);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3020);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3021);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3022);
INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3023);
```

#### Output:

```json
{
  "translatedCode": "-- ----------------------------\n-- Records of sys_field_mapping\n-- ----------------------------\nDELETE FROM cbms.`sys_field_mapping` WHERE `id` <= 10;\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (1, 'locationId', 'Cabinet Location', 'CABINET', 0);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (2, 'name', 'Cabinet Number', 'CABINET', 1);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (3, 'width', 'Cabinet Width', 'CABINET', 1);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (4, 'length', 'Cabinet Depth', 'CABINET', 1);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (5, 'capacityU', 'Cabinet U Count', 'CABINET', 1);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (6, 'description', 'Remarks', 'CABINET', 1);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (7, 'attrStatusName', 'Single Power Supply Device Status', 'CABINET', 1);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (8, 'locationFullName', 'Cabinet Location', 'CABINET', 1);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (9, 'roomId', 'Machine Room', 'CABINET', 0);\nINSERT INTO cbms.`sys_field_mapping`(`id`, `code`, `name`, `biz_belong`, `is_show`) VALUES (10, 'dcId', 'Data Center', 'CABINET', 0);\n\n-- ----------------------------\n-- Records of sys_operate_source_dict\n-- ----------------------------\nDELETE FROM cbms.`sys_operate_source_dict` WHERE `id` <= 2;\nINSERT INTO cbms.`sys_operate_source_dict` VALUES (1, 'IMPORT', 'Import');\nINSERT INTO cbms.`sys_operate_source_dict` VALUES (2, 'UI_EDIT', 'Page Edit');\n\n-- ----------------------------\n-- Records of sys_operate_type_dict\n-- ----------------------------\nDELETE FROM cbms.`sys_operate_type_dict` WHERE `id` <= 3;\nINSERT INTO cbms.`sys_operate_type_dict` VALUES (1, 'CREATE', 'Create');\nINSERT INTO cbms.`sys_operate_type_dict` VALUES (2, 'UPDATE', 'Edit');\nINSERT INTO cbms.`sys_operate_type_dict` VALUES (3, 'DELETE', 'Delete');\n\n-- ----------------------------\n-- Records of auth_function\n-- ----------------------------\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3001, 'dcim.function.dataedit.cabinet.add', 'Create Cabinet', 1, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3002, 'dcim.function.dataedit.cabinet.delete', 'Delete Cabinet', 2, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3003, 'dcim.function.dataedit.cabinet.edit', 'Edit Cabinet', 3, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3004, 'dcim.function.dataedit.cabinet.export', 'Export Cabinet', 4, 2120, 2);\n\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3005, 'dcim.function.dataedit.cabinet.import', 'Import Cabinet', 5, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3006, 'dcim.function.dataedit.cabinet.import.template', 'Download Cabinet Import Template', 6, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3007, 'dcim.function.dataedit.cabinet.import.upload', 'Upload Cabinet Template', 7, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3008, 'dcim.function.dataedit.cabinet.import.download', 'Download Cabinet Import Preview Data', 8, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3009, 'dcim.function.dataedit.cabinet.import.cancel', 'Cancel Cabinet Import', 9, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3010, 'dcim.function.dataedit.cabinet.import.commit', 'Submit Cabinet Import', 10, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3011, 'dcim.function.dataedit.cabinet.import.edit', 'Edit Cabinet Import Data', 11, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3012, 'dcim.function.dataedit.cabinet.import.delete', 'Delete Cabinet Import Data', 12, 2120, 2);\n\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3013, 'dcim.function.dataedit.cabinet.attr.add', 'Add Cabinet Power Distribution', 13, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3014, 'dcim.function.dataedit.cabinet.attr.delete', 'Delete Cabinet Power Distribution', 14, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3015, 'dcim.function.dataedit.cabinet.attr.edit', 'Edit Cabinet Power Distribution', 15, 2120, 2);\n\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3016, 'dcim.function.dataedit.cabinet.attr.import', 'Import Cabinet Power Distribution', 16, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3017, 'dcim.function.dataedit.cabinet.attr.import.template', 'Download Cabinet Power Distribution Import Template', 17, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3018, 'dcim.function.dataedit.cabinet.attr.import.upload', 'Upload Cabinet Power Distribution Template', 18, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3019, 'dcim.function.dataedit.cabinet.attr.import.download', 'Download Cabinet Power Distribution Import Preview Data', 19, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3020, 'dcim.function.dataedit.cabinet.attr.import.cancel', 'Cancel Cabinet Power Distribution Import', 20, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3021, 'dcim.function.dataedit.cabinet.attr.import.commit', 'Submit Cabinet Power Distribution Import', 21, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3022, 'dcim.function.dataedit.cabinet.attr.import.edit', 'Edit Cabinet Power Distribution Import Data', 22, 2120, 2);\nINSERT INTO `dcim`.`auth_function`(`function_id`, `function_code`, `name`, `order_by`, `parent_id`, `type`) VALUES (3023, 'dcim.function.dataedit.cabinet.attr.import.delete', 'Delete Cabinet Power Distribution Import Data', 23, 2120, 2);\n\n-- ----------------------------\n-- Records of auth_authority\n-- ----------------------------\nDELETE FROM dcim.`auth_authority` WHERE `authority_id` >= 40 AND `authority_id` <= 43;\nINSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (40, 'Cabinet Configuration View Permission', 40);\nINSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (41, 'Cabinet Configuration Edit Permission', 41);\nINSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (42, 'Cabinet Batch Import Permission', 42);\nINSERT INTO dcim.`auth_authority`(`authority_id`, `name`, `order_by`) VALUES (43, 'Cabinet Power Distribution Batch Import Permission', 43);\n\n-- ----------------------------\n-- Records of auth_role_authority\n-- ----------------------------\n-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 40); Originally there\nINSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 41);\nINSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 42);\nINSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 43);\nINSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (1, 40);\n-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 40); Originally there\nINSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 41);\nINSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 42);\n\n-- ----------------------------\n-- Records of auth_authority_function\n-- ----------------------------\n-- auth-function\n-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2000); Originally there\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2100);\n-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2120); Originally there\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 3004);\n\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 2120);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3001);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3002);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3003);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3004);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3013);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3014);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (41, 3015);\n\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 2120);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3004);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3005);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3006);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3007);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3008);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3009);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3010);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3011);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (42, 3012);\n\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 2120);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3004);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3016);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3017);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3018);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3019);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3020);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3021);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3022);\nINSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (43, 3023);",
  "details": [
    {
      "lineNumber": 5,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (1, 'locationId', '机柜位置', 'CABINET', 0);",
      "translatedText": "VALUES (1, 'locationId', 'Cabinet Location', 'CABINET', 0);"
    },
    {
      "lineNumber": 6,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (2, 'name', '机柜编号', 'CABINET', 1);",
      "translatedText": "VALUES (2, 'name', 'Cabinet Number', 'CABINET', 1);"
    },
    {
      "lineNumber": 7,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3, 'width', '机柜宽', 'CABINET', 1);",
      "translatedText": "VALUES (3, 'width', 'Cabinet Width', 'CABINET', 1);"
    },
    {
      "lineNumber": 8,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (4, 'length', '机柜深', 'CABINET', 1);",
      "translatedText": "VALUES (4, 'length', 'Cabinet Depth', 'CABINET', 1);"
    },
    {
      "lineNumber": 9,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (5, 'capacityU', '机柜U数', 'CABINET', 1);",
      "translatedText": "VALUES (5, 'capacityU', 'Cabinet U Count', 'CABINET', 1);"
    },
    {
      "lineNumber": 10,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (6, 'description', '备注', 'CABINET', 1);",
      "translatedText": "VALUES (6, 'description', 'Remarks', 'CABINET', 1);"
    },
    {
      "lineNumber": 11,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (7, 'attrStatusName', '单电源设备情况', 'CABINET', 1);",
      "translatedText": "VALUES (7, 'attrStatusName', 'Single Power Supply Device Status', 'CABINET', 1);"
    },
    {
      "lineNumber": 12,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (8, 'locationFullName', '机柜位置', 'CABINET', 1);",
      "translatedText": "VALUES (8, 'locationFullName', 'Cabinet Location', 'CABINET', 1);"
    },
    {
      "lineNumber": 13,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (9, 'roomId', '机房', 'CABINET', 0);",
      "translatedText": "VALUES (9, 'roomId', 'Machine Room', 'CABINET', 0);"
    },
    {
      "lineNumber": 14,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (10, 'dcId', '数据中心', 'CABINET', 0);",
      "translatedText": "VALUES (10, 'dcId', 'Data Center', 'CABINET', 0);"
    },
    {
      "lineNumber": 20,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (1, 'IMPORT', '导入');",
      "translatedText": "VALUES (1, 'IMPORT', 'Import');"
    },
    {
      "lineNumber": 21,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (2, 'UI_EDIT', '页面编辑');",
      "translatedText": "VALUES (2, 'UI_EDIT', 'Page Edit');"
    },
    {
      "lineNumber": 27,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (1, 'CREATE', '新建');",
      "translatedText": "VALUES (1, 'CREATE', 'Create');"
    },
    {
      "lineNumber": 28,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (2, 'UPDATE', '编辑');",
      "translatedText": "VALUES (2, 'UPDATE', 'Edit');"
    },
    {
      "lineNumber": 29,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3, 'DELETE', '删除');",
      "translatedText": "VALUES (3, 'DELETE', 'Delete');"
    },
    {
      "lineNumber": 34,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3001, 'dcim.function.dataedit.cabinet.add', '新建机柜', 1, 2120, 2);",
      "translatedText": "VALUES (3001, 'dcim.function.dataedit.cabinet.add', 'Create Cabinet', 1, 2120, 2);"
    },
    {
      "lineNumber": 35,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3002, 'dcim.function.dataedit.cabinet.delete', '删除机柜', 2, 2120, 2);",
      "translatedText": "VALUES (3002, 'dcim.function.dataedit.cabinet.delete', 'Delete Cabinet', 2, 2120, 2);"
    },
    {
      "lineNumber": 36,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3003, 'dcim.function.dataedit.cabinet.edit', '机柜编辑', 3, 2120, 2);",
      "translatedText": "VALUES (3003, 'dcim.function.dataedit.cabinet.edit', 'Edit Cabinet', 3, 2120, 2);"
    },
    {
      "lineNumber": 37,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3004, 'dcim.function.dataedit.cabinet.export', '机柜导出', 4, 2120, 2);",
      "translatedText": "VALUES (3004, 'dcim.function.dataedit.cabinet.export', 'Export Cabinet', 4, 2120, 2);"
    },
    {
      "lineNumber": 39,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3005, 'dcim.function.dataedit.cabinet.import', '机柜导入', 5, 2120, 2);",
      "translatedText": "VALUES (3005, 'dcim.function.dataedit.cabinet.import', 'Import Cabinet', 5, 2120, 2);"
    },
    {
      "lineNumber": 40,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3006, 'dcim.function.dataedit.cabinet.import.template', '机柜导入模板下载', 6, 2120, 2);",
      "translatedText": "VALUES (3006, 'dcim.function.dataedit.cabinet.import.template', 'Download Cabinet Import Template', 6, 2120, 2);"
    },
    {
      "lineNumber": 41,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3007, 'dcim.function.dataedit.cabinet.import.upload', '机柜模板上传', 7, 2120, 2);",
      "translatedText": "VALUES (3007, 'dcim.function.dataedit.cabinet.import.upload', 'Upload Cabinet Template', 7, 2120, 2);"
    },
    {
      "lineNumber": 42,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3008, 'dcim.function.dataedit.cabinet.import.download', '机柜导入预览数据下载', 8, 2120, 2);",
      "translatedText": "VALUES (3008, 'dcim.function.dataedit.cabinet.import.download', 'Download Cabinet Import Preview Data', 8, 2120, 2);"
    },
    {
      "lineNumber": 43,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3009, 'dcim.function.dataedit.cabinet.import.cancel', '机柜导入取消', 9, 2120, 2);",
      "translatedText": "VALUES (3009, 'dcim.function.dataedit.cabinet.import.cancel', 'Cancel Cabinet Import', 9, 2120, 2);"
    },
    {
      "lineNumber": 44,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3010, 'dcim.function.dataedit.cabinet.import.commit', '机柜导入提交', 10, 2120, 2);",
      "translatedText": "VALUES (3010, 'dcim.function.dataedit.cabinet.import.commit', 'Submit Cabinet Import', 10, 2120, 2);"
    },
    {
      "lineNumber": 45,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3011, 'dcim.function.dataedit.cabinet.import.edit', '机柜导入数据编辑', 11, 2120, 2);",
      "translatedText": "VALUES (3011, 'dcim.function.dataedit.cabinet.import.edit', 'Edit Cabinet Import Data', 11, 2120, 2);"
    },
    {
      "lineNumber": 46,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3012, 'dcim.function.dataedit.cabinet.import.delete', '机柜导入数据删除', 12, 2120, 2);",
      "translatedText": "VALUES (3012, 'dcim.function.dataedit.cabinet.import.delete', 'Delete Cabinet Import Data', 12, 2120, 2);"
    },
    {
      "lineNumber": 48,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3013, 'dcim.function.dataedit.cabinet.attr.add', '添加机柜配电', 13, 2120, 2);",
      "translatedText": "VALUES (3013, 'dcim.function.dataedit.cabinet.attr.add', 'Add Cabinet Power Distribution', 13, 2120, 2);"
    },
    {
      "lineNumber": 49,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3014, 'dcim.function.dataedit.cabinet.attr.delete', '删除机柜配电', 14, 2120, 2);",
      "translatedText": "VALUES (3014, 'dcim.function.dataedit.cabinet.attr.delete', 'Delete Cabinet Power Distribution', 14, 2120, 2);"
    },
    {
      "lineNumber": 50,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3015, 'dcim.function.dataedit.cabinet.attr.edit', '编辑机柜配电', 15, 2120, 2);",
      "translatedText": "VALUES (3015, 'dcim.function.dataedit.cabinet.attr.edit', 'Edit Cabinet Power Distribution', 15, 2120, 2);"
    },
    {
      "lineNumber": 52,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3016, 'dcim.function.dataedit.cabinet.attr.import', '机柜配电导入', 16, 2120, 2);",
      "translatedText": "VALUES (3016, 'dcim.function.dataedit.cabinet.attr.import', 'Import Cabinet Power Distribution', 16, 2120, 2);"
    },
    {
      "lineNumber": 53,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3017, 'dcim.function.dataedit.cabinet.attr.import.template', '机柜配电导入模板下载', 17, 2120, 2);",
      "translatedText": "VALUES (3017, 'dcim.function.dataedit.cabinet.attr.import.template', 'Download Cabinet Power Distribution Import Template', 17, 2120, 2);"
    },
    {
      "lineNumber": 54,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3018, 'dcim.function.dataedit.cabinet.attr.import.upload', '机柜配电模板上传', 18, 2120, 2);",
      "translatedText": "VALUES (3018, 'dcim.function.dataedit.cabinet.attr.import.upload', 'Upload Cabinet Power Distribution Template', 18, 2120, 2);"
    },
    {
      "lineNumber": 55,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3019, 'dcim.function.dataedit.cabinet.attr.import.download', '机柜配电导入预览数据下载', 19, 2120, 2);",
      "translatedText": "VALUES (3019, 'dcim.function.dataedit.cabinet.attr.import.download', 'Download Cabinet Power Distribution Import Preview Data', 19, 2120, 2);"
    },
    {
      "lineNumber": 56,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3020, 'dcim.function.dataedit.cabinet.attr.import.cancel', '机柜配电导入取消', 20, 2120, 2);",
      "translatedText": "VALUES (3020, 'dcim.function.dataedit.cabinet.attr.import.cancel', 'Cancel Cabinet Power Distribution Import', 20, 2120, 2);"
    },
    {
      "lineNumber": 57,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3021, 'dcim.function.dataedit.cabinet.attr.import.commit', '机柜配电导入提交', 21, 2120, 2);",
      "translatedText": "VALUES (3021, 'dcim.function.dataedit.cabinet.attr.import.commit', 'Submit Cabinet Power Distribution Import', 21, 2120, 2);"
    },
    {
      "lineNumber": 58,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3022, 'dcim.function.dataedit.cabinet.attr.import.edit', '机柜配电导入数据编辑', 22, 2120, 2);",
      "translatedText": "VALUES (3022, 'dcim.function.dataedit.cabinet.attr.import.edit', 'Edit Cabinet Power Distribution Import Data', 22, 2120, 2);"
    },
    {
      "lineNumber": 59,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (3023, 'dcim.function.dataedit.cabinet.attr.import.delete', '机柜配电导入数据删除', 23, 2120, 2);",
      "translatedText": "VALUES (3023, 'dcim.function.dataedit.cabinet.attr.import.delete', 'Delete Cabinet Power Distribution Import Data', 23, 2120, 2);"
    },
    {
      "lineNumber": 65,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (40, '机柜配置查看权限', 40);",
      "translatedText": "VALUES (40, 'Cabinet Configuration View Permission', 40);"
    },
    {
      "lineNumber": 66,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (41, '机柜配置编辑权限', 41);",
      "translatedText": "VALUES (41, 'Cabinet Configuration Edit Permission', 41);"
    },
    {
      "lineNumber": 67,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (42, '机柜批量导入权限', 42);",
      "translatedText": "VALUES (42, 'Cabinet Batch Import Permission', 42);"
    },
    {
      "lineNumber": 68,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "VALUES (43, '机柜配电批量导入权限', 43);",
      "translatedText": "VALUES (43, 'Cabinet Power Distribution Batch Import Permission', 43);"
    },
    {
      "lineNumber": 73,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 40); 原本就有",
      "translatedText": "-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (0, 40); Originally there"
    },
    {
      "lineNumber": 78,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 40); 原本就有",
      "-- INSERT INTO `dcim`.`auth_role_authority`(`role_id`, `authority_id`) VALUES (4, 40); Originally there"
    },
    {
      "lineNumber": 87,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2000);   原本就有",
      "translatedText": "-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2000); Originally had"
    },
    {
      "lineNumber": 89,
      "lineType": "values",
      "jobType": "Text Translation",
      "originalText": "-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2120);  原本就有",
      "translatedText": "-- INSERT INTO `dcim`.`auth_authority_function`(`authority_id`, `function_id`) VALUES (40, 2120); Originally there"
    }
  ]
}
```

### DEMOS6

#### Input:

```sql
-- 默认值变更
UPDATE cbms.sys_cabinet SET attr_status = null WHERE attr_status = 0;
ALTER TABLE `cbms`.`sys_cabinet` MODIFY COLUMN `attr_status` tinyint(1) NULL DEFAULT NULL COMMENT '单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在';

-- 资产电路类型 0: A路, 1: B路, 2: 冗余 ====转换(方便计算)====> 电路类型 0: 冗余, 1: A路, 2: B路
-- 机柜单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在
-- sum == 0 => 无(0)
-- sum == 1 => 仅A(1)
-- sum == 2 => 仅B(2)
-- sum == 3 => AB(3)
UPDATE cbms.sys_cabinet c JOIN
    (
        SELECT
            t.cabinet_id,
            SUM(t.convert_type) AS attr_status
        FROM
            (
                SELECT
                    cab.cabinet_id,
                    it.elect_type,
                    CASE it.elect_type WHEN 2 THEN 0 WHEN 0 THEN 1 WHEN 1 THEN 2 else null END AS convert_type,
                    COUNT( 0 ) AS num
                FROM
                    cbms.sys_cabinet cab
                        LEFT JOIN asset.asset_it it ON cab.cabinet_id = it.cabinet_id
                        LEFT JOIN asset.asset ast ON it.asset_id = ast.id
                WHERE
                        it.is_delete = 0 AND it.elect_type is not null AND it.u_from_vertical > 0 AND ast.`status` = 0
                GROUP BY
                    cab.cabinet_id, it.elect_type
            ) t
        GROUP BY t.cabinet_id
    ) AS f
    ON c.cabinet_id = f.cabinet_id
SET c.attr_status = f.attr_status;
```

#### Output:

```json
{
  "translatedCode": "-- Default value change\nUPDATE cbms.sys_cabinet SET attr_status = null WHERE attr_status = 0;\nALTER TABLE `cbms`.`sys_cabinet` MODIFY COLUMN `attr_status` tinyint(1) NULL DEFAULT NULL COMMENT 'Single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist';\n\n-- Asset circuit type 0: A-line, 1: B-line, 2: Redundant ====Conversion(for easy calculation)====> Circuit type 0: Redundant, 1: A-line, 2: B-line\n-- Cabinet single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist\n-- sum == 0 => None(0)\n-- sum == 1 => Only A(1)\n-- sum == 2 => Only B(2)\n-- sum == 3 => AB(3)\nUPDATE cbms.sys_cabinet c JOIN\n    (\n        SELECT\n            t.cabinet_id,\n            SUM(t.convert_type) AS attr_status\n        FROM\n            (\n                SELECT\n                    cab.cabinet_id,\n                    it.elect_type,\n                    CASE it.elect_type WHEN 2 THEN 0 WHEN 0 THEN 1 WHEN 1 THEN 2 else null END AS convert_type,\n                    COUNT( 0 ) AS num\n                FROM\n                    cbms.sys_cabinet cab\n                        LEFT JOIN asset.asset_it it ON cab.cabinet_id = it.cabinet_id\n                        LEFT JOIN asset.asset ast ON it.asset_id = ast.id\n                WHERE\n                        it.is_delete = 0 AND it.elect_type is not null AND it.u_from_vertical > 0 AND ast.`status` = 0\n                GROUP BY\n                    cab.cabinet_id, it.elect_type\n            ) t\n        GROUP BY t.cabinet_id\n    ) AS f\n    ON c.cabinet_id = f.cabinet_id\nSET c.attr_status = f.attr_status;",
  "details": [
    {
      "lineNumber": 1,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 默认值变更",
      "translatedText": "-- Default value change"
    },
    {
      "lineNumber": 3,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "COMMENT '单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在'",
      "translatedText": "COMMENT 'Single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist'"
    },
    {
      "lineNumber": 5,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 资产电路类型 0: A路, 1: B路, 2: 冗余 ====转换(方便计算)====> 电路类型 0: 冗余, 1: A路, 2: B路",
      "translatedText": "-- Asset circuit type 0: A-line, 1: B-line, 2: Redundant ====Conversion(for easy calculation)====> Circuit type 0: Redundant, 1: A-line, 2: B-line"
    },
    {
      "lineNumber": 6,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- 机柜单电源设备情况 0-无 1-仅A路存在电源设备 2-仅B路 3-AB路都存在",
      "translatedText": "-- Cabinet single power supply device status 0-None 1-Only A-line exists 2-Only B-line 3-Both A and B lines exist"
    },
    {
      "lineNumber": 7,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- sum == 0 => 无(0)",
      "translatedText": "-- sum == 0 => None(0)"
    },
    {
      "lineNumber": 8,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "-- sum == 1 => 仅A(1)",
      "translatedText": "-- sum == 1 => Only A(1)"
    },
    {
      "lineNumber": 9,
      "lineType": "comment",
      "jobType": "Text
      "originalText": "-- sum == 2 => 仅B(2)",
      "translatedText": "-- sum == 2 => Only B(2)"
    },
  ]
}
```