## yml

### INSTRUCTION

> You are a professional code translator specializing in translating YML/YAML source code, documentation, and comments.

> Your task is to translate all Chinese comments or text within the provided YML/YAML code into English and return the result as JSON value. 
> Ensure the file structure, imports, and comments are preserved exactly as they are. Keep all comments in their original format, including single-line or block comments. 
> Include line numbers and preserve alignment for readability. Do not capitalized letters if the original input is not capitalized.

> Return the result as a JSON containing:
> "translatedCode": the complete YML/YAML code translated into English, maintaining its formatting. 
> "details": A JSON array where each object contains: 
> "lineNumber": the exact line number of the translation. 
> "lineType": whether the line is a comment, annotation, or other. 
> "jobType": The type of transformation, e.g., "text Translation". 
> "originalText": the text before translation. 
> "translatedText": the translated text. Keep the formatting letter as the original text.

> Ensure the line numbers in "details" align exactly with the original input file (do not add new line). 
> Furthermore, do not change the value inside this ${} annotation, keep the value align exactly with the original input file (e.g., ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}).

### DEMOS1

#### Input:

```yml
spring:
  datasource:
    primary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}

    secondary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}

    liquidate:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql-liquidate:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_LIQUIDATE_PWD:wN2D&b.Z3k}
  redis:
    std:
      # 测点,指标库
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 100ms
          min-idle: 8
      # 哨兵配置
      sentinel:
        # master节点
        master: mymaster
        nodes: redis-cluster-0:6379, redis-cluster-1:6379, redis-cluster-2:6379
        password: ${JPI_DCIM_REDISCLUSTER_PWD:QT29nxDhP5m0578fj918}
    cache:
      host: jpi-dcim-redis
      port: 6379
      password: ${JPI_DCIM_REDIS_PWD:4D6e8Zu5qK70l9xph3RN}
      # mybatis二级缓存默认库
      database-list: 0
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 5ms
          min-idle: 1

gds-service:
  api-v1:
    url: https://petest.gds-services.com/api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  api-v2:
    url: https://petest.gds-services.com/api/v2/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  api-cip:
    url: https://petest.gds-services.com/cip-api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  oauth:
    url: http://auth-api
  notice:
    url: https://petest.gds-services.com/notification/dynamic/notification_api.php
  notice-ewechat:
    url: https://petest.gds-services.com/api/notice/ewechat/message/send
  api-sms:
    url: http://jpi-sms/jpi/sms/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  dcim:
    url: https://petest.gds-services.com/jpi/dcim/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
logging:
  level:
    com.gds.jpi.dcim.core.mapper: debug

government:
  #api: http://47.96.110.226:8273
  api: https://sheitc-push-http.gds-services.com
  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/
  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/
  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/

government-energy:
  api: http://61.152.172.186:19180


# minio配置
minio:
  endpoint: http://minio
  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:dcim}
  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:2plnlMla}
  bucket: dcim
```

#### Output:

```json
{
  "translatedCode": "spring:\n  datasource:\n    primary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n\n    secondary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n\n    liquidate:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql-liquidate:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_LIQUIDATE_PWD:wN2D&b.Z3k}\n  redis:\n    std:\n      # measuring point, indicator library\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 100ms\n          min-idle: 8\n      # sentinel configuration\n      sentinel:\n        # master node\n        master: mymaster\n        nodes: redis-cluster-0:6379, redis-cluster-1:6379, redis-cluster-2:6379\n        password: ${JPI_DCIM_REDISCLUSTER_PWD:QT29nxDhP5m0578fj918}\n    cache:\n      host: jpi-dcim-redis\n      port: 6379\n      password: ${JPI_DCIM_REDIS_PWD:4D6e8Zu5qK70l9xph3RN}\n      # mybatis secondary cache default database\n      database-list: 0\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 5ms\n          min-idle: 1\n\ngds-service:\n  api-v1:\n    url: https://petest.gds-services.com/api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  api-v2:\n    url: https://petest.gds-services.com/api/v2/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  api-cip:\n    url: https://petest.gds-services.com/cip-api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  oauth:\n    url: http://auth-api\n  notice:\n    url: https://petest.gds-services.com/notification/dynamic/notification_api.php\n  notice-ewechat:\n    url: https://petest.gds-services.com/api/notice/ewechat/message/send\n  api-sms:\n    url: http://jpi-sms/jpi/sms/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  dcim:\n    url: https://petest.gds-services.com/jpi/dcim/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\nlogging:\n  level:\n    com.gds.jpi.dcim.core.mapper: debug\n\ngovernment:\n  # api: http://47.96.110.226:8273\n  api: https://sheitc-push-http.gds-services.com\n  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/\n  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/\n  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/\n\ngovernment-energy:\n  api: http://61.152.172.186:19180\n\n# minio configuration\nminio:\n  endpoint: http://minio\n  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:dcim}\n  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:2plnlMla}\n  bucket: dcim",
  "details": [
    {
      "lineNumber": 22,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 测点,指标库",
      "translatedText": "# measuring point, indicator library"
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 哨兵配置",
      "translatedText": "# sentinel configuration"
    },
    {
      "lineNumber": 34,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# master节点",
      "translatedText": "# master node"
    },
    {
      "lineNumber": 42,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# mybatis二级缓存默认库",
      "translatedText": "# mybatis secondary cache default database"
    },
    {
      "lineNumber": 95,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# minio配置",
      "translatedText": "# minio configuration"
    }
  ]
}
```

