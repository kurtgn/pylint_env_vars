# Pylint Env Vars

Плагин для Pylint, который запрещает использование `os.environ` в проекте.

## Зачем?

Когда ссылки на переменные окружения разбросаны по большой кодовой базе 
в разных местах, становится сложно предсказывать поведение приложения. 

Особенно это мешает, если в проекте реализован паттерн настроек a la Django 
с несколькими файлами `base.py`, `prod.py`, `dev.py` и т.д.,
каждый из которых тоже опирается на переменные окружения.

Плагин `pylint_env_vars` позволяет запретить использование os.environ во всех файлах, кроме избранных.

## Установка и настройка

```
pip install pylint_env_vars
```

Добавить в `.pylintrc`, в секцию `[MASTER]`, плагин:

```
[MASTER]
load-plugins = pylint_env_vars
```

**Если в каком-то модуле таки разрешено обращаться к `os.environ`:**

Добавить в `.pylintrc` секцию `[pylint_env_vars]` c этим модулем. 
Это может быть регулярное выражение:

```
[pylint_env_vars]
allow_in_modules = _devtools.*restore

```


Ошибка, которую выдает чекер, - `R9000`:

```
...
getmybot/settings/my.py:97: [R9000(os-environ-prohibited), ] Usage of os.environ is prohibited. Import from django settings instead.
...
```


Как запустить pylint на все файлы и папки текущей директории:

```
find . -name "*.py" | xargs pylint
```

Как сделать игнор папки для всего pylint (не только для этого чекера):

```
find . -name "*.py" | grep -v "node_modules" | xargs pylint
```



## Roadmap

Расширить проверки на модуль `django-environ`.
