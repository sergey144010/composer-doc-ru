[Оригинал](https://github.com/composer/composer/blob/master/doc/04-schema.md)
[Оглавление](https://github.com/sergey144010/composer-doc-ru/blob/master/README.md#Оглавление)

# Схема composer.json

This chapter will explain all of the fields available in `composer.json`.

Эта глава объяснит все поля доступные в `composer.json`.

## JSON схема

We have a [JSON schema](http://json-schema.org) that documents the format and
can also be used to validate your `composer.json`. In fact, it is used by the
`validate` command. You can find it at: https://getcomposer.org/schema.json

У нас есть [JSON схема](http://json-schema.org) которая может быть использована
для проверки Вашего `composer.json`. По факту она используется для команды `validate`.
Можете найти это в : https://getcomposer.org/schema.json

## Root Package
## Корневой пакет

The root package is the package defined by the `composer.json` at the root of
your project. It is the main `composer.json` that defines your project
requirements.

Корневой пакет это пакет определённый через `composer.json` в корне Вашего проекта.
Это главный `composer.json` который определяет зависимости Вашего проекта.

Certain fields only apply when in the root package context. One example of
this is the `config` field. Only the root package can define configuration.
The config of dependencies is ignored. This makes the `config` field
`root-only`.

Некоторые поля применяются только в рамках корневого пакета. Один из примеров
это поле `config`. Только корневой пакет может определять конфигурацию.
Настройка зависимостей игнорируется. Это делает `config` поле `root-only`.

> **Note:** A package can be the root package or not, depending on the context.
> For example, if your project depends on the `monolog` library, your project
> is the root package. However, if you clone `monolog` from GitHub in order to
> fix a bug in it, then `monolog` is the root package.

> **Примечание:** Пакет может быть корневым пакетом или не быть, в зависимости от контекста.
> Например если Ваш проект зависит от библиотеки `monolog`, Ваш проект является корневым пакетом.
> Однако если Вы клонируете `monolog` из GitHub для того, чтобы исправить ошибку в нём, то тогда `monolog` является корневым пакетом.

## Properties
## Свойства

### name

The name of the package. It consists of vendor name and project name,
separated by `/`.

Имя пакета. Оно состоит из имени поставщика и имени проекта разделенных `/`.

Examples:
Примеры:

* monolog/monolog
* igorw/event-source

Required for published packages (libraries).

Требуется для публикации пакетов (библиотек).

### description

A short description of the package. Usually this is just one line long.

Краткое описание пакета. Обычно длиной только в одну линию.

Required for published packages (libraries).

Требуется для публикации пакетов (библиотек).

### version

The version of the package. In most cases this is not required and should
be omitted (see below).

Версия пакета. В большинстве случаев это поле не является обязательным и должно быть
исключено (см. ниже).

This must follow the format of `X.Y.Z` or `vX.Y.Z` with an optional suffix
of `-dev`, `-patch` (`-p`), `-alpha` (`-a`), `-beta` (`-b`) or `-RC`.
The patch, alpha, beta and RC suffixes can also be followed by a number.

Должно соответствовать формату `X.Y.Z` или `vX.Y.Z` с необязательным суффиксом
`-dev`, `-patch` (`-p`), `-alpha` (`-a`), `-beta` (`-b`) или `-RC`.
Суффиксы patch, alpha, beta и RC также могут следовать за номером.

Examples:
Примеры:

- 1.0.0
- 1.0.2
- 1.1.0
- 0.2.5
- 1.0.0-dev
- 1.0.0-alpha3
- 1.0.0-beta2
- 1.0.0-RC5
- v2.0.4-p1

Optional if the package repository can infer the version from somewhere, such
as the VCS tag name in the VCS repository. In that case it is also recommended
to omit it.

Необязательное свойство. Если репозиторий пакетов может вывести версию от куда-то, так
как имя тега VCS в VCS репозитории. В этом случае рекомендуется опустить данное свойство.

> **Note:** Packagist uses VCS repositories, so the statement above is very
> much true for Packagist as well. Specifying the version yourself will
> most likely end up creating problems at some point due to human error.

> **Примечание:** Packagist использует репозитории VCS, поэтому описанное выше очень верно также и для Packagist.
> Указание версии будет скорее всего в конечном итоге создавать проблемы в какой-то момент из-за человеческой ошибки.

### type

The type of the package. It defaults to `library`.

Тип пакета. По умолчанию `library`.

Package types are used for custom installation logic. If you have a package
that needs some special logic, you can define a custom type. This could be a
`symfony-bundle`, a `wordpress-plugin` or a `typo3-module`. These types will
all be specific to certain projects, and they will need to provide an
installer capable of installing packages of that type.

Тип пакетов используется для пользовательской логики установки. Если у Вас есть пакет для которого
нужна какая-то особая логика, то Вы можете определить пользовательский тип. Это может быть
`symfony-bundle`, `wordpress-plugin` или `typo3-module`.
Эти все типы для конкретных проектов и они должны предоставлять
установщику возможность устанавливать пакеты этого типа.

Out of the box, Composer supports four types:

Из коробки Composer поддерживает четыре типа:

- **library:** This is the default. It will simply copy the files to `vendor`.
- **project:** This denotes a project rather than a library. For example
  application shells like the [Symfony standard edition](https://github.com/symfony/symfony-standard),
  CMSs like the [SilverStripe installer](https://github.com/silverstripe/silverstripe-installer)
  or full fledged applications distributed as packages. This can for example
  be used by IDEs to provide listings of projects to initialize when creating
  a new workspace.
- **metapackage:** An empty package that contains requirements and will trigger
  their installation, but contains no files and will not write anything to the
  filesystem. As such, it does not require a dist or source key to be
  installable.
- **composer-plugin:** A package of type `composer-plugin` may provide an
  installer for other packages that have a custom type. Read more in the
  [dedicated article](articles/custom-installers.md).
  
- **library:** Это значение по умолчанию. Это просто скопирует файлы в `vendor`.
- **project:** Это обозначение проектов вместо библиотек. Например шелл приложений таких как
  [Symfony standard edition](https://github.com/symfony/symfony-standard), CMS таких как
  [SilverStripe installer](https://github.com/silverstripe/silverstripe-installer) или
  полноценных приложений распространяемых как пакеты. Это может например использоваться в IDE предоставляя
  список проектов для инициализации при создании новой рабочей области.
- **metapackage:** Пустой пакет содержащий требования и выззывающий их установку, но не содержащий файлов, не будет писать что-либо в файловой системе. Таким образом он не зависит от dist или устанавливаемых исходных ключей.
- **composer-plugin:** Пакет типа `composer-plugin` может предоставить установщик для других пакетов, которые имеют пользовательский тип. Подробнее читайте в [dedicated article](articles/custom-installers.md).

Only use a custom type if you need custom logic during installation. It is
recommended to omit this field and have it just default to `library`.

Используйте пользовательский тип, только если требуется пользовательская логика во время установки.
Рекомендуется пропустить это поле или просто использовать по умолчанию `library`.

### keywords

An array of keywords that the package is related to. These can be used for
searching and filtering.

Массив ключевых слов, которые касаются пакета. Они могут быть использованы для поиска и фильтрации.

Examples:
Примеры:

- logging
- events
- database
- redis
- templating

Optional.

Необязательное поле.

### homepage

An URL to the website of the project.

URL-адрес веб-сайта проекта.

Optional.

Необязательное поле.

### time

Release date of the version.

Дата версии релиза.

Must be in `YYYY-MM-DD` or `YYYY-MM-DD HH:MM:SS` format.

Должно быть в формате `YYYY-MM-DD` или `YYYY-MM-DD HH:MM:SS`

Optional.

Необязательное поле.

### license

The license of the package. This can be either a string or an array of strings.

Лицензия пакета. Это может быть строка или массив строк.

The recommended notation for the most common licenses is (alphabetical):

Рекомендуются нотации для наиболее распространенных лицензий (по алфавиту):

- Apache-2.0
- BSD-2-Clause
- BSD-3-Clause
- BSD-4-Clause
- GPL-2.0
- GPL-2.0+
- GPL-3.0
- GPL-3.0+
- LGPL-2.1
- LGPL-2.1+
- LGPL-3.0
- LGPL-3.0+
- MIT

Optional, but it is highly recommended to supply this. More identifiers are
listed at the [SPDX Open Source License Registry](https://www.spdx.org/licenses/).

Необязательно, но настоятельно рекомендуется указать это поле. Дополнительные идентификаторы
перечислены в [SPDX Open Source License Registry](https://www.spdx.org/licenses/).

For closed-source software, you may use `"proprietary"` as the license identifier.

Для программного обеспечения с закрытым исходным кодом можете использовать `"proprietary"` как идентификатор лицензии.

An Example:
Пример:

```json
{
    "license": "MIT"
}
```

For a package, when there is a choice between licenses ("disjunctive license"),
multiple can be specified as array.

Для пакета у которого есть выбор между лицензиями ("disjunctive license"),
может быть указано несколько как массив.

An Example for disjunctive licenses:
Пример disjunctive licenses:

```json
{
    "license": [
       "LGPL-2.1",
       "GPL-3.0+"
    ]
}
```

Alternatively they can be separated with "or" and enclosed in parenthesis;

В качестве альтернативы они могут быть разделены через "or" и должны быть заключены в скобки;

```json
{
    "license": "(LGPL-2.1 or GPL-3.0+)"
}
```

Similarly when multiple licenses need to be applied ("conjunctive license"),
they should be separated with "and" and enclosed in parenthesis.

Аналогичным образом, когда имеется несколько лицензий необходимо применять ("conjunctive license"),
они должны быть разделены через "and" и должны быть заключены в скобки.

### authors

The authors of the package. This is an array of objects.

Авторы пакета. Это массив объектов.

Each author object can have following properties:

Каждый объект автора может иметь следующие свойства:

* **name:** The author's name. Usually their real name.
* **email:** The author's email address.
* **homepage:** An URL to the author's website.
* **role:** The author's role in the project (e.g. developer or translator)

* **name:** Имя автора. Обычно реальное имя авторов.
* **email:** Адрес электронной почты автора.
* **homepage:** URL-адрес веб-сайта автора.
* **role:** Роль автора в проекте (например, разработчик или переводчик)

An example:

Пример:

```json
{
    "authors": [
        {
            "name": "Nils Adermann",
            "email": "naderman@naderman.de",
            "homepage": "http://www.naderman.de",
            "role": "Developer"
        },
        {
            "name": "Jordi Boggiano",
            "email": "j.boggiano@seld.be",
            "homepage": "http://seld.be",
            "role": "Developer"
        }
    ]
}
```

Optional, but highly recommended.

Необязательно, но настоятельно рекомендуется.

### support

Various information to get support about the project.

Различные сведения для получения поддержки проекта.

Support information includes the following:

Информация о поддержке включает в себя следующее:

* **email:** Email address for support.
* **issues:** URL to the issue tracker.
* **forum:** URL to the forum.
* **wiki:** URL to the wiki.
* **irc:** IRC channel for support, as irc://server/channel.
* **source:** URL to browse or download the sources.
* **docs:** URL to the documentation.
* **rss:** URL to the RSS feed.

* **email:** Адрес электронной почты поддержки.
* **issues:** URL-адрес в системе отслеживания проблем.
* **forum:** URL-адрес форума.
* **wiki:** URL-адрес wiki.
* **irc:** IRC канал поддержки, например irc://server/channel.
* **source:** URL-адрес, чтобы просмотреть или скачать исходники.
* **docs:** URL-адрес документации.
* **rss:** URL-адрес RSS-канала.

An example:
Пример:

```json
{
    "support": {
        "email": "support@example.org",
        "irc": "irc://irc.freenode.org/composer"
    }
}
```

Optional.

Необязательное поле.

### Package links - Ссылки на пакеты

All of the following take an object which maps package names to
[version constraints](01-basic-usage.md#package-versions).

Все следующее принимает объект, который сопоставляет имена пакетов с их
[ограничениями версий](01-basic-usage.md#package-versions).

Example:

Пример:

```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

All links are optional fields.

Все ссылки являются не обязательными полями.

`require` and `require-dev` additionally support stability flags ([root-only](04-schema.md#root-package)).
These allow you to further restrict or expand the stability of a package beyond
the scope of the [minimum-stability](#minimum-stability) setting. You can apply
them to a constraint, or just apply them to an empty constraint if you want to
allow unstable packages of a dependency for example.

`require` и `require-dev` кроме того, поддерживают флаги стабильности, но только для ([корневого пакета](04-schema.md#root-package)).
Это позволит Вам в дальнейшем ограничивать или расширять стабильность пакета за пределами
области [minimum-stability](#minimum-stability). Вы можете применить
их ограничения, или просто применить пустые ограничения, если хотите
разрешить зависимости например нестабильных пакетов.

Example:
Пример:

```json
{
    "require": {
        "monolog/monolog": "1.0.*@beta",
        "acme/foo": "@dev"
    }
}
```

If one of your dependencies has a dependency on an unstable package you need to
explicitly require it as well, along with its sufficient stability flag.

Если одна из зависимостей имеет зависимость от нестабильного пакета, то Вам нужно
явно указать его, наряду с флагом стабильности.

Example:

Пример:

```json
{
    "require": {
        "doctrine/doctrine-fixtures-bundle": "dev-master",
        "doctrine/data-fixtures": "@dev"
    }
}
```

`require` and `require-dev` additionally support explicit references (i.e.
commit) for dev versions to make sure they are locked to a given state, even
when you run update. These only work if you explicitly require a dev version
and append the reference with `#<ref>`.

`require` и `require-dev` дополнительно поддерживают явные ссылки (т.е.
commit) для dev версий, чтобы убедиться, что они заблокированы для данного состояния, даже
когда Вы запустите обновление. Это работает только если явно указана версия dev
и добавлена ссылка с `#<ref>`.

Example:

Пример:

```json
{
    "require": {
        "monolog/monolog": "dev-master#2eb0c0978d290a1c45346a1955188929cb4e5db7",
        "acme/foo": "1.0.x-dev#abc123"
    }
}
```

> **Note:** While this is convenient at times, it should not be how you use
> packages in the long term because it comes with a technical limitation. The
> composer.json metadata will still be read from the branch name you specify
> before the hash. Because of that in some cases it will not be a practical
> workaround, and you should always try to switch to tagged releases as soon
> as you can.

> **Примечание:** Хотя это удобно временами, не следует так использовать пакеты в долгосрочной перспективе потому, что это предоставляется с техническим ограничением. Composer.json метаданные будут по-прежнему читаться из названия ветки заданной перед хэшем. Из-за этого в некоторых случаях это не будет практическим решением и Вам всегда нужно будет переключаться между релизами.

It is also possible to inline-alias a package constraint so that it matches
a constraint that it otherwise would not. For more information [see the
aliases article](articles/aliases.md).

Это также возможно для inline псевдонимов, чтобы он соответствовал
ограничениям, которого в противном случае не будет. Для получения дополнительной информации см.
[aliases article](articles/aliases.md).

`require` and `require-dev` also support references to specific PHP versions
and PHP extensions your project needs to run successfully.

`require` и `require-dev` также поддерживают ссылки на конкретные версии PHP
и расширения PHP необходимые для успешного выполнения вашего проекта.

Example:

Пример:

```json
{
    "require" : {
        "php" : "^5.5 || ^7.0",
        "ext-mbstring": "*"
    }
}
```

#### require

Lists packages required by this package. The package will not be installed
unless those requirements can be met.

Список пакетов необходимых для этого пакета. Пакет не будет установлен
если эти требования не могут быть удовлетворены.

#### require-dev <span>(только для [корневого пакета](04-schema.md#root-package))</span>

Lists packages required for developing this package, or running
tests, etc. The dev requirements of the root package are installed by default.
Both `install` or `update` support the `--no-dev` option that prevents dev
dependencies from being installed.

Список пакетов необходимых для разработки этого пакета или тестирования и др.
По умолчанию устанавливаются dev зависимости корневого пакета.
Обе команды `install` или `update` поддерживают вариант `--no-dev` который предотвращает установку dev
зависимостей.

#### conflict

Lists packages that conflict with this version of this package. They
will not be allowed to be installed together with your package.

Список пакетов которые конфликтуют с этой версией этого пакета. Они
не могут быть установлены вместе с Вашим пакетом.

Note that when specifying ranges like `<1.0 >=1.1` in a `conflict` link,
this will state a conflict with all versions that are less than 1.0 *and* equal
or newer than 1.1 at the same time, which is probably not what you want. You
probably want to go for `<1.0 || >=1.1` in this case.

Обратите внимание, что при указании диапазонов таких как `<1.0 >=1.1` в `conflict` ссылке,
Это будет утверждать конфликт со всеми версиями, которые являются менее 1.0 *и* равных
или новее чем 1.1 в то время, что это является, вероятннее всего, не тем, что вы хотите. Вы
вероятно хотите указать `<1.0 || >=1.1` в данном случае.

#### replace

Lists packages that are replaced by this package. This allows you to fork a
package, publish it under a different name with its own version numbers, while
packages requiring the original package continue to work with your fork because
it replaces the original package.

Список пакетов которые заменяются на этот пакет. Это позволяет Вам сделать форк
пакета, опубликовать его под другим именем со своими собственными номерами версий, в то время как
пакеты, требующие исходный пакет по-прежнему будут работать с вашей вилкой, потому что
она заменяет исходный пакет.

This is also useful for packages that contain sub-packages, for example the main
symfony/symfony package contains all the Symfony Components which are also
available as individual packages. If you require the main package it will
automatically fulfill any requirement of one of the individual components,
since it replaces them.

Это также полезно для пакетов, которые содержат вложенные пакеты, например основной
Symfony/symfony пакет содержит все компоненты Symfony, которые также
доступны как отдельные пакеты. Если Вам требуется основной пакет это
автоматически выполнит любые зависимости одного из отдельных компонентов,
поскольку он заменяет их.

Caution is advised when using replace for the sub-package purpose explained
above. You should then typically only replace using `self.version` as a version
constraint, to make sure the main package only replaces the sub-packages of
that exact version, and not any other version, which would be incorrect.

Осторожно при использовании замены для вложенных пакетов.
Вы должны затем обычно заменять только с помощью `self.version` как версия
ограничения, чтобы убедиться, что основной пакет заменяет только точные версии вложенных пакетов, а не любые другие версии, которые могут быть неверными.

#### provide

List of other packages that are provided by this package. This is mostly
useful for common interfaces. A package could depend on some virtual
`logger` package, any library that implements this logger interface would
simply list it in `provide`.

Список других пакетов которые предоставляет этот пакет. Это в основном
полезно для общих интерфейсов. Пакет может зависеть от некоторого виртуального
пакета `logger`, любой библиотеки которая реализует этот интерфейс. Регистратор включит
просто его в список `provide`.

#### suggest

Suggested packages that can enhance or work well with this package. These are
just informational and are displayed after the package is installed, to give
your users a hint that they could add more packages, even though they are not
strictly required.

Предлагаемые пакеты которые могут быть добавлены или работают хорошо вместе с этим пакетом. Это
просто информация она отображается после установки пакета, чтобы дать
пользователю намёк на то, что он мог-бы добавить больше пакетов, даже несмотря на то, что они не являются
обязательными.

The format is like package links above, except that the values are free text
and not version constraints.

Используется формат как по ссылки выше для пакетов, за исключением того, что вместо значения версии ограничения используется свободный текст.

Example:
Пример:

```json
{
    "suggest": {
        "monolog/monolog": "Allows more advanced logging of the application flow"
    }
}
```

### autoload

Autoload mapping for a PHP autoloader.

Карта автозагрузки для автозагрузкика PHP.

Currently [`PSR-0`](http://www.php-fig.org/psr/psr-0/) autoloading,
[`PSR-4`](http://www.php-fig.org/psr/psr-4/) autoloading, `classmap` generation and
`files` includes are supported. PSR-4 is the recommended way though since it offers
greater ease of use (no need to regenerate the autoloader when you add classes).

В настоящее время поддерживаются [`PSR-0`](http://www.php-fig.org/psr/psr-0/) автозагрузка,
[`PSR-4`](http://www.php-fig.org/psr/psr-4/) автозагрузка,
генерирование `classmap` и `files`.
PSR-4 является рекомендуемым, так как он более прост в использовании (не нужно регенерировать автозагрузчик при добавлении новых классов).

#### PSR-4

Under the `psr-4` key you define a mapping from namespaces to paths, relative to the
package root. When autoloading a class like `Foo\\Bar\\Baz` a namespace prefix
`Foo\\` pointing to a directory `src/` means that the autoloader will look for a
file named `src/Bar/Baz.php` and include it if present. Note that as opposed to
the older PSR-0 style, the prefix (`Foo\\`) is **not** present in the file path.

Под `psr-4` подразумевается сопоставление пути из пространства имён относительно корня пакета.
Когда происходит автозагрузка класса с пространством имён таким как `Foo\\Bar\\Baz` префикс `Foo\\`
указывает на каталог `src/` и подразумевает, что автозагрузчик будет искать файл с именем `src/Bar/Baz.php`
и включит его если он присутствует.
Обратите внимание, что в отличие от старого стиля PSR-0, префикс (`Foo\\`) **не** присутствует в пути к файлу.

Namespace prefixes must end in `\\` to avoid conflicts between similar prefixes.
For example `Foo` would match classes in the `FooBar` namespace so the trailing
backslashes solve the problem: `Foo\\` and `FooBar\\` are distinct.

Префиксы пространств имен должны заканчиваться `\\` чтобы избежать конфликтов между аналогичными префиксами.
Например `Foo` присутствует в пространстве имен `FooBar`.
Обратные косые черты решают эту проблему: `Foo\\` и `FooBar\\` различны.

The PSR-4 references are all combined, during install/update, into a single
key => value array which may be found in the generated file
`vendor/composer/autoload_psr4.php`.

Все PSR-4 ссылки объединяются во время установки/обновления в единый массив
ключи => значения, которые могут быть найдены в созданном файле
`vendor/composer/autoload_psr4.php`.

Example:
Пример:

```json
{
    "autoload": {
        "psr-4": {
            "Monolog\\": "src/",
            "Vendor\\Namespace\\": ""
        }
    }
}
```

If you need to search for a same prefix in multiple directories,
you can specify them as an array as such:

Если Вам нужен поиск одинаковых префиксов в нескольких каталогах,
их можно указать в качестве массива таким образом:

```json
{
    "autoload": {
        "psr-4": { "Monolog\\": ["src/", "lib/"] }
    }
}
```

If you want to have a fallback directory where any namespace will be looked for,
you can use an empty prefix like:

Если Вы хотите иметь резервный каталог, где будет происходить поиск любых пространств имен,
Вы можете использовать пустой префикс так как показано ниже:

```json
{
    "autoload": {
        "psr-4": { "": "src/" }
    }
}
```

#### PSR-0

Under the `psr-0` key you define a mapping from namespaces to paths, relative to the
package root. Note that this also supports the PEAR-style non-namespaced convention.

Под `psr-0` подразумевается сопоставление пути из пространства имён относительно корня пакета.
Обратите внимание, что это также поддерживает конвенцию не пространств имён PEAR-style.

Please note namespace declarations should end in `\\` to make sure the autoloader
responds exactly. For example `Foo` would match in `FooBar` so the trailing
backslashes solve the problem: `Foo\\` and `FooBar\\` are distinct.

Пожалуйста обратите внимание объявления пространств имен следует завершать `\\`.
Например `Foo` будет входить в `FooBar`, таким образом
обратные косые черты решают проблему: `Foo\\` и `FooBar\\` различны.

The PSR-0 references are all combined, during install/update, into a single key => value
array which may be found in the generated file `vendor/composer/autoload_namespaces.php`.

Все PSR-0 ссылки объединяются во время установки/обновления в единый массив
ключи => значения, которые могут быть найдены в созданном файле
`vendor/composer/autoload_namespaces.php`.

Example:
Пример:

```json
{
    "autoload": {
        "psr-0": {
            "Monolog\\": "src/",
            "Vendor\\Namespace\\": "src/",
            "Vendor_Namespace_": "src/"
        }
    }
}
```

If you need to search for a same prefix in multiple directories,
you can specify them as an array as such:

Если Вам нужен поиск одинаковых префиксов в нескольких каталогах,
их можно указать в качестве массива таким образом:

```json
{
    "autoload": {
        "psr-0": { "Monolog\\": ["src/", "lib/"] }
    }
}
```

The PSR-0 style is not limited to namespace declarations only but may be
specified right down to the class level. This can be useful for libraries with
only one class in the global namespace. If the php source file is also located
in the root of the package, for example, it may be declared like this:

PSR-0 стиль не ограничивается только объявлением пространств имен, здесь также можно указать
вплоть до уровня класса. Это может быть полезно для библиотек только с одним классом в глобальном пространстве имен.
Если также исходный файл php, например, расположен в корневом каталоге пакета, он может быть объявлен следующим образом:

```json
{
    "autoload": {
        "psr-0": { "UniqueGlobalClass": "" }
    }
}
```

If you want to have a fallback directory where any namespace can be, you can
use an empty prefix like:

Если Вы хотите иметь резервный каталог, где могут быть любые пространства имен, Вы можете использовать пустой префикс как:

```json
{
    "autoload": {
        "psr-0": { "": "src/" }
    }
}
```

#### Classmap

The `classmap` references are all combined, during install/update, into a single
key => value array which may be found in the generated file
`vendor/composer/autoload_classmap.php`. This map is built by scanning for
classes in all `.php` and `.inc` files in the given directories/files.

Все `classmap` ссылки объединяются во время установки/обновления в единый массив
ключи => значения, которые могут быть найдены в созданном файле
`vendor/composer/autoload_classmap.php`.
Эта карта построена путем сканирования классов всех файлов `.php` и `.inc` в каталоги/файлы.

You can use the classmap generation support to define autoloading for all libraries
that do not follow PSR-0/4. To configure this you specify all directories or files
to search for classes.

Вы можете использовать поддержку генерирования classmap для создания автозагрузчика для всех библиотек
что не делается в PSR-0/4. Чтобы настроить это нужно указать все каталоги или файлы
для поиска классов.

Example:
Пример:

```json
{
    "autoload": {
        "classmap": ["src/", "lib/", "Something.php"]
    }
}
```

#### Files

If you want to require certain files explicitly on every request then you can use
the 'files' autoloading mechanism. This is useful if your package includes PHP functions
that cannot be autoloaded by PHP.

Если Вы хотите требовать определенные файлы прямо на каждый запрос, то Вы можете использовать механизм
автозагрузки файлов 'files'. Это полезно, если Ваш пакет включает в себя PHP функции которые не могут быть загружены через автозагрузку PHP.

Example:
Пример:

```json
{
    "autoload": {
        "files": ["src/MyLibrary/functions.php"]
    }
}
```

#### Exclude files from classmaps - Исключение файлов из classmaps

If you want to exclude some files or folders from the classmap you can use the 'exclude-from-classmap' property.
This might be useful to exclude test classes in your live environment, for example, as those will be skipped
from the classmap even when building an optimized autoloader.

Если Вы хотите исключить некоторые файлы или директории из classmap Вы можете использовать свойство 'exclude-from-classmap'.
Это может быть полезно для исключения тестовых классов из реального окружения, например, как те которые будут пропущены
из classmap даже при построении оптимизированного автозагрузчика.

The classmap generator will ignore all files in the paths configured here. The paths are absolute from the package
root directory (i.e. composer.json location), and support `*` to match anything but a slash, and `**` to
match anything. `**` is implicitly added to the end of the paths.

Classmap генератор будет игнорировать все файлы по пути, настроенные здесь. Пути являются абсолютными из
корневого каталога пакета (то есть где находится composer.json) и поддерживают `*` для соответствия ничего кроме слеша, и `**` ничего.
`**` неявно добавляется в конец пути.

Example:
Пример:

```json
{
    "autoload": {
        "exclude-from-classmap": ["/Tests/", "/test/", "/tests/"]
    }
}
```

### autoload-dev <span>([root-only](04-schema.md#root-package))</span>

This section allows to define autoload rules for development purposes.

Classes needed to run the test suite should not be included in the main autoload
rules to avoid polluting the autoloader in production and when other people use
your package as a dependency.

Therefore, it is a good idea to rely on a dedicated path for your unit tests
and to add it within the autoload-dev section.

Example:

```json
{
    "autoload": {
        "psr-4": { "MyLibrary\\": "src/" }
    },
    "autoload-dev": {
        "psr-4": { "MyLibrary\\Tests\\": "tests/" }
    }
}
```

### include-path

> **DEPRECATED**: This is only present to support legacy projects, and all new code
> should preferably use autoloading. As such it is a deprecated practice, but the
> feature itself will not likely disappear from Composer.

A list of paths which should get appended to PHP's `include_path`.

Example:

```json
{
    "include-path": ["lib/"]
}
```

Optional.

### target-dir

> **DEPRECATED**: This is only present to support legacy PSR-0 style autoloading,
> and all new code should preferably use PSR-4 without target-dir and projects
> using PSR-0 with PHP namespaces are encouraged to migrate to PSR-4 instead.

Defines the installation target.

In case the package root is below the namespace declaration you cannot
autoload properly. `target-dir` solves this problem.

An example is Symfony. There are individual packages for the components. The
Yaml component is under `Symfony\Component\Yaml`. The package root is that
`Yaml` directory. To make autoloading possible, we need to make sure that it
is not installed into `vendor/symfony/yaml`, but instead into
`vendor/symfony/yaml/Symfony/Component/Yaml`, so that the autoloader can load
it from `vendor/symfony/yaml`.

To do that, `autoload` and `target-dir` are defined as follows:

```json
{
    "autoload": {
        "psr-0": { "Symfony\\Component\\Yaml\\": "" }
    },
    "target-dir": "Symfony/Component/Yaml"
}
```

Optional.

### minimum-stability <span>([root-only](04-schema.md#root-package))</span>

This defines the default behavior for filtering packages by stability. This
defaults to `stable`, so if you rely on a `dev` package, you should specify
it in your file to avoid surprises.

All versions of each package are checked for stability, and those that are less
stable than the `minimum-stability` setting will be ignored when resolving
your project dependencies. Specific changes to the stability requirements of
a given package can be done in `require` or `require-dev` (see
[package links](#package-links)).

Available options (in order of stability) are `dev`, `alpha`, `beta`, `RC`,
and `stable`.

### prefer-stable <span>([root-only](04-schema.md#root-package))</span>

When this is enabled, Composer will prefer more stable packages over unstable
ones when finding compatible stable packages is possible. If you require a
dev version or only alphas are available for a package, those will still be
selected granted that the minimum-stability allows for it.

Use `"prefer-stable": true` to enable.

### repositories <span>([root-only](04-schema.md#root-package))</span>

Custom package repositories to use.

By default Composer just uses the packagist repository. By specifying
repositories you can get packages from elsewhere.

Repositories are not resolved recursively. You can only add them to your main
`composer.json`. Repository declarations of dependencies' `composer.json`s are
ignored.

The following repository types are supported:

* **composer:** A Composer repository is simply a `packages.json` file served
  via the network (HTTP, FTP, SSH), that contains a list of `composer.json`
  objects with additional `dist` and/or `source` information. The `packages.json`
  file is loaded using a PHP stream. You can set extra options on that stream
  using the `options` parameter.
* **vcs:** The version control system repository can fetch packages from git,
  svn, fossil and hg repositories.
* **pear:** With this you can import any pear repository into your Composer
  project.
* **package:** If you depend on a project that does not have any support for
  composer whatsoever you can define the package inline using a `package`
  repository. You basically just inline the `composer.json` object.

For more information on any of these, see [Repositories](05-repositories.md).

Example:

```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "http://packages.example.com"
        },
        {
            "type": "composer",
            "url": "https://packages.example.com",
            "options": {
                "ssl": {
                    "verify_peer": "true"
                }
            }
        },
        {
            "type": "vcs",
            "url": "https://github.com/Seldaek/monolog"
        },
        {
            "type": "pear",
            "url": "https://pear2.php.net"
        },
        {
            "type": "package",
            "package": {
                "name": "smarty/smarty",
                "version": "3.1.7",
                "dist": {
                    "url": "http://www.smarty.net/files/Smarty-3.1.7.zip",
                    "type": "zip"
                },
                "source": {
                    "url": "https://smarty-php.googlecode.com/svn/",
                    "type": "svn",
                    "reference": "tags/Smarty_3_1_7/distribution/"
                }
            }
        }
    ]
}
```

> **Note:** Order is significant here. When looking for a package, Composer
will look from the first to the last repository, and pick the first match.
By default Packagist is added last which means that custom repositories can
override packages from it.

### config <span>([root-only](04-schema.md#root-package))</span>

A set of configuration options. It is only used for projects. See
[Config](06-config.md) for a description of each individual option.

### scripts <span>([root-only](04-schema.md#root-package))</span>

Composer allows you to hook into various parts of the installation process
through the use of scripts.

See [Scripts](articles/scripts.md) for events details and examples.

### extra

Arbitrary extra data for consumption by `scripts`.

This can be virtually anything. To access it from within a script event
handler, you can do:

```php
$extra = $event->getComposer()->getPackage()->getExtra();
```

Optional.

### bin

A set of files that should be treated as binaries and symlinked into the `bin-dir`
(from config).

See [Vendor Binaries](articles/vendor-binaries.md) for more details.

Optional.

### archive

A set of options for creating package archives.

The following options are supported:

* **exclude:** Allows configuring a list of patterns for excluded paths. The
  pattern syntax matches .gitignore files. A leading exclamation mark (!) will
  result in any matching files to be included even if a previous pattern
  excluded them. A leading slash will only match at the beginning of the project
  relative path. An asterisk will not expand to a directory separator.

Example:

```json
{
    "archive": {
        "exclude": ["/foo/bar", "baz", "/*.test", "!/foo/bar/baz"]
    }
}
```

The example will include `/dir/foo/bar/file`, `/foo/bar/baz`, `/file.php`,
`/foo/my.test` but it will exclude `/foo/bar/any`, `/foo/baz`, and `/my.test`.

Optional.

### non-feature-branches

A list of regex patterns of branch names that are non-numeric (e.g. "latest" or something),
that will NOT be handled as feature branches. This is an array of strings.

If you have non-numeric branch names, for example like "latest", "current", "latest-stable"
or something, that do not look like a version number, then Composer handles such branches
as feature branches. This means it searches for parent branches, that look like a version
or ends at special branches (like master) and the root package version number becomes the
version of the parent branch or at least master or something.

To handle non-numeric named branches as versions instead of searching for a parent branch
with a valid version or special branch name like master, you can set patterns for branch
names, that should be handled as dev version branches.

This is really helpful when you have dependencies using "self.version", so that not dev-master,
but the same branch is installed (in the example: latest-testing).

An example:

If you have a testing branch, that is heavily maintained during a testing phase and is
deployed to your staging environment, normally "composer show -s" will give you `versions : * dev-master`.

If you configure `latest-.*` as a pattern for non-feature-branches like this:

```json
{
    "non-feature-branches": ["latest-.*"]
}
```

Then "composer show -s" will give you `versions : * dev-latest-testing`.

Optional.

&larr; [Command-line interface](03-cli.md)  |  [Repositories](05-repositories.md) &rarr;