### DEMOS2

#### Input:

```yml
spring:
  datasource:
    primary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}

    secondary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}

    liquidate:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql-liquidate:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_LIQUIDATE_PWD:wN2D&b.Z3k}
  redis:
    std:
      # 测点,指标库
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 100ms
          min-idle: 8
      # 哨兵配置
      sentinel:
        # master节点
        master: mymaster
        nodes: redis-cluster-0:6379, redis-cluster-1:6379, redis-cluster-2:6379
        password: ${JPI_DCIM_REDISCLUSTER_PWD:QT29nxDhP5m0578fj918}
    cache:
      host: jpi-dcim-redis
      port: 6379
      password: ${JPI_DCIM_REDIS_PWD:4D6e8Zu5qK70l9xph3RN}
      # mybatis二级缓存默认库
      database-list: 0
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 5ms
          min-idle: 1

gds-service:
  api-v1:
    url: https://petest.gds-services.com/api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  api-v2:
    url: https://petest.gds-services.com/api/v2/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  api-cip:
    url: https://petest.gds-services.com/cip-api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  oauth:
    url: http://auth-api
  notice:
    url: https://petest.gds-services.com/notification/dynamic/notification_api.php
  notice-ewechat:
    url: https://petest.gds-services.com/api/notice/ewechat/message/send
  api-sms:
    url: http://jpi-sms/jpi/sms/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  dcim:
    url: https://petest.gds-services.com/jpi/dcim/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
logging:
  level:
    com.gds.jpi.dcim.core.mapper: debug

government:
  #api: http://47.96.110.226:8273
  api: https://sheitc-push-http.gds-services.com
  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/
  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/
  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/

government-energy:
  api: http://61.152.172.186:19180


# minio配置
minio:
  endpoint: http://minio
  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:dcim}
  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:2plnlMla}
  bucket: dcim
```

#### Output:

```json
{
  "translatedCode": "spring:\n  datasource:\n    primary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n\n    secondary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n\n    liquidate:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql-liquidate:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_LIQUIDATE_PWD:wN2D&b.Z3k}\n  redis:\n    std:\n      # measuring point, indicator library\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 100ms\n          min-idle: 8\n      # sentinel configuration\n      sentinel:\n        # master node\n        master: mymaster\n        nodes: redis-cluster-0:6379, redis-cluster-1:6379, redis-cluster-2:6379\n        password: ${JPI_DCIM_REDISCLUSTER_PWD:QT29nxDhP5m0578fj918}\n    cache:\n      host: jpi-dcim-redis\n      port: 6379\n      password: ${JPI_DCIM_REDIS_PWD:4D6e8Zu5qK70l9xph3RN}\n      # mybatis secondary cache default database\n      database-list: 0\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 5ms\n          min-idle: 1\n\ngds-service:\n  api-v1:\n    url: https://petest.gds-services.com/api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  api-v2:\n    url: https://petest.gds-services.com/api/v2/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  api-cip:\n    url: https://petest.gds-services.com/cip-api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  oauth:\n    url: http://auth-api\n  notice:\n    url: https://petest.gds-services.com/notification/dynamic/notification_api.php\n  notice-ewechat:\n    url: https://petest.gds-services.com/api/notice/ewechat/message/send\n  api-sms:\n    url: http://jpi-sms/jpi/sms/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  dcim:\n    url: https://petest.gds-services.com/jpi/dcim/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\nlogging:\n  level:\n    com.gds.jpi.dcim.core.mapper: debug\n\ngovernment:\n  # api: http://47.96.110.226:8273\n  api: https://sheitc-push-http.gds-services.com\n  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/\n  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/\n  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/\n\ngovernment-energy:\n  api: http://61.152.172.186:19180\n\n# minio configuration\nminio:\n  endpoint: http://minio\n  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:dcim}\n  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:2plnlMla}\n  bucket: dcim",
  "details": [
    {
      "lineNumber": 22,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 测点,指标库",
      "translatedText": "# measuring point, indicator library"
    },
    {
      "lineNumber": 32,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 哨兵配置",
      "translatedText": "# sentinel configuration"
    },
    {
      "lineNumber": 34,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# master节点",
      "translatedText": "# master node"
    },
    {
      "lineNumber": 42,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# mybatis二级缓存默认库",
      "translatedText": "# mybatis secondary cache default database"
    },
    {
      "lineNumber": 95,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# minio配置",
      "translatedText": "# minio configuration"
    }
  ]
}
```

