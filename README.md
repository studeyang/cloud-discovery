# Cloud Discovery

这是一个服务发现组件，它可以让你的应用快速获取`Kubernetes`上部署的服务的实例信息。

# 快速开始

### 引入依赖

```xml
<dependency>
    <groupId>com.github.open</groupId>
    <artifactId>cloud-discovery-starter</artifactId>
</dependency>
```

### 添加注解

启动类添加注解：`org.springframework.cloud.client.discovery.EnableDiscoveryClient`。

```java
package com.github.open.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

### 配置集群连接信息

```yaml
cloud:
  kubernetes:
    apiServerUrl: https://127.0.0.0:5443
    caCertFileUrl: ${user.dir}/cloud-discovery-example/secret/caCertFile.crt
    oauth2Token: eyJhbXXX
    includeNamespaces: infra, vip
```

### 使用样例

```java
@Service
public class ExampleService implements InitializingBean {

    @Autowired
    private DiscoveryClient kubernetesDiscoveryClient;

    @Override
    public void afterPropertiesSet() {
        System.out.println("===== Cloud Discovery Example =====");
        List<ServiceInstance> instance = kubernetesDiscoveryClient.getInstances("courier-producer");
        System.out.println("instance: " + instance);
        System.out.println("ip: " + instance.stream().map(ServiceInstance::getHost).collect(Collectors.toList()));
    }
}
```

