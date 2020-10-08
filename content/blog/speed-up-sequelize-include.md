+++
title = "How to speed up Sequelize with complicated includes"
date = "2020-10-07T22:16:46+02:00"
description = "How to speed up Sequelize query  with many complicated includes"

tags = ["code", "note"]
+++

Sometimes queries can get complicated with many include statements. This can significantly slow down the response.
```js
const stageWithMeta = await db.stage.findOne({
      where: { id },
      include: [
        {
          model: db.workTimeLog,
          include: [{ model: db.user }],
        },
        {
          model: db.incomeExpense,
        },
      ],
    });
```
One way to make thing faster is to add `separate: true` to the `include`.

```js
const stageWithMeta = await db.stage.findOne({
      where: { id },
      include: [
        {
          model: db.workTimeLog,
          separate: true,
          include: [{ model: db.user }],
        },
        {
          model: db.incomeExpense,
          separate: true,
        },
      ],
    });
```

This is only supported for hasMany associations.



[doc](https://sequelize.org/master/class/lib/model.js~Model.html)

> If true, runs a separate query to fetch the associated instances, only supported for hasMany associations

Sequelize will run separate queries and merge the result in memory, instead of making a huge query with many `JOIN` statements.
  



