# Repositories

# Репозитории

This chapter will explain the concept of packages and repositories, what kinds
of repositories are available, and how they work.

Эта глава объяснит концепцию пакетов и репозиториев, какие
из репозиториев доступны, и как они работают.

## Concepts

## Концепции

Before we look at the different types of repositories that exist, we need to
understand some of the basic concepts that Composer is built on.

Прежде чем мы посмотрим на различные типы репозиториев которые существуют, мы должны
понимать некоторые основные концепции на которых построен Composer.

### Package

### Пакет

Composer is a dependency manager. It installs packages locally. A package is
essentially just a directory containing something. In this case it is PHP
code, but in theory it could be anything. And it contains a package
description which has a name and a version. The name and the version are used
to identify the package.

Composer — менеджер зависимостей. Он устанавливает пакеты локально.
Пакетом по существу является только каталог, содержащий что-то.
В данном случае это PHP код, но теоретически это может быть что угодно.
И он содержит описание пакета, который имеет имя и версию.
Имя и версия используются для идентификации пакета.

In fact, internally Composer sees every version as a separate package. While
this distinction does not matter when you are using Composer, it's quite
important when you want to change it.

В действительности, изнутрии Composer видит каждую версию как отдельный пакет.
Хотя это различие не имеет значения когда Вы используете Composer, но это довольно
важно когда Вы хотите его изменить.

In addition to the name and the version, there is useful metadata. The information
most relevant for installation is the source definition, which describes where
to get the package contents. The package data points to the contents of the
package. And there are two options here: dist and source.

В дополнении к имени и версии есть полезные метаданные.
Информацией наиболее актуальной при установке является нахождение исходников, которая описывает где можно взять содержимое пакета.
Точки данных пакета в содержимом пакета. И здесь есть два типа пакета: dist и source.

**Dist:** The dist is a packaged version of the package data. Usually a
released version, usually a stable release.

**Dist:** Упакованная версия пакета данных. Обычно
выпущенная версия, обычно стабильный релиз.

**Source:** The source is used for development. This will usually originate
from a source code repository, such as git. You can fetch this when you want
to modify the downloaded package.

**Source:** Источник, используется для разработки. Это обычно берётся
из репозитория исходного кода, таких как git. Вы можете получить его когда Вы хотите
что-либо изменить в загруженном пакете.

Packages can supply either of these, or even both. Depending on certain
factors, such as user-supplied options and stability of the package, one will
be preferred.

В пакетах можно указать любой из них, или даже оба. В зависимости от определенных
факторов, таких как пользовательские параметры и стабильности пакета, но указать один будет
более предпочтительно.

### Repository

### Репозиторий

A repository is a package source. It's a list of packages/versions. Composer
will look in all your repositories to find the packages your project requires.

Репозиторий это источник пакета. Это список пакетов/версий.
Composer будет смотреть во все ваши хранилища (репозитории), для того чтобы найти пакеты которые требует ваш проект.

By default only the Packagist repository is registered in Composer. You can
add more repositories to your project by declaring them in `composer.json`.

По умолчанию только репозиторий Packagist зарегистрирован в Composer. Вы можете
добавить дополнительные репозитории для вашего проекта объявив их в `composer.json`.

