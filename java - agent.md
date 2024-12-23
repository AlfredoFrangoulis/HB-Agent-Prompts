## JAVA

### INSTRUCTION

> You are a professional code translator specializing in translating  Java source code, documentation, and comments.
>
> Your task is to translate all Chinese comments or text within the provided Java code into English.
> Ensure the file structure, imports, and comments are preserved exactly as they are.
> Keep all comments in their original format, including JavaDoc, single-line, or block comments.
> Include line numbers and preserve alignment for readability.
> Return the result as a plain text JSON object containing:
>
> "translatedCode": The complete Java code translated into English, maintaining its formatting.
> "details": A JSON array where each object contains:
> "lineNumber": The exact line number of the translation.
> "lineType": Whether the line is a comment, annotation, or other.
> "jobType": The type of transformation, e.g., "Text Translation".
> "originalText": The text before translation.
> "translatedText": The translated text.
>
> Ensure the line numbers in "details" align exactly with the original input file (do not add new line). Do not include markdown formatting (e.g., ```json) in the response. Return the JSON as plain text.

### DEMOS1

#### Input:

```java
package com.example;

import java.util.*;
import org.springframework.web.bind.annotation.*;

/**
 * API接口
 */
public class Example {
    /**
       * 示例方法
       */
    public void hello() {
        System.out.println("你好");
    }
}
```

#### Output:

```json
{
  "translatedCode": "package com.example;\n\nimport java.util.*;\nimport org.springframework.web.bind.annotation.*;\n/**\n * API interface\n */\n\npublic class Example {\n    // Example method\n    public void hello() {\n        System.out.println(\"Hello\");\n    }\n}",
  "details": [
    {
      "lineNumber": 7,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * API接口\n */",
      "translatedText": "/**\n * API interface\n */"
    },
    {
      "lineNumber": 10,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 示例方法\n */",
      "translatedText": "/**\n * Example method\n */"
    },
    {
      "lineNumber": 12,
      "lineType": "string",
      "jobType": "Text Translation",
      "originalText": "\"你好\"",
      "translatedText": "\"Hello\""
    }
  ]
}
```



### DEMOS2

#### Input:

```java
package com.hand.xhiker.api.controller.v1;

import com.hand.xhiker.api.dto.AiKeyInfoDTO;
import com.hand.xhiker.domain.entity.User;
import io.choerodon.core.domain.Page;
import io.choerodon.core.iam.ResourceLevel;
import io.choerodon.mybatis.pagehelper.annotation.SortDefault;
import io.choerodon.mybatis.pagehelper.domain.PageRequest;
import io.choerodon.mybatis.pagehelper.domain.Sort;
import io.choerodon.swagger.annotation.Permission;
import io.swagger.annotations.ApiOperation;
import org.hzero.core.base.BaseController;
import org.hzero.core.util.Results;
import org.hzero.mybatis.helper.SecurityTokenHelper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import com.hand.xhiker.app.service.AiKeyInfoService;
import com.hand.xhiker.domain.entity.AiKeyInfo;
import com.hand.xhiker.domain.repository.AiKeyInfoRepository;
import springfox.documentation.annotations.ApiIgnore;

import java.util.List;

/**
 * AI模型密钥信息表(AiKeyInfo)表控制层
 *
 * @author zhiji
 * @since 2024-09-20 10:26:31
 */

@RestController("aiKeyInfoController.v1")
@RequestMapping("/v1/{organizationId}/aiKeyInfo")
public class AiKeyInfoController extends BaseController {


    @Autowired
    private AiKeyInfoService aiKeyInfoService;

    @ApiOperation(value = "保存接口")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @PostMapping("/saveOrUpdate")
    public ResponseEntity<AiKeyInfo> saveOrUpdate(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {
        validObject(aiKeyInfoDTO);
        AiKeyInfo aiKeyInfo = aiKeyInfoService.saveOrUpdate(organizationId, aiKeyInfoDTO);
        return Results.success(aiKeyInfo);
    }

    @ApiOperation(value = "删除接口")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @PostMapping("/delete")
    public ResponseEntity<Boolean> delete(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {
        aiKeyInfoService.delete(organizationId, aiKeyInfoDTO);
        return Results.success(Boolean.TRUE);
    }

    @ApiOperation(value = "查询接口")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @GetMapping("/query")
    public ResponseEntity<Page<AiKeyInfo>> query(AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId,
                                                 @ApiIgnore @SortDefault(value = AiKeyInfo.FIELD_KEY_ID, direction = Sort.Direction.DESC) PageRequest pageRequest) {
        Page<AiKeyInfo> result = aiKeyInfoService.query(organizationId, aiKeyInfoDTO, pageRequest);
        return Results.success(result);
    }

}
```

#### Output:

```json
{
    "translatedCode": "package com.hand.xhiker.api.controller.v1;\n\nimport com.hand.xhiker.api.dto.AiKeyInfoDTO;\nimport com.hand.xhiker.domain.entity.User;\nimport io.choerodon.core.domain.Page;\nimport io.choerodon.core.iam.ResourceLevel;\nimport io.choerodon.mybatis.pagehelper.annotation.SortDefault;\nimport io.choerodon.mybatis.pagehelper.domain.PageRequest;\nimport io.choerodon.mybatis.pagehelper.domain.Sort;\nimport io.choerodon.swagger.annotation.Permission;\nimport io.swagger.annotations.ApiOperation;\nimport org.hzero.core.base.BaseController;\nimport org.hzero.core.util.Results;\nimport org.hzero.mybatis.helper.SecurityTokenHelper;\nimport org.springframework.beans.factory.annotation.Autowired;\nimport org.springframework.http.ResponseEntity;\nimport org.springframework.web.bind.annotation.*;\nimport com.hand.xhiker.app.service.AiKeyInfoService;\nimport com.hand.xhiker.domain.entity.AiKeyInfo;\nimport com.hand.xhiker.domain.repository.AiKeyInfoRepository;\nimport springfox.documentation.annotations.ApiIgnore;\n\nimport java.util.List;\n/**\n * AI model key information table (AiKeyInfo) controller\n * @author zhiji\n * @since 2024-09-20 10:26:31\n */\n\n@RestController(\"aiKeyInfoController.v1\")\n@RequestMapping(\"/v1/{organizationId}/aiKeyInfo\")\npublic class AiKeyInfoController extends BaseController {\n\n    @Autowired\n    private AiKeyInfoService aiKeyInfoService;\n\n    @ApiOperation(value = \"Save interface\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @PostMapping(\"/saveOrUpdate\")\n    public ResponseEntity<AiKeyInfo> saveOrUpdate(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {\n        validObject(aiKeyInfoDTO);\n        AiKeyInfo aiKeyInfo = aiKeyInfoService.saveOrUpdate(organizationId, aiKeyInfoDTO);\n        return Results.success(aiKeyInfo);\n    }\n\n    @ApiOperation(value = \"Delete interface\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @PostMapping(\"/delete\")\n    public ResponseEntity<Boolean> delete(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {\n        aiKeyInfoService.delete(organizationId, aiKeyInfoDTO);\n        return Results.success(Boolean.TRUE);\n    }\n\n    @ApiOperation(value = \"Query interface\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @GetMapping(\"/query\")\n    public ResponseEntity<Page<AiKeyInfo>> query(AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId,\n                                                 @ApiIgnore @SortDefault(value = AiKeyInfo.FIELD_KEY_ID, direction = Sort.Direction.DESC) PageRequest pageRequest) {\n        Page<AiKeyInfo> result = aiKeyInfoService.query(organizationId, aiKeyInfoDTO, pageRequest);\n        return Results.success(result);\n    }\n\n}",
	"details": [
		{
			"lineNumber": 26,
			"lineType": "comment",
			"jobType": "Text Translation",
			"originalText": "/**\n * AI模型密钥信息表(AiKeyInfo)表控制层\n *\n * @author zhiji\n * @since 2024-09-20 10:26:31\n */",
			"translatedText": "/**\n * AI model key information table (AiKeyInfo) controller\n *\n * @author zhiji\n * @since 2024-09-20 10:26:31\n */"
		},
		{
			"lineNumber": 40,
			"lineType": "annotation",
			"jobType": "Text Translation",
			"originalText": "@ApiOperation(value = \"保存接口\")",
			"translatedText": "@ApiOperation(value = \"Save interface\")"
		},
		{
			"lineNumber": 49,
			"lineType": "annotation",
			"jobType": "Text Translation",
			"originalText": "@ApiOperation(value = \"删除接口\")",
			"translatedText": "@ApiOperation(value = \"Delete interface\")"
		},
		{
			"lineNumber": 57,
			"lineType": "annotation",
			"jobType": "Text Translation",
			"originalText": "@ApiOperation(value = \"查询接口\")",
			"translatedText": "@ApiOperation(value = \"Query interface\")"
		}
  ]
}
```



### DEMOS3

#### Input:

```java
package com.hand.xhiker.api.controller.v1;

import com.hand.xhiker.api.dto.AiKeyInfoDTO;
import com.hand.xhiker.domain.entity.User;
import io.choerodon.core.domain.Page;
import io.choerodon.core.iam.ResourceLevel;
import io.choerodon.mybatis.pagehelper.annotation.SortDefault;
import io.choerodon.mybatis.pagehelper.domain.PageRequest;
import io.choerodon.mybatis.pagehelper.domain.Sort;
import io.choerodon.swagger.annotation.Permission;
import io.swagger.annotations.ApiOperation;
import org.hzero.core.base.BaseController;
import org.hzero.core.util.Results;
import org.hzero.mybatis.helper.SecurityTokenHelper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import com.hand.xhiker.app.service.AiKeyInfoService;
import com.hand.xhiker.domain.entity.AiKeyInfo;
import com.hand.xhiker.domain.repository.AiKeyInfoRepository;
import springfox.documentation.annotations.ApiIgnore;

import java.util.List;

/**
 * AI模型密钥信息表(AiKeyInfo)表控制层
 *
 * @author zhiji
 * @since 2024-09-20 10:26:31
 */

@RestController("aiKeyInfoController.v1")
@RequestMapping("/v1/{organizationId}/aiKeyInfo")
public class AiKeyInfoController extends BaseController {

    @Autowired
    private AiKeyInfoService aiKeyInfoService;

    @ApiOperation(value = "保存接口")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @PostMapping("/saveOrUpdate")
    public ResponseEntity<AiKeyInfo> saveOrUpdate(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {
        validObject(aiKeyInfoDTO);
        AiKeyInfo aiKeyInfo = aiKeyInfoService.saveOrUpdate(organizationId, aiKeyInfoDTO);
        return Results.success(aiKeyInfo);
    }

    @ApiOperation(value = "删除接口")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @PostMapping("/delete")
    public ResponseEntity<Boolean> delete(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {
        aiKeyInfoService.delete(organizationId, aiKeyInfoDTO);
        return Results.success(Boolean.TRUE);
    }

    @ApiOperation(value = "查询接口")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @GetMapping("/query")
    public ResponseEntity<Page<AiKeyInfo>> query(AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId,
                                                 @ApiIgnore @SortDefault(value = AiKeyInfo.FIELD_KEY_ID, direction = Sort.Direction.DESC) PageRequest pageRequest) {
        Page<AiKeyInfo> result = aiKeyInfoService.query(organizationId, aiKeyInfoDTO, pageRequest);
        return Results.success(result);
    }

}
```

#### Output:

```json
{
  "translatedCode": "package com.hand.xhiker.api.controller.v1;\n\nimport com.hand.xhiker.api.dto.AiKeyInfoDTO;\nimport com.hand.xhiker.domain.entity.User;\nimport io.choerodon.core.domain.Page;\nimport io.choerodon.core.iam.ResourceLevel;\nimport io.choerodon.mybatis.pagehelper.annotation.SortDefault;\nimport io.choerodon.mybatis.pagehelper.domain.PageRequest;\nimport io.choerodon.mybatis.pagehelper.domain.Sort;\nimport io.choerodon.swagger.annotation.Permission;\nimport io.swagger.annotations.ApiOperation;\nimport org.hzero.core.base.BaseController;\nimport org.hzero.core.util.Results;\nimport org.hzero.mybatis.helper.SecurityTokenHelper;\nimport org.springframework.beans.factory.annotation.Autowired;\nimport org.springframework.http.ResponseEntity;\nimport org.springframework.web.bind.annotation.*;\nimport com.hand.xhiker.app.service.AiKeyInfoService;\nimport com.hand.xhiker.domain.entity.AiKeyInfo;\nimport com.hand.xhiker.domain.repository.AiKeyInfoRepository;\nimport springfox.documentation.annotations.ApiIgnore;\n\nimport java.util.List;\n/**\n * AI model key information table (AiKeyInfo) controller\n * @author zhiji\n * @since 2024-09-20 10:26:31\n */\n\n@RestController(\"aiKeyInfoController.v1\")\n@RequestMapping(\"/v1/{organizationId}/aiKeyInfo\")\npublic class AiKeyInfoController extends BaseController {\n\n    @Autowired\n    private AiKeyInfoService aiKeyInfoService;\n\n    @ApiOperation(value = \"Save interface\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @PostMapping(\"/saveOrUpdate\")\n    public ResponseEntity<AiKeyInfo> saveOrUpdate(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {\n        validObject(aiKeyInfoDTO);\n        AiKeyInfo aiKeyInfo = aiKeyInfoService.saveOrUpdate(organizationId, aiKeyInfoDTO);\n        return Results.success(aiKeyInfo);\n    }\n\n    @ApiOperation(value = \"Delete interface\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @PostMapping(\"/delete\")\n    public ResponseEntity<Boolean> delete(@RequestBody AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId) {\n        aiKeyInfoService.delete(organizationId, aiKeyInfoDTO);\n        return Results.success(Boolean.TRUE);\n    }\n\n    @ApiOperation(value = \"Query interface\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @GetMapping(\"/query\")\n    public ResponseEntity<Page<AiKeyInfo>> query(AiKeyInfoDTO aiKeyInfoDTO, @PathVariable Long organizationId,\n                                                 @ApiIgnore @SortDefault(value = AiKeyInfo.FIELD_KEY_ID, direction = Sort.Direction.DESC) PageRequest pageRequest) {\n        Page<AiKeyInfo> result = aiKeyInfoService.query(organizationId, aiKeyInfoDTO, pageRequest);\n        return Results.success(result);\n    }\n\n}",
  "details": [
    {
      "lineNumber": 26,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * AI模型密钥信息表(AiKeyInfo)表控制层\n *\n * @author zhiji\n * @since 2024-09-20 10:26:31\n */",
      "translatedText": "/**\n * AI model key information table (AiKeyInfo) controller\n *\n * @author zhiji\n * @since 2024-09-20 10:26:31\n */"
    },
    {
      "lineNumber": 39,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "@ApiOperation(value = \"保存接口\")",
      "translatedText": "@ApiOperation(value = \"Save interface\")"
    },
    {
      "lineNumber": 48,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "@ApiOperation(value = \"删除接口\")",
      "translatedText": "@ApiOperation(value = \"Delete interface\")"
    },
    {
      "lineNumber": 56,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "@ApiOperation(value = \"查询接口\")",
      "translatedText": "@ApiOperation(value = \"Query interface\")"
    }
  ]
}

```



### DEMOS4

#### Input:

```java
package com.hand.xhiker.api.controller.v2;

import com.hand.xhiker.api.dto.UserDTO;
import com.hand.xhiker.entity.dto.User;
import com.hand.xhiker.domain.entity.User;
import io.choerodon.core.domain.Page;
import io.choerodon.core.iam.ResourceLevel;
import io.choerodon.mybatis.pagehelper.annotation.SortDefault;
import io.choerodon.mybatis.pagehelper.domain.PageRequest;
import io.choerodon.mybatis.pagehelper.domain.Sort;
import io.choerodon.swagger.annotation.Permission;
import io.swagger.annotations.ApiOperation;
import org.hzero.core.base.BaseController;
import org.hzero.core.util.Results;
import org.hzero.mybatis.helper.SecurityTokenHelper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import com.hand.xhiker.app.service.UserService;
import com.hand.xhiker.domain.entity.User;
import com.hand.xhiker.domain.repository.UserRepository;
import springfox.documentation.annotations.ApiIgnore;

import java.util.List;

/**
 * 用户信息表(User)控制层
 *
 * @author liwei
 * @since 2024-09-25 15:10:00
 */

@ApiOperation(value = "用户信息表")
@RestController("userController.v2")
@RequestMapping("/v2/{organizationId}/user")
public class UserController extends BaseController {

    @Autowired
    private UserService userService;

    @ApiOperation(value = "保存用户信息")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @PostMapping("/saveOrUpdate")
    public ResponseEntity<User> saveOrUpdate(@RequestBody UserDTO userDTO, @PathVariable Long organizationId) {
        validObject(userDTO);
        User user = userService.saveOrUpdate(organizationId, userDTO);
        return Results.success(user);
    }

    @ApiOperation(value = "删除用户")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @PostMapping("/delete")
    public ResponseEntity<Boolean> delete(@RequestBody UserDTO userDTO, @PathVariable Long organizationId) {
        userService.delete(organizationId, userDTO);
        return Results.success(Boolean.TRUE);
    }

    @ApiOperation(value = "查询用户")
    @Permission(level = ResourceLevel.ORGANIZATION)
    @GetMapping("/query")
    public ResponseEntity<Page<User>> query(UserDTO userDTO, @PathVariable Long organizationId,
                                                 @ApiIgnore @SortDefault(value = User.FIELD_USER_ID, direction = Sort.Direction.DESC) PageRequest pageRequest) {
        Page<User> result = userService.query(organizationId, userDTO, pageRequest);
        return Results.success(result);
    }

}
```

#### Output:

```json
{
  "translatedCode": "package com.hand.xhiker.api.controller.v2;\n\nimport com.hand.xhiker.api.dto.UserDTO;\nimport com.hand.xhiker.domain.entity.User;\nimport io.choerodon.core.domain.Page;\nimport io.choerodon.core.iam.ResourceLevel;\nimport io.choerodon.mybatis.pagehelper.annotation.SortDefault;\nimport io.choerodon.mybatis.pagehelper.domain.PageRequest;\nimport io.choerodon.mybatis.pagehelper.domain.Sort;\nimport io.choerodon.swagger.annotation.Permission;\nimport io.swagger.annotations.ApiOperation;\nimport org.hzero.core.base.BaseController;\nimport org.hzero.core.util.Results;\nimport org.hzero.mybatis.helper.SecurityTokenHelper;\nimport org.springframework.beans.factory.annotation.Autowired;\nimport org.springframework.http.ResponseEntity;\nimport org.springframework.web.bind.annotation.*;\nimport com.hand.xhiker.app.service.UserService;\nimport com.hand.xhiker.domain.entity.User;\nimport com.hand.xhiker.domain.repository.UserRepository;\nimport springfox.documentation.annotations.ApiIgnore;\n\nimport java.util.List;\n/**\n * User information table (User) controller \n * @author liwei\n * @since 2024-09-25 15:10:00\n */\n\n@RestController(\"userController.v2\")\n@RequestMapping(\"/v2/{organizationId}/user\")\npublic class UserController extends BaseController {\n\n    @Autowired\n    private UserService userService;\n\n    @ApiOperation(value = \"Save user information\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @PostMapping(\"/saveOrUpdate\")\n    public ResponseEntity<User> saveOrUpdate(@RequestBody UserDTO userDTO, @PathVariable Long organizationId) {\n        validObject(userDTO);\n        User user = userService.saveOrUpdate(organizationId, userDTO);\n        return Results.success(user);\n    }\n\n    @ApiOperation(value = \"Delete user\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @PostMapping(\"/delete\")\n    public ResponseEntity<Boolean> delete(@RequestBody UserDTO userDTO, @PathVariable Long organizationId) {\n        userService.delete(organizationId, userDTO);\n        return Results.success(Boolean.TRUE);\n    }\n\n    @ApiOperation(value = \"Query user\")\n    @Permission(level = ResourceLevel.ORGANIZATION)\n    @GetMapping(\"/query\")\n    public ResponseEntity<Page<User>> query(UserDTO userDTO, @PathVariable Long organizationId,\n                                                 @ApiIgnore @SortDefault(value = User.FIELD_USER_ID, direction = Sort.Direction.DESC) PageRequest pageRequest) {\n        Page<User> result = userService.query(organizationId, userDTO, pageRequest);\n        return Results.success(result);\n    }\n\n}",
  "details": [
    {
      "lineNumber": 27,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "/**\n * 用户信息表(User)控制层\n *\n * @author liwei\n * @since 2024-09-25 15:10:00\n */",
      "translatedText": "/**\n * User information table (User) controller\n *\n * @author liwei\n * @since 2024-09-25 15:10:00\n */"
    },
    {
      "lineNumber": 41,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "@ApiOperation(value = \"保存接口\")",
      "translatedText": "@ApiOperation(value = \"Save user information\")"
    },
    {
      "lineNumber": 50,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "@ApiOperation(value = \"删除接口\")",
      "translatedText": "@ApiOperation(value = \"Delete user\")"
    },
    {
      "lineNumber": 58,
      "lineType": "annotation",
      "jobType": "Text Translation",
      "originalText": "@ApiOperation(value = \"查询接口\")",
      "translatedText": "@ApiOperation(value = \"Query user\")"
    }
  ]
}
```



### DEMOS5

#### Input:

```java
package com.gds.jpi.dcim.model;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import com.baomidou.mybatisplus.extension.activerecord.Model;

import io.swagger.annotations.ApiModelProperty;

/**
 * @author ZhangHao
 * @className AlarmAccountPriority
 * @date 2021/4/20
 * @description 通知角色
 */
@TableName(schema = "cbms", value = "alarm_notice_priority_dict")
public class AlarmAccountPriority extends Model<AlarmAccountPriority> {

    private static final long serialVersionUID = 1705264939604061343L;

    /**
     * 通知优先级主键
     */
    @ApiModelProperty(value = "通知优先级主键")
    @TableId(type = IdType.AUTO)
    private Integer id;

    /**
     * 通知优先级
     */
    @ApiModelProperty(value = "通知优先级")
    private String name;

    /**
     * 排序依据
     */
    @ApiModelProperty(value = "排序依据", name = "order_by")
    private Integer orderBy;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getOrderBy() {
        return orderBy;
    }

    public void setOrderBy(Integer orderBy) {
        this.orderBy = orderBy;
    }

    @Override
    public String toString() {
        return "AlarmAccountPriority{" + "id=" + id + ", name='" + name + '\'' + ", orderBy=" + orderBy + '}';
    }
}
```

#### Output:

```json
{
"translatedCode": "package com.gds.jpi.dcim.model;\n\nimport com.baomidou.mybatisplus.annotation.IdType;\nimport com.baomidou.mybatisplus.annotation.TableId;\nimport com.baomidou.mybatisplus.annotation.TableName;\nimport com.baomidou.mybatisplus.extension.activerecord.Model;\n\nimport io.swagger.annotations.ApiModelProperty;\n\n/**\n * Author: ZhangHao\n * ClassName: AlarmAccountPriority\n * Date: 2021/4/20\n * Description: Notification role\n */\n\n@TableName(schema = \"cbms\", value = \"alarm_notice_priority_dict\")\npublic class AlarmAccountPriority extends Model<AlarmAccountPriority> {\n\n    private static final long serialVersionUID = 1705264939604061343L;\n\n    /**\n     * Notification priority primary key\n     */\n    @ApiModelProperty(value = \"Notification priority primary key\")\n    @TableId(type = IdType.AUTO)\n    private Integer id;\n\n    /**\n     * Notification priority\n     */\n    @ApiModelProperty(value = \"Notification priority\")\n    private String name;\n\n    /**\n     * Sorting basis\n     */\n    @ApiModelProperty(value = \"Sorting basis\", name = \"order_by\")\n    private Integer orderBy;\n\n    public Integer getId() {\n        return id;\n    }\n\n    public void setId(Integer id) {\n        this.id = id;\n    }\n\n    public String getName() {\n        return name;\n    }\n\n    public void setName(String name) {\n        this.name = name;\n    }\n\n    public Integer getOrderBy() {\n        return orderBy;\n    }\n\n    public void setOrderBy(Integer orderBy) {\n        this.orderBy = orderBy;\n    }\n\n    @Override\n    public String toString() {\n        return \"AlarmAccountPriority{\" + \"id=\" + id + \", name='\" + name + '\\'' + \", orderBy=\" + orderBy + '}';\n    }\n\n}",
"details": [
{
"lineNumber": 9,
"lineType": "comment",
"jobType": "Text Translation",
"originalText": "/**\n * @author ZhangHao\n * @className AlarmAccountPriority\n * @date 2021/4/20\n * @description 通知角色\n */",
"translatedText": "/**\n * @author ZhangHao\n * @className AlarmAccountPriority\n * @date 2021/4/20\n * @description Notification role\n */"
},
{
"lineNumber": 17,
"lineType": "comment",
"jobType": "Text Translation",
"originalText": "/**\n     * 通知优先级主键\n     */",
"translatedText": "/**\n     * Notification priority primary key\n     */"
},
{
"lineNumber": 23,
"lineType": "annotation",
"jobType": "Text Translation",
"originalText": "@ApiModelProperty(value = "通知优先级主键")",
"translatedText": "@ApiModelProperty(value = "Notification priority primary key")"
},
{
"lineNumber": 27,
"lineType": "comment",
"jobType": "Text Translation",
"originalText": "/**\n     * 通知优先级\n     */",
"translatedText": "/**\n     * Notification priority\n     */"
},
{
"lineNumber": 30,
"lineType": "annotation",
"jobType": "Text Translation",
"originalText": "@ApiModelProperty(value = "通知优先级")",
"translatedText": "@ApiModelProperty(value = "Notification priority")"
},
{
"lineNumber": 34,
"lineType": "comment",
"jobType": "Text Translation",
"originalText": "/**\n     * 排序依据\n     */",
"translatedText": "/**\n     * Sorting basis\n     */"
},
{
"lineNumber": 37,
"lineType": "annotation",
"jobType": "Text Translation",
"originalText": "@ApiModelProperty(value = "排序依据", name = "order_by")",
"translatedText": "@ApiModelProperty(value = "Sorting basis", name = "order_by")"
}
]
}

```





# 🐟 GARGANTUAN WAS HERE 🐟
