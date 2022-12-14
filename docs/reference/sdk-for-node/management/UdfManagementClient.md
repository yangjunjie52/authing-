
# 管理用户自定义字段

<LastUpdated/>


Udf 是 User Defined Field（用户自定义字段） 的简称。{{$localeConfig.brandName}} 的数据实体（如用户、角色、分组、组织机构等）可以添加自定义字段，你可以配置 {{$localeConfig.brandName}} 默认不自带的字段，比如你需要创建以一个学校相关的应用，就可以添加一个自定义 `school` 字段。

同时你可以在用户注册完成之后要求用户补充此字段的信息，[点此查看详情](/guides/authentication/extensibility/user-defined-field.md)。


请使用以下方式使用该模块：
```javascript
import { ManagementClient } from "authing-js-sdk"
const managementClient = new ManagementClient({
   userPoolId: "YOUR_USERPOOL_ID",
   secret: "YOUR_USERPOOL_SECRET",
})
managementClient.udf.list // 获取自定义字段元数据列表
managementClient.udf.set // 设置自定义字段
managementClient.udf.remove // 删除自定义字段
```


## 设置自定义字段元数据
> 设置自定义字段元数据，如果该字段不存在会自动创建。

```js
UdfManagementClient().set(targetType, key, dataType, label)
```



#### 参数

- `targetType` \<UdfTargetType\> 自定义字段目标类型， USER 表示用户、ROLE 表示角色。 
- `key` \<string\> 字段 key 
- `dataType` \<UdfDataType\> 数据类型，目前共支持五种数据类型。STRING 为字符串、NUMBER 为数字、DATETIME 为日期、BOOLEAN 为 boolean 值、object 为对象。 
- `label` \<string\> 字段 Label，一般是一个 Human Readable 字符串。 

#### 示例

```javascript
import { ManagementClient, UdfTargetType, UdfDataType  } from "authing-js-sdk"
const udf = await managementClient.udf.set(
   UdfTargetType.User,
   'school',
   UdfDataType.String,
   '学校'
);
```
```javascript
// 如果 age 这个自定义字段不存在，第一次会创建

import { ManagementClient, UdfTargetType, UdfDataType  } from "authing-js-sdk"
const udf = await managementClient.udf.set(
   UdfTargetType.User,
   'age',
   UdfDataType.Number,
   '年龄'
);

// 如果 age 字段之前创建过，会修改该字段的配置

const udf = await managementClient.udf.set(
   UdfTargetType.User,
   'age',
   UdfDataType.Number,
   '新的描述信息'
);
```

#### 返回值

-  `Promise<UserDefinedField[]>` 


      

## 删除自定义字段
> 删除自定义字段

```js
UdfManagementClient().remove(targetType, key)
```



#### 参数

- `targetType` \<UdfTargetType\> 自定义字段目标类型， USER 表示用户、ROLE 表示角色。 
- `key` \<string\> 字段 key 

#### 示例

```javascript
await managementClient.udf.remove(UdfTargetType.User, 'school');
```

#### 返回值

-  `Promise<UserDefinedField[]>` 

## 获取自定义字段定义
> 查询用户池定义的自定义字段

```js
UdfManagementClient().list(targetType)
```



#### 参数

- `targetType` \<UdfTargetType\> 自定义字段目标类型， USER 表示用户、ROLE 表示角色。 

#### 示例

```javascript
const list = await managementClient.udf.list(UdfTargetType.User);
```

#### 返回值

-  `Promise<UserDefinedField[]>` 



## 获取自定义字段数据列表
> 获取某一实体的自定义字段数据列表

```js
UdfManagementClient().listUdv(targetType, targetId)
```



#### 参数

- `targetType` \<UdfTargetType\> 自定义字段目标类型， USER 表示用户、ROLE 表示角色。 
- `targetId` \<string\> 自定义字段目标 id，如用户 ID。

#### 示例

```javascript
const list = await managementClient.udf.listUdv(UdfTargetType.User, 'userId');
```

## 批量添加自定义数据
> 批量添加自定义数据

```js
UdfManagementClient().setUdvBatch(targetType, targetId)
```

#### 参数

- `targetType` \<UdfTargetType\> 自定义字段目标类型， USER 表示用户、ROLE 表示角色。 
- `targetId` \<string\> 自定义字段目标 id，如用户 ID。
- `udvList` \<object []\> 自定义字段数组。
- `udvList.key` \<string\> 自定义字段的 key。
- `udvList.value` \<string\> 自定义字段的值。

#### 示例

```javascript
const list = await managementClient.udf.setUdvBatch(UdfTargetType.User, 'userId', [{key: 'school', 'value': '清华大学'}]);
```