### DEMOS3

#### Input:

```yml
spring:
  datasource:
    primary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.240.171:3306/geo?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:qu<y!1=5N+}

    secondary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.240.171:3306/geo?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:qu<y!1=5N+}
  redis:
    std:
      # 测点,指标库
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 100ms
          min-idle: 8
      # 哨兵配置
      sentinel:
        # master节点
        master: mymaster
        nodes: 192.168.242.121:26380, 192.168.242.121:26381, 192.168.242.121:26382
        password: ${JPI_DCIM_REDISCLUSTER_PWD:QT29nxDhP5m0578fj918}
    cache:
      host: 192.168.240.172
      port: 6379
      database: 0
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 5ms
          min-idle: 1

gds-service:
  api-v1:
    url: https://pestg.gds-services.com/api/
  api-v2:
    url: https://pestg.gds-services.com/api/v2/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}

  api-cip:
    url: https://pestg.gds-services.com/cip-api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  oauth:
    url: https://pestg.gds-services.com/oauth
  notice:
    url: https://pestg.gds-services.com/api/notification_api.api
  notice-ewechat:
    url: https://pestg.gds-services.com/api/notice/ewechat/message/send
  dcim-park:
    url: https://park{0}-stg.gds-services.com/api/v3/park-query-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
```

#### Output:

```json
{
  "translatedCode": "spring:\n  datasource:\n    primary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.240.171:3306/geo?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:qu<y!1=5N+}\n\n    secondary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.240.171:3306/geo?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:qu<y!1=5N+}\n  redis:\n    std:\n      # measuring points, index library\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 100ms\n          min-idle: 8\n      # sentinel configuration\n      sentinel:\n        # master node\n        master: mymaster\n        nodes: 192.168.242.121:26380, 192.168.242.121:26381, 192.168.242.121:26382\n        password: ${JPI_DCIM_REDISCLUSTER_PWD:QT29nxDhP5m0578fj918}\n    cache:\n      host: 192.168.240.172\n      port: 6379\n      database: 0\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 5ms\n          min-idle: 1\n\ngds-service:\n  api-v1:\n    url: https://pestg.gds-services.com/api/\n  api-v2:\n    url: https://pestg.gds-services.com/api/v2/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n\n  api-cip:\n    url: https://pestg.gds-services.com/cip-api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  oauth:\n    url: https://pestg.gds-services.com/oauth\n  notice:\n    url: https://pestg.gds-services.com/api/notification_api.api\n  notice-ewechat:\n    url: https://pestg.gds-services.com/api/notice/ewechat/message/send\n  dcim-park:\n    url: https://park{0}-stg.gds-services.com/api/v3/park-query-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}",
  "details": [
    {
      "lineNumber": 16,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 测点,指标库",
      "translatedText": "# measuring points, index library"
    },
    {
      "lineNumber": 26,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 哨兵配置",
      "translatedText": "# sentinel configuration"
    },
    {
      "lineNumber": 28,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# master节点",
      "translatedText": "# master node"
    }
  ]
}
```

### DEMOS4

#### Input:

