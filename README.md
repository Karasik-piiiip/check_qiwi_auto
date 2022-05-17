# check_qiwi_auto
Модуль для выставления счетов и их автоматической проверки через QiwiAPI.
# Создание ключей доступа
Перед работой с модулем необходимо создать публичный и секретный ключи доступа.
Для этого переходим на p2p.qiwi.com, авторизумся и переходим к вкладке "Прием переводов -> API".
Затем нажимаем "Создать пару ключей и настроить" и сохраняем оба ключа.

![param](https://sun9-43.userapi.com/impg/LfY6qRfXaqG4D4jHg28WfIkuBhlVgCoy1N-TaQ/esWtXgZktLU.jpg?size=582x482&quality=96&sign=cbecbb4ea26c627d39ab9e0fa4afdbef&type=album)

После этого в папке с проектом необходимо создать файл ```config.txt```, и в первую его строчку вставить Public Key, а во вторую Secret Key.

# Пример использования
Затем переходим к созданию системы автоматического выставления счетов и их проверки.
```
import check_qiwi
from random import randint

Check = check_qiwi.Check(public_key, secret_key) # Создаём экземпляр класса.
user = randint(1, 99999999) # Уникальный номер пользователя, который должен оплатить счёт.
print(Check.create_paylink(amount=49, user=user, comment='За вкусный кофе')) # Вывод ссылки.
input('Нажмите любую клавишу для проверки оплаты') # Ожидание ввода с клавиатуры.
if Check.check_pay(user):
    print('Счёт успешно оплачен')
else:
    print('Счёт не был оплачен!')
```
Функция ```create_paylink()``` принимает 2 обязательных параметра ```amount``` (стоимость счёта в рублях) и ```user``` (уникальный номер клиента), а также опциональный параметр ```comment``` как комментарий к счёту.

# Важно!!!
Во время работы программа создает файл ```bill_ids.sqlite3```, который хранит уникальные billid каждого платежа. Без них нельзя будет проверить, оплачен ли был тот или иной выставленный счёт, поэтому удалять этот файл ни в коем случае нельзя.
