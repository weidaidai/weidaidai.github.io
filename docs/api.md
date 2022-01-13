swagger

- info，提供API的元数据

- tags，补充的元数据，在swagger ui中，用于作为api的分组标签

- host，主机，如果没有提供，则使用文档所在的host

- basePath，相对于host的路径

- schemes，API的传输协议，http，https，ws，wss

- consumes，API可以消费的MIME类型列表

- produces，API产生的MIME类型列表

- paths，API的路径，以及每个路径的HTTP方法，一个路径加上一个HTTP方法构成了一个操作。每个操作都有以下内容：

- - tags，操作的标签
  - summary，短摘要
  - description，描述
  - externalDocs，外部文档
  - operationId，标识操作的唯一字符串
  - consumes，MIME类型列表
  - produces，MIME类型列表
  - parameters，参数列表
  - responses，应答状态码和对于的消息的Schema
  - schemes，传输协议
  - deprecated，不推荐使用
  - security，安全

- definitions，定义API消费或生产的数据类型，使用json-schema描述，操作的parameter和response部分可以通过引用的方式使用definitions部分定义的schema

- parameters，多个操作共用的参数

- responses，多个操作共用的响应

- securityDefinitions，安全scheme定义

- security，安全声明

- externalDocs，附加的外部文档

  ```yaml
  openapi: 3.0.0 #openapi版本
  # API描述信息
  #然后我们需要说明一下API文档的相关信息，比如API文档版本（注意不同于上面的规范版本）、API文档名称已经可选的描述信息。
  info:
    version: 1.0.0 
    title: student information
    description: Simple Student Information API
  servers:
    # Added by API Auto Mocking Plugin
    - description: SwaggerHub API Auto Mocking
      url: https://app.swaggerhub.com/apis/weidaidai/student/1.0.0++
  #路径    
  paths:
    /student/{type}:
      parameters: #多个操作共用的参数
        - name: type
          required: true 
          in: path #路径
          schema: #通过响应消息中的模式（schema）属性来描述清楚具体的返回内容
            enum: #枚举
              - mysql
              - redis
      post:
        description: Add student information
        requestBody:
          content:
            application/json:
              schema:
                 $ref: '#/components/schemas/student' #复用连接
        responses:
          200:
            $ref: '#/components/responses/okResp'
          400:
            $ref: '#/components/responses/badRequest'
          500:
              description: 服务器错误
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      ok:
                        type: boolean
                      error:
                        type: string
                    example:
                      ok: false
                      error: service internal error
                      
                      
                      
  components:
    responses:
      okResp:
        description: ok response
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/okResp'
            example:
              ok: true
      badRequest:
        description: bad request
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/errorResp'
            example:
              ok: false
              error: blah blah blah
                      
  ```

  

  

  