```yml
spring:
  datasource:
    primary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}

    secondary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}


    liquidate:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.242.103:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: zhongchenyang
      password: ${JPI_DCIM_LIQUIDATE_PWD:3YYcT6LHR*}
  redis:
    std:
      # 测点,指标库
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 100ms
          min-idle: 8
      # 哨兵配置
      sentinel:
        # master节点
        master: mymaster
        nodes: redis.redis.svc.cluster.local:26379
        password: ${JPI_DCIM_REDISCLUSTER_PWD:Gds@123456}
    cache:
      host: redis
      port: 6379
      # mybatis二级缓存默认库
      database-list: 0
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 5ms
          min-idle: 1

gds-service:
  api-v1:
    url: https://pedev.gds-services.com/api/
    basic-auth-user: JPI
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:123456}
  api-v2:
    url: https://pedev.gds-services.com/api/v2/
    basic-auth-user: API_V2
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:123456}
  api-cip:
    url: https://pedev.gds-services.com/cip-api/
    basic-auth-user: API_V2
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:123456}
  oauth:
    url: https://pedev.gds-services.com/oauth
  notice:
    url: http://192.168.242.104:82/api/notification_api.api
  notice-ewechat:
    url: http://api-notice-ewechat:7004/message/send
  api-sms:
    url: http://192.168.242.120:8300/sms/
    basic-auth-user: JPI
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:L4UBSYSgGaAmZIaV}

government-energy:
  api: http://61.152.172.186:19180


logging:
  level:
    com.gds.jpi.dcim.core.mapper: debug
```

#### Output:

```json
{
  "translatedCode": "spring:\n  datasource:\n    primary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n\n    secondary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://mysql:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n\n\n    liquidate:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.242.103:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: zhongchenyang\n      password: ${JPI_DCIM_LIQUIDATE_PWD:3YYcT6LHR*}\n  redis:\n    std:\n      # measuring points, index library\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 100ms\n          min-idle: 8\n      # sentinel configuration\n      sentinel:\n        # master node\n        master: mymaster\n        nodes: redis.redis.svc.cluster.local:26379\n        password: ${JPI_DCIM_REDISCLUSTER_PWD:Gds@123456}\n    cache:\n      host: redis\n      port: 6379\n      # mybatis secondary cache default library\n      database-list: 0\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 5ms\n          min-idle: 1\n\ngds-service:\n  api-v1:\n    url: https://pedev.gds-services.com/api/\n    basic-auth-user: JPI\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:123456}\n  api-v2:\n    url: https://pedev.gds-services.com/api/v2/\n    basic-auth-user: API_V2\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:123456}\n  api-cip:\n    url: https://pedev.gds-services.com/cip-api/\n    basic-auth-user: API_V2\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:123456}\n  oauth:\n    url: https://pedev.gds-services.com/oauth\n  notice:\n    url: http://192.168.242.104:82/api/notification_api.api\n  notice-ewechat:\n    url: http://api-notice-ewechat:7004/message/send\n  api-sms:\n    url: http://192.168.242.120:8300/sms/\n    basic-auth-user: JPI\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:L4UBSYSgGaAmZIaV}\n\ngovernment-energy:\n  api: http://61.152.172.186:19180\n\n\nlogging:\n  level:\n    com.gds.jpi.dcim.core.mapper: debug",
  "details": [
    {
      "lineNumber": 23,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 测点,指标库",
      "translatedText": "# measuring points, index library"
    },
    {
      "lineNumber": 33,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 哨兵配置",
      "translatedText": "# sentinel configuration"
    },
    {
      "lineNumber": 35,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# master节点",
      "translatedText": "# master node"
    },
    {
      "lineNumber": 42,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# mybatis二级缓存默认库",
      "translatedText": "# mybatis secondary cache default library"
    }
  ]
}
```

### DEMOS5

#### Input:

