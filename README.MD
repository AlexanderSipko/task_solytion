
# UPLOAD FILE
 - задача описана в файле [полученный по API kaggle](task.txt)
## установить окружение и библиотеки:
    >>> python -m venv .venv
    >>> . .venv/bin/activate
    >>> pip install -r requirements.txt

    if use macos install postgresql ()
    >>> brew update
    >>> brew install postgresql
    >>> brew services start postgresql

## загрузить файлы с kaggle:
    1. в файле `kaggle.json` (нет в репозитории)  указать token [полученный по API kaggle](https://www.kaggle.com/docs/api)
        {"username":"username","key":"key"}

    2. в файле `conf.json` (нет в репозитории) указать параметры подключения к базе postgresql:
        {
            "DB":"dbname",
            "LOGIN":"login",
            "PASSWORD":"password",
            "HOST":"localhost",
            "PORT":5432
        }
    
    3. в скрипте команда отвечает за загрузку >>> `od.download(URL, force=True)`

## Чтение файлов и логика загрузки в базы таблицы cyclones_history

    1. если в базе нет таблицы она создается
    2. все папки создаются и очищаются при запуске скрипта

## Описание логики работы
    1. получаем данные из kaggle
    2. формируем регион дат от заданной даты
    3. очищаем директорию для загрузки файлов или создаем ее если она не существует
    4. для каждой записи создаем файл в директории
    5. получаем список всех файлов директории
    6. проходим по каждоуму файлу
    7. проверяем дубли записи если они заливаются повторно
    8. если есть то удаляем запись и изменяем дату у предудущей записи на null
    9. проверяем наличие данного id в базе если нет то заливаем
    10. проверяем статус у последней записи, если он отличин от текущего в файле то
        - добавляем дату окончания у последней записи на вчера
        - заносим текущую запись в базу

## примечание
    1. при повторной загрузки последнего дня, проверка отрабатывает и изменят значения
    2. при загрузки диапазона больше чем текущий день данные могут буть обработанны не корректно, в связи с изменением даты у множества строк
