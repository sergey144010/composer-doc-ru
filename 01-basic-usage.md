[Оригинал](https://github.com/composer/composer/blob/master/doc/01-basic-usage.md)
[Оглавление](https://github.com/sergey144010/composer-doc-ru/blob/master/README.md#Оглавление)

# Базовое использование

## Введение

Для вводного ознакомления мы установим библиотеку логирования `monolog/monolog`.
Если Вы ещё не установили Composer, обратитесь к главе [Введение](00-intro.md).

> **Примечание:** для простоты в этом ознакомлении будем считать, что Вы
выполнили [локальную](00-intro.md#locally) установку Composer

## `composer.json`: Настройка проекта

Чтобы начать использовать Composer в Вашем проекте, всё, что Вам нужно это `composer.json`
файл. Этот файл описывает зависимости Вашего проекта и может содержать также другие метаданные.

### Ключ `require`

Первым (и часто единственным) делом укажите в `composer.json` ключ
[`require`](04-schema.md#require). Таким образом Вы просто говорите Composer от каких
пакетов зависит Ваш проект.

```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

Как Вы можете видеть [`require`](04-schema.md#require) принимает объект состоящий из
**имени пакета** (например `monolog/monolog`) и **версии пакета** (например `1.0.*`).

### Имена пакетов

Имя пакета состоит из имени поставщика (vendor) и имени проекта (project name). Часто они будут идентичны - имя поставщика просто существует для предотвращения столкновения наименований. Это позволяет двум разным людям, создать библиотеку с одинаковым именем `json`, которая затем будет просто названа `igorw/json` и `seldaek/json`.

Здесь мы требуем пакет `monolog/monolog`, поэтому имя поставщика совпадает с именем проекта. Для проектов с уникальным именем такой подход рекомендуется. Это также позволяет позже добавить более смежные проекты в рамках того же пространства имен. Если Вы работаете с библиотекой, это сделает её более легкой и удобной к разделению на меньшие части.

### Версии Пакетов

В предыдущем примере мы требуем версию [`1.0.*`](http://semver.mwl.be/#?package=monolog%2Fmonolog&version=1.0.*) Monolog.
Это означает любая версия `1.0` в ветке разработки (development branch). Это
эквивалентно сказанному - версия которая соответствует `>=1.0 <1.1`.

Ограничения для версий можно указать несколькими способами, читайте
[версии](articles/versions.md) для получения более подробной информации по этой теме.

### Стабильность Stability

По умолчанию только стабильные релизы принимаются во внимание. Если бы Вы
хотели бы также получить зависимости RC, beta, alpha или dev версий, Вы можете это
сделать с помощью [флагов стабильности](04-schema.md#package-links) (stability flags). Чтобы применить это для
всех пакетов, вместо того чтобы делать это для каждой зависимости, Вы также можете использовать настройку
[минимальная стабильность](04-schema.md#minimum-stability) (minimum-stability).

## Установка зависимостей

Чтобы установить зависимости для Вашего проекта просто запустите
команду [`install`](03-cli.md#install)

```sh
php composer.phar install
```

Это найдёт последнюю версию `monolog/monolog`, которая соответствует Вашим
ограничениям для этой версии пакета, и скачает его в директорию поставщика `vendor`.
Здесь имеет место соглашение ставить код сторонних производителей в каталог с именем `vendor`.
В случае с Monolog, он будет располагаться в `vendor/monolog/monolog`.

> **Совет:** Если Вы используете git для вашего проекта, Вам вероятней всего
> необходимо добавить каталог `vendor` в  файл `.gitignore`.
> Вы же не хотите добавить весь этот код в Ваш репозиторий.

Вы заметите, что команда [`install`](03-cli.md#install) также создала файл `composer.lock`

## `composer.lock` - Файл блокировки

После установки зависимостей Composer пишет список точных
версий в файл `composer.lock`. Это блокирует Ваш проект
для этих конкретных версий.

**`composer.lock` фиксирует Ваши приложения (наряду с `composer.json`)
в системе контроля версий.**

Это важно потому, что команда ['install'](03-cli.md#install) проверяет
присутствует ли файл блокировки, и если это так, он загружает указанные там версии
(независимо от того, что говорит `composer.json`).

Это означает, что любой, кто настраивает проект, будет загружать точно такие же
версии зависимостей. Ваш CI сервер, рабочая машина, другие
разработчики в Вашей команде, все они и всё работает на одних и тех же зависимостях,
что снижает потенциал для ошибок, затрагивающих только некоторые части
развертывания. Даже если Вы разрабатываете самостоятельно, через шесть месяцев при переустановке
проекта, Вы можете быть уверены, что установленные зависимости по-прежнему работают даже
если зависимости выпустили много новых версий с тех пор.

Если файл `composer.lock` не существует, Composer будет считывать зависимости и
версии из `composer.json` и создаст файл блокировки после выполнения
команды [`update`](03-cli.md#update) или [`install`](03-cli.md#install).

Это означает, что если какая-либо из зависимостей выпустила новую версию, Вы не получите обновления автоматически.
Для обновления до новой версии, используйте команду [`update`](03-cli.md#update).
Это получит последнюю соответствующую версию (в соответствии с Вашим файлом `composer.json`), а также обновит файл блокировки
до новой версии.

```sh
php composer.phar update
```

> **Примечание:** Composer отобразит предупреждение при выполнении команды `install`
> если `composer.lock` и `composer.json` не синхронизированы.

Если Вы хотите установить или обновить только одну зависимость, Вы можете сделать это следующим образом:

```sh
php composer.phar update monolog/monolog [...]
```

> **Примечание:** Для библиотек не является необходимым фиксировать файл блокировки,
> см. также: [Библиотеки - Файл блокировки](02-libraries.md#lock-file).

## Packagist

[Packagist](https://packagist.org/) является главным репозиторием (хранилищем) Composer.
Репозиторий Composer это основной источник пакетов: место, откуда Вы можете получить различные пакеты.
Packagist стремится быть центральным репозиторием который используют все. Это
означает, что можно автоматически затребовать `require` для любого пакета который доступен здесь.

Если Вы перейдёте на [веб-сайт Packagist](https://packagist.org/) (packagist.org),
здесь Вы можете просматривать и искать пакеты.

Любой проект с открытым кодом используемый с Composer рекомендуется публиковать на Packagist. Библиотекам не обязательно нужно находиться на Packagist чтобы использовать Composer, но это позволяет более быстро обнаруживать их и пробовать их другими разработчиками.

## Автозагрузка

Для библиотек которые поддерживают автозагрузку Composer создаёт файл `vendor/autoload.php`.
Вы можете просто включить этот файл в проект и библиотека автоматически загрузится.

```php
require __DIR__ . '/vendor/autoload.php';
```

Это делает библиотеку очень простой в использовании третьей стороной. Например: Если Ваш проект
зависит от Monolog, Вы можете просто начать использовать классы из него, и они будут видны.

```php
$log = new Monolog\Logger('name');
$log->pushHandler(new Monolog\Handler\StreamHandler('app.log', Monolog\Logger::WARNING));
$log->addWarning('Foo');
```

Вы даже можете добавить собственный код для автозагрузчика путем добавления поля
[`autoload`](04-schema.md#autoload) (автозагрузка) в `composer.json`.

```json
{
    "autoload": {
        "psr-4": {"Acme\\": "src/"}
    }
}
```

Composer зарегистрирует [PSR-4](http://www.php-fig.org/psr/psr-4/) автозагрузку
для пространства имен `Acme`.

Вы определяете сопоставления из пространства имен в директории.
Каталог `src` будет корнем Вашего проекта, на том же уровне, как и `vendor` каталог. Например файл `src/Foo.php` будет содержащий класс `Acme\Foo`.

После добавления поля [`autoload`](04-schema.md#autoload) (автозагрузка), нужно перезапустить [`dump-autoload`](03-cli.md#dump-autoload) для повторного создания файла `vendor/autoload.php`.

Включая этот файл, он также будет возвращать экземпляр автозагрузчика, так что Вы можете хранить его возвращаемое значение в переменной и добавлять больше пространств имен.
Это может быть полезно для автоматической загрузки классов в наборе тестов, например.

```php
$loader = require __DIR__ . '/vendor/autoload.php';
$loader->add('Acme\\Test\\', __DIR__);
```

В дополнение к PSR-4 автозагрузке Composer также поддерживает PSR-0, classmap и файловую автозагрузку. Смотрите  раздел [`autoload`](04-schema.md#autoload) (автозагрузка) для получения большей информации.

> **Примечание:** Composer предоставляет свой собственный автозагрузчик. Если Вы не хотите использовать его одного, Вы можете просто включить файлы `vendor/composer/autoload_*.php` которые возвращают ассоциативные массивы, позволяя Вам настроить свой собственный автозагрузчик.

&larr; [Введение](00-intro.md)  |  [Библиотеки](02-libraries.md) &rarr;