```yml
spring:
  datasource:
    primary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:Nq<2&#K5Z2}

    secondary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:Nq<2&#K5Z2}
  redis:
    # 指标库
    std:
      # 指标库
      port: 6379
      # 测点,指标库
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 100ms
          min-idle: 8
      # 哨兵配置
      sentinel:
        # master节点
        master: mymaster
        nodes: 192.168.240.220:26379, 192.168.240.221:26379, 192.168.240.222:26379
        password: ${JPI_DCIM_REDISCLUSTER_PWD:7DrbHhBSN55jH7ypcn1v}
    # 缓存库
    cache:
      host: 127.0.0.1
      port: 6379
      password: ${JPI_DCIM_REDIS_PWD:Xpn9GdGYRC5tNru9Zml1}
      # 默认使用0库进行mybatis二级缓存.
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 5ms
          min-idle: 1
  data:
    neo4j:
      uri: bolt://192.168.240.242:7687
      username: neo4j
      password: ${JPI_DCIM_NEO4J_PWD:WGgxdqKiryFH3no7}

gds-service:
  domain:
    url: https://online.gds-services.com/
  api-v1:
    url: https://online.gds-services.com/api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  api-v2:
    url: https://online.gds-services.com/api/v2/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  api-cip:
    url: https://cip.gds-services.com/api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}

  oauth:
    url: https://auth.gds-services.com/oauth
  notice:
    url: https://wechat.gds-services.com/dynamic/notification_api.php
  notice-ewechat:
    url: https://online.gds-services.com/api/notice/ewechat/message/send
  api-sms:
    url: https://online.gds-services.com/jpi/sms/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  dcim-park:
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}


government:
  api: http://sheitc-push.gds-services.com:9273
  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/
  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/
  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/


# minio配置
minio:
  endpoint: https://minio-prd.gds-services.com
  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:tYv2hlb7}
  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:iEq3hOmyw1e}
  bucket: dcim
```

#### Output:

```json
{
  "translatedCode": "spring:\n  datasource:\n    primary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD<2&#K5Z2}\n\n    secondary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD<2&#K5Z2}\n  redis:\n    # indicator library\n    std:\n      # indicator library\n      port: 6379\n      # measuring point, indicator library\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 100ms\n          min-idle: 8\n      # sentinel configuration\n      sentinel:\n        # master node\n        master: mymaster\n        nodes: 192.168.240.220:26379, 192.168.240.221:26379, 192.168.240.222:26379\n        password: ${JPI_DCIM_REDISCLUSTER_PWD:7DrbHhBSN55jH7ypcn1v}\n    # cache library\n    cache:\n      host: 127.0.0.1\n      port: 6379\n      password: ${JPI_DCIM_REDIS_PWD}\n      # default uses database 0 for mybatis secondary cache.\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 5ms\n          min-idle: 1\n  data:\n    neo4j:\n      uri: bolt://192.168.240.242:7687\n      username: neo4j\n      password: ${JPI_DCIM_NEO4J_PWD}\n\ngds-service:\n  domain:\n    url: https://online.gds-services.com/\n  api-v1:\n    url: https://online.gds-services.com/api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET}\n  api-v2:\n    url: https://online.gds-services.com/api/v2/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET}\n  api-cip:\n    url: https://cip.gds-services.com/api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET}\n\n  oauth:\n    url: https://auth.gds-services.com/oauth\n  notice:\n    url: https://wechat.gds-services.com/dynamic/notification_api.php\n  notice-ewechat:\n    url: https://online.gds-services.com/api/notice/ewechat/message/send\n  api-sms:\n    url: https://online.gds-services.com/jpi/sms/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET}\n  dcim-park:\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET}\n\ngovernment:\n  api: http://sheitc-push.gds-services.com:9273\n  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/\n  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/\n  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/\n\n# minio configuration\nminio:\n  endpoint: https://minio-prd.gds-services.com\n  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY}\n  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY}\n  bucket: dcim",
  "details": [
    {
      "lineNumber": 15,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 指标库",
      "translatedText": "# indicator library"
    },
    {
      "lineNumber": 17,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 指标库",
      "translatedText": "# indicator library"
    },
    {
      "lineNumber": 19,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 测点,指标库",
      "translatedText": "# measuring point, indicator library"
    },
    {
      "lineNumber": 29,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 哨兵配置",
      "translatedText": "# sentinel configuration"
    },
    {
      "lineNumber": 31,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# master节点",
      "translatedText": "# master node"
    },
    {
      "lineNumber": 35,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 缓存库",
      "translatedText": "# cache library"
    },
    {
      "lineNumber": 40,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# 默认使用0库进行mybatis二级缓存.",
      "translatedText": "# default uses database 0 for mybatis secondary cache."
    },
    {
      "lineNumber": 94,
      "lineType": "comment",
      "jobType": "Text Translation",
      "originalText": "# minio配置",
      "translatedText": "# minio configuration"
    }
  ]
}
```

