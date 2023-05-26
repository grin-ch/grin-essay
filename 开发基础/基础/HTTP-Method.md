#### HTTP 中 Method 的区别和含义

- **安全方法(Safe Methods)**:是指用户不管进行多少次操作,资源的状态都不会改变.如:

  `GET`和`HEAD`方法仅能获取资源,而不会执行动作(`action`),这些方法属于"安全"的方法.

  而`POST`,`PUT`,`DELETE`方法改变了资源的状态,可能会执行不安全的动作,使用者应该意识到这一点.

- **幂等(Idempotent Methods)**: 相同的请求,其结果不会因不同的次数而改变.

##### HTTP1.1的方法(当前版本)

| Method  | 幂等 | 安全 |
| ------- | ---- | ---- |
| OPTIONS | yes  | yes  |
| GET     | yes  | yes  |
| HEAD    | yes  | yes  |
| PUT     | yes  | no   |
| POST    | no   | no   |
| DELETE  | yes  | no   |
| PATCH   | no   | no   |

