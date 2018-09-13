---
title: 在LEAP项目如何使用Swagger调试Restful Api
tags:
  - Swagger
  - Restful Api
categories:
  - [开源, 接口]
date: 2018-09-13 13:34:56
thumbnail: http://paz1myrij.bkt.clouddn.com/20180913134016.png
---
### 背景
> 在前后端分离开发环境中，后端同事开发好接口后，前端同事需要没有一个浏览所有接口的页面
> `LEAP`中是通过 `/restservices` 访问所有发布的接口，界面不是特别友好

<!-- toc -->

`Swagger`是基于`REST API`测试/文档类插件，由于`LEAP`中可能存在第三方包不兼容的问题（没有针对原`Swagger`服务包做兼容测试，本着能少引用包就少引用的想法），我将`Swagger`服务整合在`LWEG`公共项目中。

### 具体配置
- 打包最新`LWEG.BLL.jar`

***在`Service`类!!!***
#### 添加类注解 
```java
@Singleton 
@Api(name = "PAD RESTful API", description = "技能竞赛PAD端Restservices Api") // name:分类名称; description:本类的描述信息;
public class JNJSPADService extends LEAPContextService
{
}
```

#### 添加方法注解
- `@ApiOperation(value="方法名", notes="描述")`    
- `@ApiImplicitParams` `ApiImplicitParam`
    - `name`参数名称.
    - `value` 参数中文名称.
    - `example` 测试数据.     

所有曝光的参数都要求只传`EntityBean`.比如,方法要接收两个参数name 和 age ,那就请将这两个放到`EntityBean`中 , 然后将EntityBean传给方法,特殊例外,这样写的好处是方便之后扩展.我们还要将方法中必须用到的参数写到注解中,这样方便前端调试. 
```java
@ApiImplicitParams({
        @ApiImplicitParam(name = "Bean里面的参数说明", value = "参数名称", example = "测试数据"),
        @ApiImplicitParam(name = "Bean里面的参数说明", value = "参数名称", example = "测试数据")
    })
```

- `@RequestMapping` restservices 地址 
```java
@RequestMapping(value={"/JNJSApi/PAD_Template/query"}) //'restservices 地址'
```

#### 样例
```java
    /**
     * Api 样例
     *
     * @Description: PAD_Template
     * @param bean
     * @return
     * @author Lix.
     * @date 2018年8月28日 下午5:38:58 
     * @version V1.0
     */
    @ApiOperation(value = "API 服务样例", notes = "入参统一用EntityBean，出参用JsonResult")      
    @ApiImplicitParams({
        @ApiImplicitParam(name = "Bean里面的参数说明", value = "参数名称", example = "测试数据"),
        @ApiImplicitParam(name = "Bean里面的参数说明", value = "参数名称", example = "测试数据")
    })
    @RequestMapping(value={"/JNJSApi/PAD_Template/query"}) //'restservices 地址'
    public JsonResult PAD_Template(EntityBean bean)
    {
        JNJSPADLogic logic = SingletonService.getInstance(JNJSPADLogic.class);
        LogicManager.getInstance().setLogic(this, logic);
        return logic.PAD_Template(bean);
    }
```

#### 创建Swagger匹配类 RestApiConfig
- `description` 描述信息
- `title` 标题
- `termsOfServiceUrl` 服务反馈地址或者官网
- `contact` 开发者
- `version` 版本号
目前一个包一个类就够了.
```java
package com.longrise.JNJS.Conf;

import java.util.List;

import com.longrise.LWEG.UnifyPort.Pojo.ApiInfo;
import com.longrise.LWEG.UnifyPort.Pojo.ApiInfoBuilder;
import com.longrise.LWEG.UnifyPort.Pojo.Contact;
import com.longrise.LWEG.UnifyPort.Pojo.ExternalDocs;
import com.longrise.LWEG.UnifyPort.Swagger.DocInit;
import com.longrise.LWEG.UnifyPort.Swagger.SwaggerApiConfig;

public class RestApiConfig implements SwaggerApiConfig
{
    public DocInit createRestApi(List<String> list)
    {
        return new DocInit().apiInfo(apiInfo()).basePackage(list).externalDocs(externalDocs()).build();
    }
    
    private ApiInfo apiInfo()
    {
        return new ApiInfoBuilder().description("" +
                "<div class='ex_description'>" +
                "   <span><strong>测试地址</strong>：http://192.168</span>" +
                "   <span><strong>正式地址</strong>：http://192.168</span>" +
                "</div>")
        .title("技能竞赛 RESTful API")
        .termsOfServiceUrl("")
        .contact(new Contact("lix、wangf", "", "lix@longrise.com.cn、wangf@longrise.com.cn"))
        .version("1.0")
        .build();
    }
    
    private ExternalDocs externalDocs()
    {
        return new ExternalDocs("Find out more about LEAP", "http://www.longrise.com.cn");
    }

}
```

#### META-INF配置

- 在`SERVICE.CONF`中添加`JNJSApi com.longrise.JNJS.BLL.JNJSPADService {rest}`
- 在`SWAGGER.CONF`中添加`RestApiConfig`的限定名 `com.longrise.JNJS.Conf.RestApiConfig`

#### 测试
- 打开 `http://localhost:8012/JNJS/LEAP/Swagger/index.html?type=jnjs`,type为你要访问的包名称,因为可以很多包都放在一个项目中,所以根据包名来区分.
![](http://paz1myrij.bkt.clouddn.com/20180828174553.png)

- 如果你上面配置的都没有问题那么直接点 1 的位置 参数就自动设置到值,然后点测试将会有返回结果
![](http://paz1myrij.bkt.clouddn.com/20180828174659.png)
![](http://paz1myrij.bkt.clouddn.com/20180828174734.png)







 