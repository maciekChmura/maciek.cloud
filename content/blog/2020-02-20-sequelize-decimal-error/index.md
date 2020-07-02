---
title: Sequelize decimal type error
date: '2020-02-20'
template: 'post'
draft: false
url: '/sequelize-decimal-type-error/'
category: 'JavaScript'
tags:
  - 'JavaScript'
description: "Sequelize returns a string instead of a number for decimal type"
---

Sequelize is probably the most popular ORM for Express. It helped me to quickly start with a NodeJS server and a Postgres database in my current side project.
Unfortunately I encountered a strange issue when I wanted to introduce decimal numbers to one of my models.

Sequelize in version 5.21.3 has an error with decimal type.

My model looked like this:

```js
module.exports = (sequelize, DataTypes) => {
  const incomeExpense = sequelize.define(
    'incomeExpense',
    {
      id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
      },
      value: DataTypes.DECIMAL(10, 2),
      description: DataTypes.STRING,
      date: DataTypes.DATEONLY,
      type: DataTypes.STRING
    },
    { freezeTableName: true }
  );
  incomeExpense.associate = function(db) {
    incomeExpense.belongsTo(db.stage);
  };
  return incomeExpense;
};
```

Data in Postgres:

|   id   | value |description |date | type |stageId |
|---|---|---|---|---|---|
| 6 |120.00|invoice 1|2019-11-11|income|	3|
| 7 |	120.33|	invoice 2|	2019-11-11|	income|		3|

JSON response:
```json
[
    {
        "id": 6,
        "value": "120.00",
        "description": "invoice 1",
        "date": "2019-11-11",
        "type": "income",
        "createdAt": "2019-11-10T23:00:00.000Z",
        "updatedAt": "2019-11-10T23:00:00.000Z",
        "stageId": 3
    },
    {
        "id": 7,
        "value": "120.33",
        "description": "invoice 2",
        "date": "2019-11-11",
        "type": "income",
        "createdAt": "2020-02-06T16:41:36.868Z",
        "updatedAt": "2020-02-06T16:41:36.868Z",
        "stageId": 3
    }
]
```
The returned `value` is a string type.

I thought that the conversion from a number to a string heppens somewhere in Node or React. As it turns out, it is the model itself. 


Model after changing the `value` to `DataTypes.FLOAT`:
```js
module.exports = (sequelize, DataTypes) => {
  const incomeExpense = sequelize.define(
    'incomeExpense',
    {
      id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
      },
      // value: DataTypes.DECIMAL(10, 2),
      value: DataTypes.FLOAT,
      description: DataTypes.STRING,
      date: DataTypes.DATEONLY,
      type: DataTypes.STRING
    },
    { freezeTableName: true }
  );
  incomeExpense.associate = function(db) {
    // associations can be defined here
    incomeExpense.belongsTo(db.stage);
  };
  return incomeExpense;
};

```
Postgres dropped the trailing zeros:


|   id   | value |description |date | type |stageId |
|---|---|---|---|---|---|
| 6 |120|invoice 1|2019-11-11|income|	3|
| 7 |	120.33|	invoice 2|	2019-11-11|	income|		3|

And now the `value` in response is a number:
```json
[
    {
        "id": 6,
        "value": 120,
        "description": "invoice 1",
        "date": "2019-11-11",
        "type": "income",
        "createdAt": "2019-11-10T23:00:00.000Z",
        "updatedAt": "2019-11-10T23:00:00.000Z",
        "stageId": 3
    },
    {
        "id": 7,
        "value": 120.33,
        "description": "invoice 2",
        "date": "2019-11-11",
        "type": "income",
        "createdAt": "2020-02-06T16:41:36.868Z",
        "updatedAt": "2020-02-06T16:41:36.868Z",
        "stageId": 3
    }
]
```

This issue is opened, and there is an ongoing discussion:
[link](https://github.com/sequelize/sequelize/issues/8019)

For now, I can't think of a better fix than Changing a `DECIMAL` to a `FLOAT`.



