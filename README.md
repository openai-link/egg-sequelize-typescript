# @openai-link/egg-sequelize-ts

## Statement

**Long Time Non-updated!!!**

This repo is just an upgrade for [egg-sequelize-ts]('https://github.com/stone-lyl/egg-sequelize-ts'), as [egg-sequelize-ts]('https://github.com/stone-lyl/egg-sequelize-ts') uses very old sequelize and typescript related packages.

## Purpose

Use typescript and sequelize to build models (ORM) in egg.js

## Install

```bash
npm i --save @openai-link/egg-sequelize-ts
# or
yarn add @openai-link/egg-sequelize-ts
```

## Use

- Enable plugin in `config/plugin.ts`

```ts
    sequelize: {
        enable: true,
        package: '@openai-link/egg-sequelize-ts'
    }
```

- Edit your configurations in `conif/config.{env}.ts`

```ts
config.sequelize = {
  dialect: "mysql",
  host: "127.0.0.1",
  port: 3306,
  database: "database",
};
```

## Example

Create a model (table) `User`

```ts
// app/model/User.ts

/**
 * @desc User Table
 */
import {
  AutoIncrement,
  Column,
  DataType,
  Model,
  PrimaryKey,
  Table,
} from "sequelize-typescript";
@Table({
  modelName: "user",
})
export class User extends Model {
  @PrimaryKey
  @AutoIncrement
  @Column({
    type: DataType.INTEGER(11),
    comment: "用户ID",
    comment: "user id",
  })
  id: number;

  @Column({
    comment: "用户姓名",
  })
  name: string;

  @Column({
    comment: "用户邮箱",
  })
  email: string;

  @Column({
    comment: "用户手机号码",
  })
  phone: string;

  @Column({
    field: "created_at",
  })
  createdAt: Date;

  @Column({
    field: "updated_at",
  })
  updatedAt: Date;
}
export default () => User;
```

```js
// app/service/user.js
import { Service } from "egg";
import { Sequelize } from "sequelize-typescript";

class UserService extends Service {
  async index() {
    const { or } = Sequelize.Op;
    this.ctx.model.User.create({
      name: "A",
      email: "devilyouwei@gmail.com",
      phone: "7322680284",
    });
    const users = await this.ctx.model.User.findOne({
      where: {
        [or]: [{ name, phone }, { id }],
      },
    });
    this.ctx.body = users;
  }
}
```
