# Инструменты для автоматического заполнения 3-НДФЛ на nalog.ru

Так как по дивидендам иностранных компаний налоговым агентом является физлицо, то приходится самостоятельно вносить информацию о них в декларацию 3-НДФЛ и самостоятельно платить налоги.
Также инструмент может быть полезен тем, кому надо заполнить в 3-НДФЛ доходы связанные с зарплатой за пределами РФ.

Для этого можно пытаться автоматически генерировать отчет в формате ПО "Декларация", либо вносить эту информацию в форму на nalog.ru, что умеет делать nalog.ipynb

## Автор

Alexander Gerasiov <a@gerasiov.net>

https://github.com/gerasiov/3ndfl

## Лицензия 
GPL версии 2 или более поздняя.

## Donate here

* BTC: bc1qjxet0e0u5gzufz40dvcl7nxu3jx7p0du7a5pgd
* ETH: 0x8769ec4b48b9bb0aafaa8e54f5139611f3cb0a89
* Russian Cards: https://www.tinkoff.ru/cf/4PWqTbN1xiv
* Paypal: a@gerasiov.net


## Инструменты

В этом репозитории содержится два инструмента: tinkoff.ipynb и nalog.ipynb.

Оба скрипта сделаны в виде jupyter notebook, во-первых, чтобы видеть код, который выполняется (всё-таки доступ к финансам и госуслугам), во-вторых, при необходимости такой вариант легче отладить.

Для простоты и надежности можно запускать ноутбуки в venv, чтобы не ставить зависимости в систему:
```
virtualenv 3ndfl-venv
. ./3ndfl-venv/bin/activate
pip install notebook ipykernel
python -m ipykernel install --user --name=3ndfl-venv
jupyter notebook tinkoff.ipynb
```

Также убедитесь что у вас установлен webdriver (можно скачать с официального сайта или поставить через пакетный менеджер)

После окончания работы можно удалить ipykernel и venv:
```
deactivate
jupyter kernelspec remove 3ndfl-venv
rm -rf 3ndfl-venv
```

## tinkoff.ipynb

Jupyter notebook с темплейтом кода, который подключается к Broker API Tinkoff, забирает оттуда информацию о дивидендах и формирует файл, который умеет читать nalog.ipynb

## nalog.ipynb

Jupyter notebook который умеет запускать Chrome/Chromium через selenium и заполнять в нем форму 3-НДФЛ на сайте nalog.ru

Обратите внимание, что для того, чтобы введенные данные сохранились, надо убедиться, что все доходы оформлены правильно и нажалась кнопка Далее. Но, если вы только начали заполнять декларацию, кнопка "Далее" может отказаться нажиматься. В таком случае необходимо проверить, чего ей не хватает и исправить ошибки в форме. (Например может потребоваться указать вариант налогового вычета для каких-то доходов, импортированных из 2-НДФЛ.)

## Формат файла

Файл который принимает на вход `nalog.ipynb` можем быть вами подготовлен самостоятельно вручную или сторонними инструментами.

Это должнен быть tsv (tab separated file, как csv но с табами вместо запятых) со следующим порядком колонок:
1. Дата, в формате ДД.ММ.ГГГГ
2. Страна источника выплаты
3. Страна зачисления выплаты
4. Тип дохода
5. Код валюты в которой получен доход
6. Сумма (**с точкой в качестве дробного разделителя**)
7. Сумма уплаченного налога (**с точкой в качестве дробного разделителя**)
8. Наименование истоничка дохода

Значение для стран, типа дохода и валюты должны быть указаны в справочниках который можно найти в одной из ячеек тетрадки (ищите по `get_country_code`). Если нужных вам значений нет - добавьте их в справочники.

## Возможные проблемы и способы их решения

1. Если браузер развернуть на весь экран, могут возникнуть сбой с поиском элементов формы. Поэтому во время заполнения данных, воздержитесь от изменения его размеров
