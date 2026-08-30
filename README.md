# НЕФТЕБАЗА ЧЕРЕПАНОВО — статическая версия сайта

Экспорт из Tilda, переработанный так, чтобы работать без Apache/`.htaccess` —
на GitHub Pages, Netlify, Cloudflare Pages или просто из файловой системы.

## Структура

```
index.html            главная
details.html          Реквизиты (было: /details → page50056539.html)
privacy.html          Политика обработки персональных данных (было: /privacy)
404.html              страница ошибки
page50023219.html     Header (служебный блок Tilda, закрыт в robots.txt)
page50037671.html     Footer (служебный блок Tilda, закрыт в robots.txt)
css/ js/ images/      ассеты, все ссылки относительные
files/                тела страниц из экспорта Tilda, нигде не используются
.nojekyll             отключает обработку Jekyll на GitHub Pages
```

## Деплой на GitHub Pages

1. Создать репозиторий и залить содержимое этой папки в корень ветки `main`.
2. Settings → Pages → Source: **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Через минуту сайт будет доступен по адресу
   `https://<username>.github.io/<repo>/`.

Все пути относительные, поэтому сайт одинаково работает и в корне домена,
и в подпапке `/<repo>/`.

## Что изменено по сравнению с экспортом Tilda

- Удалён `htaccess` — на GitHub Pages он не работает.
- ЧПУ-адреса `/details` и `/privacy` держались на `RewriteRule`. Страницы
  переименованы в `details.html` и `privacy.html`, все ссылки обновлены.
- `DirectoryIndex page50022231.html` заменён на существующий `index.html`.
- Ссылки от корня домена (`href="/"`, `href="/#products"` и т.п.) переписаны
  на относительные — иначе в подпапке `/<repo>/` они вели бы на корень
  `github.io`. Внутри главной якоря стали локальными (`#products`),
  на остальных страницах — `index.html#products`.
- Жёсткие ссылки `https://cherepanovo.tilda.ws/privacy` в подвале заменены
  на локальные.
- `canonical` и `og:url` больше не указывают на старый домен `tilda.ws`.
- `404.html` вместо заглушки Tilda — своя страница, без внешних ассетов.
- `sitemap.xml` удалён (содержал адреса старого домена), из `robots.txt`
  убраны `Host:` и `Sitemap:` со старым доменом.
- Добавлен `.nojekyll`.

## Что нужно сделать после публикации

**Абсолютный адрес сайта.** Когда будет известен финальный URL, стоит
проставить его в `canonical`/`og:url` и вернуть sitemap:

```bash
SITE="https://username.github.io/repo"
sed -i "s|<link rel=\"canonical\" href=\"./\">|<link rel=\"canonical\" href=\"$SITE/\">|" index.html
sed -i "s|content=\"./\"|content=\"$SITE/\"|" index.html
```

**Формы обратной связи.** Формы Tilda (`.js-form-proccess`) отправляются
скриптом `js/tilda-forms-1.0.min.js` на `forms.tildacdn.com` с ключом проекта.
Вне Tilda это работать не будет — нужен внешний обработчик
(Formspree, Getform, FormSubmit) или `mailto:`. Телефон и почта на сайте
работают в любом случае.

**Яндекс.Метрика.** Счётчик в коде остался и продолжит слать данные; если
проект новый, стоит поменять номер счётчика или удалить блок.
