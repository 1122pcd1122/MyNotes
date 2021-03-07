---
title:Effective Java学习-服务提供者框架
tags:
	-java
date:2021-03-07 16:15:00
---



# Effective Java学习-服务提供者框架

> 定义:
>
> 服务提供者框架是指这样一个系统,多个服务提供者实现一个服务,系统为服务提供者的客户端提供多个实现(服务),并且把他们从多个实现中解耦出来

- 服务接口(Service Interface) 这是提供者实现的
- 服务提供者注册API(Provider Registration API) 这是系统用来注册实现的,让客户端访问他们
- 服务访问API(Service Access API) 这是客户端用来获取服务的实例的
- 服务提供者接口(Service Provider Interface) 这些提供者负责创建器服务实现的实例,如果没有服务提供者接口,实现就按类名注册,并通过反射进行实例化

## 服务框架

### 服务接口

​	定义一些提供具体服务的方法.

```java
package com.company.serviceProvider;

/**
 * @author peichendong
 * @Description 服务接口
 * @date 2021/3/7-15:05
 */
public interface Service {
    /**
     * 登录
     */
    public void login();

    /**
     * 注册
     */
    public void register();
}

```

## 服务提供者注册API

​	就是在服务提供者具体实现类里面去注册这个API,在类中的静态初始化块中去注册,当使用类加载是将这个类加载到JVM并初始化该类,就会执行注册方法,将服务注册给服务提供者,只有注册了 ,服务提供者才能获取该服务并去使用

```java
  public static void registerProvider(String name, Provider provider){
        PROVIDERS.put(name,provider);
    }

 static {
        ServiceManager.registerProvider("登录注册",new ProviderImpl());
    }

 //Class.forName()初始化指定的类使得静态代码块可以执行  ,手动将指定的类加载到JVM中,并且进行类初始化操作
 Class.forName("com.company.serviceProvider.ProviderImpl");

```

### 服务访问API

获取对应服务提供者的服务

```java
  public static Service getProvider(String name){
        Provider provider = PROVIDERS.get(name);
        if (provider == null){
            throw new IllegalArgumentException("No Provider registered with name = "+name);
        }

        return provider.getService();
    }
```

### 服务提供者接口

在服务提供者接口中,定义了我们要获取什么样的服务

```java
/**
 * @author peichendong
 * @Description 服务提供者接口
 * @date 2021/3/7-15:06
 */
public interface Provider {
    /**
     * 获取登录服务
     * @return 返回登录注册的服务提供具体实现类
     */
     Service getService();
}
```

## 完整代码

**服务接口**

```java
package com.company.serviceProvider;

/**
 * @author peichendong
 * @Description 服务接口
 * @date 2021/3/7-15:05
 */
public interface Service {
    /**
     * 登录
     */
    public void login();

    /**
     * 注册
     */
    public void register();
}
```

**服务具体实现类**

```java
package com.company.serviceProvider;

/**
 * @author peichendong
 * @Description 服务具体实现类
 * @date 2021/3/7-15:06
 */
public class ServiceImpl implements Service{

    @Override
    public void login() {
        System.out.println("登录");
    }

    @Override
    public void register() {
        System.out.println("注册");
    }
}

```

**服务提供者接口**

```java
package com.company.serviceProvider;

/**
 * @author peichendong
 * @Description 服务提供者接口
 * @date 2021/3/7-15:06
 */
public interface Provider {
    /**
     * 获取登录服务
     * @return 返回登录注册的服务提供具体实现类
     */
     Service getService();
}

```

**服务提供者实现类**

```java
package com.company.serviceProvider;

/**
 * @author peichendong
 * @Description 服务提供者具体实现类
 * @date 2021/3/7-15:07
 */
public class ProviderImpl implements Provider{
    static {
        ServiceManager.registerProvider("登录注册",new ProviderImpl());
    }

    @Override
    public Service getService() {
        return new ServiceImpl();
    }
}

```

**Test测试**

```java
package com.company.serviceProvider;

/**
 * @author peichendong
 * @Description 测试类
 * @date 2021/3/7-15:17
 */
public class Test {

    public static void main(String[] args) throws ClassNotFoundException {
        //Class.forName()初始化指定的类使得静态代码块可以执行  ,手动将指定的类加载到JVM中,并且进行类初始化操作
        Class.forName("com.company.serviceProvider.ProviderImpl");
        Service loginAndRegister = ServiceManager.getProvider("登录注册");
        loginAndRegister.login();
        loginAndRegister.register();
    }
}
```