### DEMOS6

#### Input:

```yml
spring:
  datasource:
    primary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://10.80.92.200:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}

    secondary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://10.80.92.200:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi
      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}
  redis:
    host: dcim-redis
    port: 6379
    password: ${JPI_DCIM_REDIS_PWD:gds123}
    timeout: 5s
    lettuce:
      shutdown-timeout: 100s
      pool:
        max-active: 10
        max-idle: 8
        max-wait: 5ms
        min-idle: 1
    std:
      # 测点,指标库
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 100ms
          min-idle: 8
    cache:
      host: dcim-redis
      port: 6379
      password: ${JPI_DCIM_REDIS_PWD:gds123}
      # mybatis二级缓存默认库
      database-list: 0
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 5ms
          min-idle: 1

gds-service:
  domain:
    url: https://pedev.gds-services.com/
  api-v1:
    url: https://petest.gds-services.com/api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  api-v2:
    url: https://petest.gds-services.com/api/v2/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  api-cip:
    url: https://petest.gds-services.com/cip-api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  oauth:
    url: https://petest.gds-services.com/oauth/
  notice:
    url: https://petest.gds-services.com/notification/dynamic/notification_api.php
  notice-ewechat:
    url: https://petest.gds-services.com/api/notice/ewechat/message/send
  api-sms:
    url: https://petest.gds-services.com/jpi/sms/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  dcim:
    url: https://petest.gds-services.com/jpi/dcim/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
  dcim-park:
    url: https://park{0}-test.gds-services.com/api/v3/park-query-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}
logging:
  level:
    com.gds.jpi.dcim.core.mapper: debug

government:
  api: http://47.96.110.226:8273
  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/
  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/
  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/


# minio配置
minio:
  endpoint: https://minio-dev.gds-services.com
  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:console}
  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:console123}
  bucket: dcim
```

#### Output:

```json
{
  "translatedCode": "spring:\n  datasource:\n    primary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://10.80.92.200:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n\n    secondary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://10.80.92.200:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi\n      password: ${JPI_DCIM_MYSQL_PWD:ez@6s#dz&4mg}\n  redis:\n    host: dcim-redis\n    port: 6379\n    password: ${JPI_DCIM_REDIS_PWD:gds123}\n    timeout: 5s\n    lettuce:\n      shutdown-timeout: 100s\n      pool:\n        max-active: 10\n        max-idle: 8\n        max-wait: 5ms\n        min-idle: 1\n    std:\n      # measurement point, indicator library\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 100ms\n          min-idle: 8\n    cache:\n      host: dcim-redis\n      port: 6379\n      password: ${JPI_DCIM_REDIS_PWD:gds123}\n      # mybatis secondary cache default library\n      database-list: 0\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 5ms\n          min-idle: 1\n\ngds-service:\n  domain:\n    url: https://pedev.gds-services.com/\n  api-v1:\n    url: https://petest.gds-services.com/api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  api-v2:\n    url: https://petest.gds-services.com/api/v2/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  api-cip:\n    url: https://petest.gds-services.com/cip-api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  oauth:\n    url: https://petest.gds-services.com/oauth/\n  notice:\n    url: https://petest.gds-services.com/notification/dynamic/notification_api.php\n  notice-ewechat:\n    url: https://petest.gds-services.com/api/notice/ewechat/message/send\n  api-sms:\n    url: https://petest.gds-services.com/jpi/sms/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  dcim:\n    url: https://petest.gds-services.com/jpi/dcim/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\n  dcim-park:\n    url: https://park{0}-test.gds-services.com/api/v3/park-query-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:Gds123456}\nlogging:\n  level:\n    com.gds.jpi.dcim.core.mapper: debug\n\ngovernment:\n  api: http://47.96.110.226:8273\n  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/\n  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/\n  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/\n\n\n# minio configuration\nminio:\n  endpoint: https://minio-dev.gds-services.com\n  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:console}\n  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:console123}\n  bucket: dcim",
  "details": [
    {
      "lineNumber": 27,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "测点,指标库",
      "translatedText": "measurement point, indicator library"
    },
    {
      "lineNumber": 41,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "mybatis二级缓存默认库",
      "translatedText": "mybatis secondary cache default library"
    },
    {
      "lineNumber": 96,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "minio配置",
      "translatedText": "minio configuration"
    }
  ]
}
```

