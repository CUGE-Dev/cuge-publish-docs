# 业务接入指南

## 0. 注意

**目前此业务二方包Springboot版本最高只支持到 2.3.0.RELEASE，使用更高版本的业务方请暂时走RPC泛化调用方式使用二方服务。**

## 1. 引入业务二方包

> 版本请以Maven上 [cuge-client-core](http://47.107.119.93:8090/#browse/search=keyword%3Dcuge-client-core%20AND%20name.raw%3Dcuge-client-core) 最新版本为准。

```xml
<dependency>
  <groupId>com.nemesiss.dev.uge</groupId>
  <artifactId>cuge-client-core</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>
```

私服Maven地址：

```xml
    <repositories>
        <repository>
            <id>maven-public</id>
            <name>maven-public</name>
            <url>http://47.107.119.93:8090/repository/maven-public/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>true</enabled>
            </releases>
        </repository>
    </repositories>

```


## 2. 配置CUGE服务中心集群地址

```properties
eureka.client.serviceUrl.ugeZone=CUGE数据中心服务的eureka地址，例如http://localhost:10010/eureka/
eureka.client.serviceUrl.defaultZone=CUGE数据中心服务的eureka地址，例如http://localhost:10010/eureka/
eureka.client.prefer-same-zone-eureka=true

eureka.instance.preferIpAddress=true
eureka.instance.metadata-map.zone=ugeZone
eureka.instance.lease-renewal-interval-in-seconds=10

# CUGE 用户数据中心的spring.application.name, 以及RPC调用认证密钥
rpc.service.name=cuge-server
rpc.secret=rpc-invocation-secret
```

## 3. 主启动类加入二方包启动配置类

在`scanBasePackageClasses `添加 `CugeClientFeignConfiguration.class`。

## 4. 引入二方Client，开始使用

```kotlin
package com.nemesiss.dev.cugebizinvocationkt

import com.nemesiss.dev.uge.cugeclientcore.clients.CrowdClient
import com.nemesiss.dev.uge.cugeclientcore.config.CugeClientFeignConfiguration
import com.nemesiss.dev.uge.cugeclientcore.models.crowd.SingleCrowdQuery
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.boot.CommandLineRunner
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@SpringBootApplication(scanBasePackageClasses = [CugeClientFeignConfiguration::class])
class CugeBizInvocationKtApplication : CommandLineRunner {

    @Autowired
    lateinit var crowdClient: CrowdClient
    override fun run(vararg args: String?) {
        val user1Query = crowdClient.isInCrowd(SingleCrowdQuery(1, 1))
        val user3Query = crowdClient.isInCrowd(SingleCrowdQuery(1, 3))

        println(user1Query)
        println(user3Query)
    }
}

fun main(args: Array<String>) {
    runApplication<CugeBizInvocationKtApplication>(*args)
}
```

