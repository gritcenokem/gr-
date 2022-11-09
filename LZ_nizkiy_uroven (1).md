###### 1. Нежурналируемая таблица
Создала нежурналируемую таблицу в пользовательском табличном пространстве:
![avatar](https://sun9-58.userapi.com/impg/7oMnVqFmHO9y2GeN2_dJXTr078QwU7SLgmzNLA/Ot9pzVR7KNQ.jpg?size=609x179&quality=96&sign=49f04289dfea3c1cfac065f146e56ede&type=album)
Убедилась, что для таблицы существует слой init:
![avatar](https://sun9-44.userapi.com/impg/8uqv9c89DOU-DkVoti0DECBun7J0w83T-KTx4g/VSSfz64heUg.jpg?size=681x72&quality=96&sign=d50abf0f12ba43b54424330e2e39c851&type=album)
Удалила созданное пространство:
![avatar](https://sun9-73.userapi.com/impg/zHRT5AetbkcUephQ2QcdlqEF1vHwk4-27bCsCw/am-G4AD-Ssk.jpg?size=615x102&quality=96&sign=bb637c2d5ba21bf5507219bfde2dc23d&type=album)
###### 2. Таблица с текстовым столбцом
Создала таблицу со столбцом типа text:
![avatar](https://sun9-88.userapi.com/impg/OsMteB8kWR23UcK131MS3WklUv0ufKmOPQLg3w/Gb1HTohg_bY.jpg?size=672x137&quality=96&sign=320771f3ed02c9fc6853d4b1031416e6&type=album)
По умолчанию для типа text используется стратегия extended. 
Я изменила стратегию на external:
![avatar](https://sun9-45.userapi.com/impg/kuGe9_82XE5d72xd89WpkiHFjzKT9Ab3gNkvOA/QozCY8CbJoU.jpg?size=670x137&quality=96&sign=47e868fefa15b8fa9beff4027ef7d232&type=album)
Далее вставила в таблицу длинную и короткую строки:
![avatar](https://sun9-81.userapi.com/impg/WNvmhX0l54CmPsBpIZp914DbGmA8XlpK6m0R-A/uOsTjLXhJ5s.jpg?size=559x69&quality=96&sign=50a3a5d9cc7a4d2ea7475ba176b313db&type=album)
Проверила, попали ли строки в toast-таблицу, выполнив прямой запрос к ней:
![avatar](https://sun9-41.userapi.com/impg/MMvnLB5jh4aH001VjcSY2AXh0jcusIlQtMHw2g/OG2L88FQKns.jpg?size=680x248&quality=96&sign=f9194dc243fd96ffc77c5d0912b6042f&type=album)
Видно, что в TOAST-таблицу попала только длинная строка (два фрагмента, общий размер совпадает с длиной строки). Короткая строка не вынесена в TOAST просто потому, что в этом нет необходимости — версия строки и без этого помещается в страницу.