Repositories are only available to the root package and the repositories
defined in your dependencies will not be loaded. Read the
[FAQ entry](faqs/why-can't-composer-load-repositories-recursively.md) if you
want to learn why.

Репозитории доступны только для корневого пакета, а репозитории
определенные в зависимостях загружены не будут.
Читайте [FAQ entry](faqs/why-can't-composer-load-repositories-recursively.md) если
хотите узнать, почему.

## Types

## Типы

### Composer

### Composer

The main repository type is the `composer` repository. It uses a single
`packages.json` file that contains all of the package metadata.

Основным типом репозитория является `composer` репозиторий. Он использует единичный
`packages.json` файл который содержит все метаданные пакета.

This is also the repository type that packagist uses. To reference a
`composer` repository, just supply the path before the `packages.json` file.
In case of packagist, that file is located at `/packages.json`, so the URL of
the repository would be `packagist.org`. For `example.org/packages.json` the
repository URL would be `example.org`.

Это также тип репозитория используемый Packagist.
Ссылка `composer` репозитория просто указывает на путь до файла `packages.json`.
В случае packagist этот файл расположен в `/packages.json` поэтому URL-адрес
репозитория будет `packagist.org`. Для репозитория `example.org/packages.json` URL будет `example.org`.

#### packages

The only required field is `packages`. The JSON structure is as follows:

Единственное обязательное поле — `packages`. Структура JSON выглядит следующим образом:

```json
{
    "packages": {
        "vendor/package-name": {
            "dev-master": { @composer.json },
            "1.0.x-dev": { @composer.json },
            "0.0.1": { @composer.json },
            "1.0.0": { @composer.json }
        }
    }
}
```

The `@composer.json` marker would be the contents of the `composer.json` from
that package version including as a minimum:

Маркер `@composer.json` будет содержать `composer.json` из этой версии пакета включая как минимум:

* name
* version
* dist или source

Here is a minimal package definition:

Вот определение минимального пакета:

```json
{
    "name": "smarty/smarty",
    "version": "3.1.7",
    "dist": {
        "url": "http://www.smarty.net/files/Smarty-3.1.7.zip",
        "type": "zip"
    }
}
```

It may include any of the other fields specified in the [schema](04-schema.md).

Он может включать в себя любое из других полей указанных в [схеме](04-schema.md).

#### notify-batch

The `notify-batch` field allows you to specify a URL that will be called
every time a user installs a package. The URL can be either an absolute path
(that will use the same domain as the repository) or a fully qualified URL.

Поле `notify-batch` позволяет указать URL-адрес который будет вызываться
каждый раз когда пользователь устанавливает пакет. URL-адрес может быть либо абсолютный путь
(который будет использовать тот же домен который и репозиторий) или полный URL-адрес.

An example value:

Пример значения:

```json
{
    "notify-batch": "/downloads/"
}
```

For `example.org/packages.json` containing a `monolog/monolog` package, this
would send a `POST` request to `example.org/downloads/` with following
JSON request body:

Для `example.org/packages.json` содержащий пакет `monolog/monolog`, это пошлёт `POST` запрос на `example.org/downloads/` со следующим
JSON телом запроса:

```json
{
    "downloads": [
        {"name": "monolog/monolog", "version": "1.2.1.0"}
    ]
}
```

The version field will contain the normalized representation of the version
number.

В поле версия будет содержаться нормализованное представление номера версии.

This field is optional.

Это поле является необязательным.

#### includes

For larger repositories it is possible to split the `packages.json` into
multiple files. The `includes` field allows you to reference these additional
files.

Для крупных репозиториев можно разделить `packages.json` на
несколько файлов. Поле `includes` позволяет ссылаться на эти дополнительные
файлы.

An example:

Пример:

```json
{
    "includes": {
        "packages-2011.json": {
            "sha1": "525a85fb37edd1ad71040d429928c2c0edec9d17"
        },
        "packages-2012-01.json": {
            "sha1": "897cde726f8a3918faf27c803b336da223d400dd"
        },
        "packages-2012-02.json": {
            "sha1": "26f911ad717da26bbcac3f8f435280d13917efa5"
        }
    }
}
```

The SHA-1 sum of the file allows it to be cached and only re-requested if the
hash changed.

Сумма SHA-1 файла обеспечивает его кэширование и запрашивается заново только если хэш изменился.

This field is optional. You probably don't need it for your own custom
repository.

Это поле является необязательным. Вероятнее всего оно не требуется для собственных пользовательских
репозиториев.

#### provider-includes и providers-url

For very large repositories like packagist.org using the so-called provider
files is the preferred method. The `provider-includes` field allows you to
list a set of files that list package names provided by this repository. The
hash should be a sha256 of the files in this case.

Для очень больших репозиториев как packagist.org использование так называемого поставщика
файлов является предпочтительным методом. Поле `provider-includes` позволяет Вам
перечислить набор файлов в которых перечислены имена пакетов представляемые этот репозиторий.

The `providers-url` describes how provider files are found on the server. It
is an absolute path from the repository root.

`providers-url` описывает как найти поставщика файлов на сервере. 
Это абсолютный путь от корня хранилища.

An example:

```json
{
    "provider-includes": {
        "providers-a.json": {
            "sha256": "f5b4bc0b354108ef08614e569c1ed01a2782e67641744864a74e788982886f4c"
        },
        "providers-b.json": {
            "sha256": "b38372163fac0573053536f5b8ef11b86f804ea8b016d239e706191203f6efac"
        }
    },
    "providers-url": "/p/%package%$%hash%.json"
}
```

Those files contain lists of package names and hashes to verify the file
integrity, for example:

Эти файлы содержат списки имен пакетов и хэши для проверки целостности файла например:

```json
{
    "providers": {
        "acme/foo": {
            "sha256": "38968de1305c2e17f4de33aea164515bc787c42c7e2d6e25948539a14268bb82"
        },
        "acme/bar": {
            "sha256": "4dd24c930bd6e1103251306d6336ac813b563a220d9ca14f4743c032fb047233"
        }
    }
}
```

The file above declares that acme/foo and acme/bar can be found in this
repository, by loading the file referenced by `providers-url`, replacing
`%package%` by the package name and `%hash%` by the sha256 field. Those files
themselves just contain package definitions as described [above](#packages).

Приведенный выше файл заявляет, что acme/foo и acme/bar можно найти в этом
репозитории, путем загрузки файла, на который ссылается `providers-url`, заменив
`%package%` именем пакета и `%hash%` полем sha256. Эти файлы
сами просто содержат определения пакетов как описано [above](#packages).

This field is optional. You probably don't need it for your own custom
repository.

Это поле является необязательным. Вероятнее всего оно не требуется для собственных пользовательских
репозиториев.

#### stream параметры

The `packages.json` file is loaded using a PHP stream. You can set extra options
on that stream using the `options` parameter. You can set any valid PHP stream
context option. See [Context options and parameters](https://php.net/manual/en/context.php)
for more information.

`packages.json` файл загружается при помощи PHP потока. Можно задать дополнительные опции
для этого потока с помощью параметра `options`. Вы можете задать любой допустимый параметр контекста для PHP потока.
Смотрите [Контекстные опции и параметры](https://php.net/manual/en/context.php) для получения дополнительной информации.

### VCS

VCS stands for version control system. This includes versioning systems like
git, svn, fossil or hg. Composer has a repository type for installing packages from
these systems.

VCS означает - системы управления версиями. Это включает в себя системы управления версиями, такие как
git, svn, fossil или hg. Composer имеет тип репозитория для установки пакетов из этих систем.

#### Loading a package from a VCS repository

#### Загрузка пакета из VCS репозитория

There are a few use cases for this. The most common one is maintaining your
own fork of a third party library. If you are using a certain library for your
project and you decide to change something in the library, you will want your
project to use the patched version. If the library is on GitHub (this is the
case most of the time), you can simply fork it there and push your changes to
your fork. After that you update the project's `composer.json`. All you have
to do is add your fork as a repository and update the version constraint to
point to your custom branch. Your custom branch name must be prefixed with `"dev-"`. For version constraint naming conventions see
[Libraries](02-libraries.md) for more information.

Есть несколько вариантов. Наиболее распространенным является сделать Вашу
собственную ветвь сторонней библиотеки. Например если Вы используете определенные библиотеки для Вашего
проекта и Вы решили изменить что-то в какой-либо библиотеке и хотите использовать исправленную версию
в проекте. Если библиотека находится на GitHub (это случается наиболее часто)
Вы можете просто ответвить её там и отправить Ваши изменения в Вашу ветвь.
После этого Вы обновляете `composer.json` проекта. Все что вам нужно
чтобы сделать это - добавить Вашу ветвь как репозиторий указав Вашу пользовательскую ветвь и обновить ограничение версии.
Имени Вашей пользовательской ветви должно предшествовать `"dev-"`. Для наименования версии ограничения см.
[Библиотеки](02-libraries.md) для получения дополнительной информации.

Example assuming you patched monolog to fix a bug in the `bugfix` branch:

Например, предположим, что Вы нашли и исправили ошибку в monolog в своей ветке `bugfix`:

```json
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/igorw/monolog"
        }
    ],
    "require": {
        "monolog/monolog": "dev-bugfix"
    }
}
```

When you run `php composer.phar update`, you should get your modified version
of `monolog/monolog` instead of the one from packagist.

Когда Вы запустите `php composer.phar update`, Вы должны получить Вашу измененную версию
из `monolog/monolog` вместо основной из packagist.

Note that you should not rename the package unless you really intend to fork
it in the long term, and completely move away from the original package.
Composer will correctly pick your package over the original one since the
custom repository has priority over packagist. If you want to rename the
package, you should do so in the default (often master) branch and not in a
feature branch, since the package name is taken from the default branch.

Обратите внимание, что Вам не следует переименовывать пакет и полностью отказываться от оригинального пакета, если Вы действительно намерены делать форк в долгосрочной перспективе.
Composer будет правильно выбирать Ваш пакет поверх оригинального из
пользовательского хранилища, оно имеет приоритет над packagist. Если Вы хотите переименовать
пакет, Вы должны делать это в ветке по умолчанию (часто master), а не в
других ветках, поскольку из ветки по умолчанию берётся имя пакета.

Also note that the override will not work if you change the `name` property
in your forked repository's `composer.json` file as this needs to match the
original for the override to work.

Также, обратите внимание, что переопределение не будет работать, если Вы измените свойство `name` в Вашей ветке репозиториев
в файле `composer.json` , как это должно соответствовать первоначальному определению для работы.

If other dependencies rely on the package you forked, it is possible to
inline-alias it so that it matches a constraint that it otherwise would not.
For more information [see the aliases article](articles/aliases.md).

Если другие зависимости полагаются на пакет который Вы разветвили, возможно сделать
inline-alias так что-бы он соответствовал ограничениям.
Для получения дополнительной информации см. [псевдонимы article](articles/aliases.md).

#### Using private repositories
#### Использование частных репозиториев

Exactly the same solution allows you to work with your private repositories at
GitHub and BitBucket:

Так-же аналогичное решение позволяет работать с Вашими частными репозиториями на
GitHub и BitBucket:

```json
{
    "require": {
        "vendor/my-private-repo": "dev-master"
    },
    "repositories": [
        {
            "type": "vcs",
            "url":  "git@bitbucket.org:vendor/my-private-repo.git"
        }
    ]
}
```

The only requirement is the installation of SSH keys for a git client.

Единственным требованием является установка SSH-ключей для git клиента.

#### Git alternatives

#### Альтернативы Git

Git is not the only version control system supported by the VCS repository.
The following are supported:

Git является не единственной версией систем управления поддерживаемой через VCS репозиторий.
Поддерживаются следующие:

* **Git:** [git-scm.com](https://git-scm.com)
* **Subversion:** [subversion.apache.org](https://subversion.apache.org)
* **Mercurial:** [mercurial.selenic.com](http://mercurial.selenic.com)
* **Fossil**: [fossil-scm.org](https://www.fossil-scm.org/)

To get packages from these systems you need to have their respective clients
installed. That can be inconvenient. And for this reason there is special
support for GitHub and BitBucket that use the APIs provided by these sites, to
fetch the packages without having to install the version control system. The
VCS repository provides `dist`s for them that fetch the packages as zips.

Чтобы получить пакеты из этих систем необходимо иметь установленными их соответствующие клиенты.
Это может быть неудобно. И по этой причине существует специальная поддержка GitHub и BitBucket, которые используют API,
предоставляемые этими сайтами, чтобы получать пакеты без установки системы управления версиями.
VCS репозиторий обеспечивает `dist` для них который получает пакеты как молнии.

* **GitHub:** [github.com](https://github.com) (Git)
* **BitBucket:** [bitbucket.org](https://bitbucket.org) (Git and Mercurial)

The VCS driver to be used is detected automatically based on the URL. However,
should you need to specify one for whatever reason, you can use `fossil`, `git`, 
`svn` or `hg` as the repository type instead of `vcs`.

VCS драйвер необходимый для использования определяется автоматически на основе URL-адреса. Однако,
если Вам необходимо указать только один по любой причине, Вы можете использовать `fossil`, `git`, 
`svn` или `hg` как тип репозитория вместо `vcs`.

If you set the `no-api` key to `true` on a github repository it will clone the
repository as it would with any other git repository instead of using the
GitHub API. But unlike using the `git` driver directly, Composer will still
attempt to use github's zip files.

Если задать ключ `no-api` в значение `true` на github репозитории, это будет клонировать
репозиторий как бы с другими репозиториями вместо использования
GitHub API. Но в отличие от непосредственного использования с помощью драйвера `git`, Composer будет по-прежнему
пытаться использовать github zip-файлы.

#### Subversion Options

#### Subversion опции

Since Subversion has no native concept of branches and tags, Composer assumes
by default that code is located in `$url/trunk`, `$url/branches` and
`$url/tags`. If your repository has a different layout you can change those
values. For example if you used capitalized names you could configure the
repository like this:

Поскольку Subversion не имеет собственной концепции веток и меток, Composer предполагает
по умолчанию, что код находится в `$url/trunk`, `$url/branches` и
`$url/tags`. Если Ваше хранилище имеет другой формат можно изменить эти
значения. Например, если Вы использовали названия через прописные буквы, то можно настроить
репозитории следующим образом:

```json
{
    "repositories": [
        {
            "type": "vcs",
            "url": "http://svn.example.org/projectA/",
            "trunk-path": "Trunk",
            "branches-path": "Branches",
            "tags-path": "Tags"
        }
    ]
}
```

If you have no branches or tags directory you can disable them entirely by
setting the `branches-path` or `tags-path` to `false`.

Если у Вас нет веток или директорий тегов Вы это можете отключить через
настройки `branches-path` или `tags-path` в `false`.

If the package is in a sub-directory, e.g. `/trunk/foo/bar/composer.json` and
`/tags/1.0/foo/bar/composer.json`, then you can make Composer access it by
setting the `"package-path"` option to the sub-directory, in this example it
would be `"package-path": "foo/bar/"`.

Если пакет находится в поддиректории, например `/trunk/foo/bar/composer.json` и
`/tags/1.0/foo/bar/composer.json` Вы можете сделать доступ к нему через параметр
`"package-path"`, в этом примере это будет `"package-path": "foo/bar/"`.

If you have a private Subversion repository you can save credentials in the
http-basic section of your config (See [Schema](04-schema.md)):

Если у Вас есть личные Subversion репозитории можно сохранить учетные данные в
http-basic секции вашего config (см. [схема](04-schema.md)):

```json
{
    "http-basic": {
        "svn.example.org": {
            "username": "username",
            "password": "password"
        }
    }
}
```

If your Subversion client is configured to store credentials by default these
credentials will be saved for the current user and existing saved credentials
for this server will be overwritten. To change this behavior by setting the
`"svn-cache-credentials"` option in your repository configuration:

Если Ваш клиент Subversion настроен на хранение учетных данных по умолчанию, эти
учетные данные будут сохранены для текущего пользователя и существующие сохраненные учетные данные
для этого сервера будут перезаписаны. Чтобы изменить это поведение, установите
параметр `"svn-cache-credentials"` в конфигурации Вашего репозитория:

```json
{
    "repositories": [
        {
            "type": "vcs",
            "url": "http://svn.example.org/projectA/",
            "svn-cache-credentials": false
        }
    ]
}
```

### PEAR

It is possible to install packages from any PEAR channel by using the `pear`
repository. Composer will prefix all package names with `pear-{channelName}/` to
avoid conflicts. All packages are also aliased with prefix `pear-{channelAlias}/`

Позволяет устанавливать пакеты из любого PEAR канала с помощью `pear` репозитория.
Composer добавит префикс всем именам пакетов на `pear-{channelName}/` во избежании конфликтов.
Все пакеты являются также псевдонимами с префиксом `pear-{channelAlias}/`

Example using `pear2.php.net`:

Пример использования `pear2.php.net`:

```json
{
    "repositories": [
        {
            "type": "pear",
            "url": "https://pear2.php.net"
        }
    ],
    "require": {
        "pear-pear2.php.net/PEAR2_Text_Markdown": "*",
        "pear-pear2/PEAR2_HTTP_Request": "*"
    }
}
```

In this case the short name of the channel is `pear2`, so the
`PEAR2_HTTP_Request` package name becomes `pear-pear2/PEAR2_HTTP_Request`.

В этом случае коротким именем канала является `pear2`, так имя пакета
`PEAR2_HTTP_Request` становится `pear-pear2/PEAR2_HTTP_Request`.

> **Note:** The `pear` repository requires doing quite a few requests per
> package, so this may considerably slow down the installation process.

> **Примечание:** `pear` репозиторий требует делать довольно много запросов на
> пакет, так что это может значительно замедлить процесс установки.

#### Custom vendor alias - Псевдоним пользовательского поставщика

It is possible to alias PEAR channel packages with a custom vendor name.

Возможность псевдонимов пакетов PEAR канала с именем пользовательского поставщика.

Example:
Пример:

Suppose you have a private PEAR repository and wish to use Composer to
incorporate dependencies from a VCS. Your PEAR repository contains the
following packages:

Предположим, у Вас есть личный PEAR репозиторий и Вы хотите использовать Composer
включить зависимости из VCS. Ваш PEAR репозиторий содержит
следующие пакеты:

 * `BasePackage`
 * `IntermediatePackage`, который зависит от `BasePackage`
 * `TopLevelPackage1` и `TopLevelPackage2` которые оба зависят от `IntermediatePackage`

Without a vendor alias, Composer will use the PEAR channel name as the
vendor portion of the package name:

Без псевдонима вендора Composer будет использовать имя PEAR канала как
часть имени пакета вендора:

 * `pear-pear.foobar.repo/BasePackage`
 * `pear-pear.foobar.repo/IntermediatePackage`
 * `pear-pear.foobar.repo/TopLevelPackage1`
 * `pear-pear.foobar.repo/TopLevelPackage2`

Suppose at a later time you wish to migrate your PEAR packages to a
Composer repository and naming scheme, and adopt the vendor name of `foobar`.
Projects using your PEAR packages would not see the updated packages, since
they have a different vendor name (`foobar/IntermediatePackage` vs
`pear-pear.foobar.repo/IntermediatePackage`).

By specifying `vendor-alias` for the PEAR repository from the start, you can
avoid this scenario and future-proof your package names.

To illustrate, the following example would get the `BasePackage`,
`TopLevelPackage1`, and `TopLevelPackage2` packages from your PEAR repository
and `IntermediatePackage` from a Github repository:

```json
{
    "repositories": [
        {
            "type": "git",
            "url": "https://github.com/foobar/intermediate.git"
        },
        {
            "type": "pear",
            "url": "http://pear.foobar.repo",
            "vendor-alias": "foobar"
        }
    ],
    "require": {
        "foobar/TopLevelPackage1": "*",
        "foobar/TopLevelPackage2": "*"
    }
}
```

### Package

If you want to use a project that does not support Composer through any of the
means above, you still can define the package yourself by using a `package`
repository.

Basically, you define the same information that is included in the `composer`
repository's `packages.json`, but only for a single package. Again, the
minimum required fields are `name`, `version`, and either of `dist` or
`source`.

Here is an example for the smarty template engine:

```json
{
    "repositories": [
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
                    "url": "http://smarty-php.googlecode.com/svn/",
                    "type": "svn",
                    "reference": "tags/Smarty_3_1_7/distribution/"
                },
                "autoload": {
                    "classmap": ["libs/"]
                }
            }
        }
    ],
    "require": {
        "smarty/smarty": "3.1.*"
    }
}
```

Typically you would leave the source part off, as you don't really need it.

> **Note**: This repository type has a few limitations and should be avoided
> whenever possible:
>
> - Composer will not update the package unless you change the `version` field.
> - Composer will not update the commit references, so if you use `master` as
>   reference you will have to delete the package to force an update, and will
>   have to deal with an unstable lock file.

## Hosting your own

While you will probably want to put your packages on packagist most of the time,
there are some use cases for hosting your own repository.

* **Private company packages:** If you are part of a company that uses Composer
  for their packages internally, you might want to keep those packages private.

* **Separate ecosystem:** If you have a project which has its own ecosystem,
  and the packages aren't really reusable by the greater PHP community, you
  might want to keep them separate to packagist. An example of this would be
  wordpress plugins.

For hosting your own packages, a native `composer` type of repository is
recommended, which provides the best performance.

There are a few tools that can help you create a `composer` repository.

### Packagist

The underlying application used by packagist is open source. This means that you
can technically install your own copy of packagist. However it is not a
supported use case and changes will happen without caring for third parties
using the code.

Packagist is a Symfony2 application, and it is [available on
GitHub](https://github.com/composer/packagist). It uses Composer internally and
acts as a proxy between VCS repositories and the Composer users. It holds a list
of all VCS packages, periodically re-crawls them, and exposes them as a Composer
repository.

### Toran Proxy

[Toran Proxy](https://toranproxy.com/) is a web app much like Packagist but
providing private package hosting as well as mirroring/proxying of GitHub and
packagist.org. Check its homepage and the [Satis/Toran Proxy article](articles/handling-private-packages-with-satis.md)
for more information.

### Satis

Satis is a static `composer` repository generator. It is a bit like an ultra-
lightweight, static file-based version of packagist.

You give it a `composer.json` containing repositories, typically VCS and
package repository definitions. It will fetch all the packages that are
`require`d and dump a `packages.json` that is your `composer` repository.

Check [the satis GitHub repository](https://github.com/composer/satis) and
the [Satis article](articles/handling-private-packages-with-satis.md) for more
information.

### Artifact

There are some cases, when there is no ability to have one of the previously
mentioned repository types online, even the VCS one. Typical example could be
cross-organisation library exchange through built artifacts. Of course, most
of the times they are private. To simplify maintenance, one can simply use a
repository of type `artifact` with a folder containing ZIP archives of those
private packages:

```json
{
    "repositories": [
        {
            "type": "artifact",
            "url": "path/to/directory/with/zips/"
        }
    ],
    "require": {
        "private-vendor-one/core": "15.6.2",
        "private-vendor-two/connectivity": "*",
        "acme-corp/parser": "10.3.5"
    }
}
```

Each zip artifact is just a ZIP archive with `composer.json` in root folder:

```sh
unzip -l acme-corp-parser-10.3.5.zip

composer.json
...
```

If there are two archives with different versions of a package, they are both
imported. When an archive with a newer version is added in the artifact folder
and you run `update`, that version will be imported as well and Composer will
update to the latest version.

### Path

In addition to the artifact repository, you can use the path one, which allows
you to depend on a local directory, either absolute or relative. This can be
especially useful when dealing with monolithic repositories.

For instance, if you have the following directory structure in your repository:
```
- apps
\_ my-app
  \_ composer.json
- packages
\_ my-package
  \_ composer.json
```

Then, to add the package `my/package` as a dependency, in your `apps/my-app/composer.json`
file, you can use the following configuration:

```json
{
    "repositories": [
        {
            "type": "path",
            "url": "../../packages/my-package"
        }
    ],
    "require": {
        "my/package": "*"
    }
}
```

If the package is a local VCS repository, the version may be inferred by
the branch or tag that is currently checked out. Otherwise, the version should
be explicitly defined in the package's `composer.json` file. If the version
cannot be resolved by these means, it is assumed to be `dev-master`.

The local package will be symlinked if possible, in which case the output in
the console will read `Symlinked from ../../packages/my-package`. If symlinking
is _not_ possible the package will be copied. In that case, the console will
output `Mirrored from ../../packages/my-package`.

Instead of default fallback strategy you can force to use symlink with `"symlink": true`
or mirroring with `"symlink": false` option.
Forcing mirroring can be useful when deploying or generating package from a monolithic repository.

```json
{
    "repositories": [
        {
            "type": "path",
            "url": "../../packages/my-package",
            "options": {
                "symlink": false
            }
        }
    ]
}
```

Leading tildes are expanded to the current user's home folder, and environment
variables are parsed in both Windows and Linux/Mac notations. For example
`~/git/mypackage` will automatically load the mypackage clone from
`/home/<username>/git/mypackage`, equivalent to `$HOME/git/mypackage` or
`%USERPROFILE%/git/mypackage`.

> **Note:** Repository paths can also contain wildcards like ``*`` and ``?``.
> For details, see the [PHP glob function](http://php.net/glob).

## Disabling Packagist

## Отключение Packagist

You can disable the default Packagist repository by adding this to your
`composer.json`:

Можно отключить хранилище Packagist по умолчанию, добавив это в Ваш
`composer.json`:

```json
{
    "repositories": [
        {
            "packagist": false
        }
    ]
}
```

&larr; [Schema](04-schema.md)  |  [Config](06-config.md) &rarr;
