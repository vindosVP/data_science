# ЛР 3
> Валов Вадим М80-108Б-22

### Деплой всех ресурсов

>Используем `kubectl apply`, видим все все создалось (`kubectl get`), 

![deploy](screens/1.png)

#

>Далее проверяем дашборд

![dashboard1](screens/2.png)

![dashboard2](screens/3.png)

![dashboard3](screens/4.png)

#

### Получаем доступ к ресурсам

> Изменил тип `redis-service` на NodePort 

Получаем url сервиса

![url](screens/5.png)

>Далее с помощью `python` работаем с redis и проверяем, что все работает верно

![redis](screens/6.png)
