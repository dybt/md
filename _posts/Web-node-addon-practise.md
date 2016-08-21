title: node addon功能实践
date: 2016-07-10 11:29:16
tags:
- Node
- Web
---
### 预备
+ 作用
  - 运用node作web应用时，可以用js处理一切，包括计算，存储，I/O请求处理，网络请求处理等，但是会面临一个问题，就是在计算与存储方面，js的性能并不是很高，那么addon就来了，其是用c++来编写的，性能很高，很适用于做存储与计算这种需要高性能的功能
  - 参考 [官网](https://nodejs.org/api/addons.html)
<!-- more -->
+ 步骤（直接参考官方文档）
  - 安装node-gyp `npm install -g node-gyp`
  - 创建模块需要的目录src `mkdir -p hello/src`
  - 在src目录下创建hello.cc文件  
  ```c++
  // hello.cc
  #include <node.h>

  namespace demo {

  using v8::FunctionCallbackInfo;
  using v8::Isolate;
  using v8::Local;
  using v8::Object;
  using v8::String;
  using v8::Value;

  void Method(const FunctionCallbackInfo<Value>& args) {
    Isolate* isolate = args.GetIsolate();
    args.GetReturnValue().Set(String::NewFromUtf8(isolate, "world"));
  }

  void init(Local<Object> exports) {
    NODE_SET_METHOD(exports, "hello", Method);
  }

  NODE_MODULE(addon, init)

  }  // namespace demo
  ```

+ 在hello目录下创建node-gyp的配置文档
  ```
  {
    'targets': [
      {
        'target_name': 'addon',
        'sources': [
          'src/hello.cc'
        ],
        'dependencies': [
        ]
      }
    ]
  }
  ```
+ 在hello目录下执行`node-gyp configure` `node-gyp build`，成功后进行测试
+ 在hello目录下建test.js
  ```javascript
  // hello.js
  const addon = require('./build/Release/addon');

  console.log(addon.hello()); // 'world
  ```
+ 直接`node test.js` 可以看到结果