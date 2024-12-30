###### 框架租户使用笔记------------------------@yanghua--2023-05-23--------------------------------------

### 一、租户使用

#### 1、Infrastructure.Core的Repository中

* Shared的实体基类接口写上需要的字段。

* Repository中的Databases的Entities中的实体需要继承实体基类，实现公共字段

* 增加所需字段的管理器，用AsyncLocal存储。（**能够保证异步线程的数据保留，即每次请求的线程内都是单独的数据**）

* MultiFreeSql.cs中增加全局过滤。

  ```C#
  //1.租户授权机场四字码过滤
  Fsql.GlobalFilter.ApplyIf<ITenant>(
                        "TetradCodeFilter", // 航司四字码过滤器
                        () => TetradCodeManager.TetradCode != null, // 过滤器生效判断
                        a => TetradCodeManager.TetradCode.IndexOf(a.TetradCode) > 0 // 过滤器条件，是否包含四字码
                    );
  ```



#### 2、将租户id与机场四字码放入token

* (1)、GetJwtUserAsync中将所需变量放入oJwtUserDto实体中。
* (2)、CreateJwt方法中将oJwtUserDto中所需变量放入claims中，并生成token。



#### 3、拦截每次请求，将租户信息放入管理器

* (1)、Middlewares的TenantFilterHandleMiddleware中的Invoke方法中，从请求头中获取token并解析，将其放入管理器即可。



#### 4、禁用全局过滤（个人总结，使用请验证。）

* (1)、接口临时禁用方式：oSelect.DisableGlobalFilter("TenantFilter");
* (2)、字段忽略方式：[JsonProperty, Column(**IsIgnore =true**)]//忽略字段后，该字段的全局过滤将不生效，**但同时也无法查询该字段值**。
* (3)、实体类型限制方式：**Fsql.GlobalFilter.ApplyOnlyIf<ITetradCode>**。只有**实体类继承了ITetradCode**过滤才会生效。
* (4)、还有一种情况，如果使用的是**ApplyIf<T>**，实体类中没有过滤字段时，过滤不生效，但如果实体有过滤字段，即便**实体没有继承T类。过滤也会生效**！

