Мини отчет по MongoDb
============================
Использую готовый тестовый стенд с mongoDb.

Примеры запросов:
------------------
 * Пример 1.
db.getCollection('orders').find({$and:[{'price.total':{$gte:238}},{'price.total':{$lte:239}}]}).count()
Нашел количество заказов на сумму от 238 до 239 включительно

* Пример 2.
db.getCollection('orders').find ({ $and : [{"cancel": null} , {"pay.paid": true}]}).count()
Нашел количество оплаченных заказов, которые не были отменены

* Пример 3.
db.getCollection('orders').aggregate([{$group: { _id: "$client"}}])
Нашел все вариации значений клиентов

* Пример 4.
db.getCollection('orders').aggregate([{$group: { _id: "$shop_group.name", count:{ $sum: 1 }}}, {$sort: {count: -1 }}, {$limit: 1}])
Нашел магазин с наибольшим числом заказов
проверил через:
db.getCollection('orders').find({"shop_group.name":"Пятёрочка" }).count()
* Пример 5.
db.users.updateOne({name : "admin", role: "admin"}, {$set: {role : "guest"}})
Изменение роли учетной записи admin
* Пример 6.
db.products.updateOne({name : "Колбаса Докторская"}, {$unset: {promo: true}})
удаление поля промо у товара


Создание индексов:
-------------------
Запрос выполняется ~3s :
```
db.users.find({})
  .sort({ name: 1 })
  .limit(1)
  .explain('allPlansExecution')
```
  
Запрос с сокращенным временем выполнения до ~1ms :
```
db.users.createIndex({"name" : 1})

db.users.find({})
  .sort({ name: 1 })
  .limit(1)
  .explain('allPlansExecution')
```
