# ydr-validator
validator for ydr.me


# usage
```
npm install -S ydr-validator
```
browser 端使用__[alien/libs/Validator](https://github.com/cloudcome/alien/blob/master/src/libs/Validator.js)__，
API全部一致，因此可以通用。


# API

## options
```
{
    // true: 返回单个错误对象
    // false: 返回错误对象组成的数组
    // 浏览器端，默认为 false
    // 服务器端，默认为 true
    isBreakOnInvalid: true
}
```

## #pushRule
```
var validator = new Validator(options);

validator.pushRule({
   // 字段名称，必须，唯一性
   name: 'username',
   // 数据类型，必须，为 string/email/url/number/boolean 之一
   type: 'string',
   // 别称，当没有填写自定义错误消息时
   // 提示为 username不能为空
   // 当前设置别名之后
   // 提示为 用户名不能为空
   alias: '用户名',
   // 验证前置
   // val 当前字段值
   // data 所有数据
   onbefore: function(val, data){
       return val + 'abc';
   },
   exist: false,
   trim: true,
   // 验证规则
   required: true,
   length: 10,
   minLength: 4,
   maxLength: 12,
   bytes: 10,
   minBytes: 4,
   maxBytes: 12,
   min: 4,
   max: 4,
   regexp: /^[a-z]\w{3,11}$/,
   equal: 'cloudcome',
   inArray: ['cloudcome', 'yundanran'],
   // val 当前字段值
   // [data 所有数据] 可选
   // next 执行下一步
   function: function(val, next){
       // 这里可以是异步的
       // 比如远程校验可以写在这里
       ajax.post('./validate.json', {
           username: val
       }).on('success', function(json){
           if(json.code>0){
               next();
           }else{
               next(new Error(json.msg));
           }
       }).on('error', function(){
           next(new Error('网络连接错误，验证失败'));
       });
   },
   // 验证消息
   msg: {
       required: '用户名不能为空',
       length: '用户名长度必须为10'
       // 未自定义的消息，以默认输出
       // 每个验证规则都必须配备一个消息体，
       // 除了自定义的`function`
   },
   // 验证后置
   onafter: function(val){
       return val + 'abc';
   }
});
```


## #validateAll
```
validator.validatorAll({
	username: 'hehe',
	password: 'haha'
}, function(err){
	// do what
});
```


## #validateOne
```
validator.validatorOne({
	username: 'hehe'
}, function(err){
	// do what
});
```


# validator rules
- `name` 表单名称，必须
- `type` 数据类型，必须，包括：`strin`g、`number`、`url`、`email`
- `alias` 别名，如`username`别名为`用户名`，用于表单消息本地化
- `onbefore` 表单验证前置方法
- `exist=false` 是否存在值时才进行验证
- `trim=true` 是否进行`trim`后才进行验证
- `required` 是否必填
- `length` 字符长度
- `minLength` 字符最小长度
- `maxLength` 字符最大长度
- `bytes` 字节长度
- `minBytes` 最小字节长度
- `maxBytes` 最大字节长度
- `min` 最小值
- `max` 最大值
- `regexp` 正则表达式
- `equal` 全等于某值
- `inArray` 指定值
- `function` 指定验证规则
- `onafter` 表单验证后置方法
- `msg` 表单验证消息
- `isOverride=false` 是否覆盖之前存在的规则