### DEMOS7

#### Input:

```yml
spring:
  datasource:
    primary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:Nq<2&#K5Z2}

    secondary:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_MYSQL_PWD:Nq<2&#K5Z2}
    liquidate:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://192.168.240.160:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true
      username: jpi-dcim
      password: ${JPI_DCIM_LIQUIDATE_PWD:ax9vkaJYSPX}
  redis:
    # 指标库
    std:
      # 指标库
      port: 6379
      # 测点,指标库
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 100ms
          min-idle: 8
      # 哨兵配置
      sentinel:
        # master节点
        master: mymaster
        nodes: 192.168.240.220:26379, 192.168.240.221:26379, 192.168.240.222:26379
        password: ${JPI_DCIM_REDISCLUSTER_PWD:7DrbHhBSN55jH7ypcn1v}
    # 缓存库
    cache:
      host: 127.0.0.1
      port: 6379
      password: ${JPI_DCIM_REDIS_PWD:Xpn9GdGYRC5tNru9Zml1}
      # 默认使用0库进行mybatis二级缓存.
      database-list: 0,1
      timeout: 5s
      lettuce:
        shutdown-timeout: 100s
        pool:
          max-active: 10
          max-idle: 8
          max-wait: 5ms
          min-idle: 1
#  elasticsearch:
#    rest:
#      uris: 192.168.240.247:9200
  data:
    neo4j:
      uri: bolt://192.168.240.242:7687
      username: neo4j
      password: ${JPI_DCIM_NEO4J_PWD:WGgxdqKiryFH3no7}
app:
  api-gateway:
    basic: QVBJX0dBVEVXQVk6QmtueThVN1RicmpES1ZKRQ==
gds-service:
  domain:
    url: https://online.gds-services.com/
  api-v1:
    url: https://online.gds-services.com/api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}

  api-v2:
    url: https://online.gds-services.com/api/v2/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  api-cip:
    url: https://cip.gds-services.com/api/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  oauth:
    url: https://auth.gds-services.com/oauth
  notice:
    url: https://wechat.gds-services.com/dynamic/notification_api.php
  notice-ewechat:
    url: https://online.gds-services.com/api/notice/ewechat/message/send
  api-sms:
    url: https://online.gds-services.com/jpi/sms/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  dcim-park:
    url: https://park{0}.gds-services.com/api/v3/park-query-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  dcom-power-backend:
    url: https://online.gds-services.com/api/v3/dcom-power-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  jpi-eicd:
    url: https://online.gds-services.com/api/v3/dcom-eicd-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  dcbm:
    url: https://online.gds-services.com/api/v3/dcbm-db/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  incident:
    url: https://online.gds-services.com/api/v3/dcom-incident-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  database:
    url: https://online.gds-services.com/api/v3/infra-database-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  ewechat:
    url: https://online.gds-services.com/api/v3/base-ewechat-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}
  dcom-duty:
    url: https://online.gds-services.com/api/v3/dcom-duty-backend/
    basic-auth-user: JPI_DCIM
    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}

government:
  api: http://sheitc-push.gds-services.com:9273
  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/
  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/
  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/


# minio配置
minio:
  endpoint: https://minio-prd.gds-services.com
  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:tYv2hlb7}
  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:iEq3hOmyw1e}
  bucket: dcim

server:
  port: 8400

elasticsearch:
  host: 192.168.240.247
```

#### Output:

