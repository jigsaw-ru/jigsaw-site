---
extends: _layouts.documentation
section: documentation_content
---

## Пользовательская страница 404

Вы можете создать настраиваемую страницу ошибки 404 для отображения, когда кто-то пытается получить доступ к несуществующей странице Вашего сайта. Как Вы это делаете, зависит от того, где размещен Ваш сайт.

### Использование GitHub Pages или Netlify

Некоторые хосты, такие как GitHub Pages и Netlify, автоматически настраиваются для поиска файла с именем `404.html` на корневом уровне Вашего сайта. Если Ваш сайт Jigsaw использует [красивые URL-адреса](/docs/pretty-urls), Вы можете указать `permalink` в файле для своей пользовательской страницы 404, чтобы сохранить расширение `.html`:

> _source/404.md_

```
---
extends: _layouts.master
section: content
permalink: 404.html
---

### Извините, данная страница не существует.
```

Обратите внимание, что передняя часть YAML также может использоваться в файлах Blade, поэтому Вы можете сделать то же самое, используя файл Blade с именем `404.blade.php`.

Это создаст файл с именем `404.html` в каталоге `build` Вашего сайта.

### Использование сервера Nginx

Вы можете создать свой собственный файл 404 как `404.md` или `404.blade.php` в Вашем каталоге `source`, и если Ваш сайт Jigsaw использует [красивые URL-адреса](/docs/pretty-urls), он будет выводится как `/404/index.html`:

> _source/404.md_

```
---
extends: _layouts.master
section: content
---

### Извините, данная страница не существует.
```

При размещении Вашего сайта на сервере Nginx Вам необходимо настроить параметр `error_page` в файле `nginx.conf` Вашего сервера или в конкретном файле конфигурации, который Nginx использует для Вашего сайта. Эти файлы конфигурации обычно находятся в `/etc/nginx/`, хотя их расположение зависит от сервера. Если Ваш сайт управляется с помощью Laravel Forge, например, файл конфигурации для Вашего сайта будет расположен в `/etc/nginx/sites-enabled/{name-of-your-site}`; его также можно редактировать с помощью параметра Forge «Редактировать конфигурацию Nginx» в меню «Файлы».

Найдя файл конфигурации Nginx, добавьте следующую строку в блок `server`:

```
error_page 404 /404;
```

Кроме того, если его там еще нет, убедитесь, что следующая строка отображается в разделе Вашего файла конфигурации, который начинается с `location ~ \.php$ {`:

```
fastcgi_intercept_errors on;
```

После перезапуска сервера Nginx он будет искать страницу с ошибкой `/404/index.html` в Вашем каталоге `build` всякий раз, когда кто-то переходит на страницу, которая не существует.