```json
{
  "translatedCode": "spring:\n  datasource:\n    primary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:Nq<2&#K5Z2}\n\n    secondary:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.240.156:3306/cbms?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_MYSQL_PWD:Nq<2&#K5Z2}\n    liquidate:\n      driver-class-name: com.mysql.cj.jdbc.Driver\n      jdbc-url: jdbc:mysql://192.168.240.160:3306/data_cabinet?useTimezone=true&serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false&allowMultiQueries=true\n      username: jpi-dcim\n      password: ${JPI_DCIM_LIQUIDATE_PWD:ax9vkaJYSPX}\n  redis:\n    # indicator library\n    std:\n      # indicator library\n      port: 6379\n      # measurement point, indicator library\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 100ms\n          min-idle: 8\n      # sentinel configuration\n      sentinel:\n        # master node\n        master: mymaster\n        nodes: 192.168.240.220:26379, 192.168.240.221:26379, 192.168.240.222:26379\n        password: ${JPI_DCIM_REDISCLUSTER_PWD:7DrbHhBSN55jH7ypcn1v}\n    # cache library\n    cache:\n      host: 127.0.0.1\n      port: 6379\n      password: ${JPI_DCIM_REDIS_PWD:Xpn9GdGYRC5tNru9Zml1}\n      # default use 0 library for mybatis secondary cache.\n      database-list: 0,1\n      timeout: 5s\n      lettuce:\n        shutdown-timeout: 100s\n        pool:\n          max-active: 10\n          max-idle: 8\n          max-wait: 5ms\n          min-idle: 1\n#  elasticsearch:\n#    rest:\n#      uris: 192.168.240.247:9200\n  data:\n    neo4j:\n      uri: bolt://192.168.240.242:7687\n      username: neo4j\n      password: ${JPI_DCIM_NEO4J_PWD:WGgxdqKiryFH3no7}\napp:\n  api-gateway:\n    basic: QVBJX0dBVEVXQVk6QmtueThVN1RicmpES1ZKRQ==\ngds-service:\n  domain:\n    url: https://online.gds-services.com/\n  api-v1:\n    url: https://online.gds-services.com/api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n\n  api-v2:\n    url: https://online.gds-services.com/api/v2/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  api-cip:\n    url: https://cip.gds-services.com/api/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  oauth:\n    url: https://auth.gds-services.com/oauth\n  notice:\n    url: https://wechat.gds-services.com/dynamic/notification_api.php\n  notice-ewechat:\n    url: https://online.gds-services.com/api/notice/ewechat/message/send\n  api-sms:\n    url: https://online.gds-services.com/jpi/sms/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  dcim-park:\n    url: https://park{0}.gds-services.com/api/v3/park-query-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  dcom-power-backend:\n    url: https://online.gds-services.com/api/v3/dcom-power-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  jpi-eicd:\n    url: https://online.gds-services.com/api/v3/dcom-eicd-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  dcbm:\n    url: https://online.gds-services.com/api/v3/dcbm-db/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  incident:\n    url: https://online.gds-services.com/api/v3/dcom-incident-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  database:\n    url: https://online.gds-services.com/api/v3/infra-database-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  ewechat:\n    url: https://online.gds-services.com/api/v3/base-ewechat-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n  dcom-duty:\n    url: https://online.gds-services.com/api/v3/dcom-duty-backend/\n    basic-auth-user: JPI_DCIM\n    basic-auth-pwd: ${JPI_DCIM_APP_SECRET:fejnA64GVLErBqOUDaBM}\n\ngovernment:\n  api: http://sheitc-push.gds-services.com:9273\n  dcConfigPath: /opt/jpi-dcim/government/shanghai/eco-it/config/\n  publicKeyPath: /opt/jpi-dcim/government/shanghai/eco-it/rsa-pub/\n  pushDataPath: /opt/files/dcim/government/shanghai/eco-it/log/\n\n\n# minio configuration\nminio:\n  endpoint: https://minio-prd.gds-services.com\n  accessKey: ${JPI_DCIM_MINIO_ACCESS_KEY:tYv2hlb7}\n  secretKey: ${JPI_DCIM_MINIO_SECRET_KEY:iEq3hOmyw1e}\n  bucket: dcim\n\nserver:\n  port: 8400\n\nelasticsearch:\n  host: 192.168.240.247",
  "details": [
    {
      "lineNumber": 20,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "指标库",
      "translatedText": "indicator library"
    },
    {
      "lineNumber": 22,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "指标库",
      "translatedText": "indicator library"
    },
    {
      "lineNumber": 24,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "测点,指标库",
      "translatedText": "measurement point, indicator library"
    },
    {
      "lineNumber": 34,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "哨兵配置",
      "translatedText": "sentinel configuration"
    },
    {
      "lineNumber": 36,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "master节点",
      "translatedText": "master node"
    },
    {
      "lineNumber": 40,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "缓存库",
      "translatedText": "cache library"
    },
    {
      "lineNumber": 45,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "默认使用0库进行mybatis二级缓存.",
      "translatedText": "default use 0 library for mybatis secondary cache."
    },
    {
      "lineNumber": 132,
      "lineType": "comment",
      "jobType": "text Translation",
      "originalText": "minio配置",
      "translatedText": "minio configuration"
    }
  ]
}
```
