# Libraries
# Библиотеки

This chapter will tell you how to make your library installable through
Composer.

Этот раздел расскажет Вам о том как сделать Ваши библиотеки устанавливаемыми через Composer.

## Every project is a package
## Каждый проект является пакетом

As soon as you have a `composer.json` in a directory, that directory is a
package. When you add a [`require`](04-schema.md#require) to a project, you are
making a package that depends on other packages. The only difference between
your project and libraries is that your project is a package without a name.

Как только у Вас в каталоге появляется `composer.json` , этот каталог становится пакетом.
Когда Вы добавляете зависимости [`require`](04-schema.md#require) в проект, Вы создаёте
 пакет, который зависит от других пакетов. Единственное различие между
Вашим проектом и библиотеками в том, что Ваш проект является пакетом без имени. 

In order to make that package installable you need to give it a name. You do
this by adding the [`name`](04-schema.md#name) property in `composer.json`:

Для того, чтобы сделать пакет установочным Вам нужно дать ему имя. Вы делаете
это путем добавления свойства [`name`](04-schema.md#name) (имя) в `composer.json`:

```json
{
    "name": "acme/hello-world",
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

In this case the project name is `acme/hello-world`, where `acme` is the vendor
name. Supplying a vendor name is mandatory.

В нашем случае именем проекта является `acme/hello-world`, где `acme` является именем поставщика.
Имя поставщика является обязательным.

> **Note:** If you don't know what to use as a vendor name, your GitHub
> username is usually a good bet. While package names are case insensitive, the
> convention is all lowercase and dashes for word separation.

> **Примечание:** Если Вы не знаете, что использовать в качестве имени поставщика, Ваше имя пользователя на GitHub обычно является хорошим выбором. Имена пакетов нечувствительны к регистру, все буквы строчные и дефисы для разделения слов.

## Platform packages
## Пакеты платформ

Composer has platform packages, which are virtual packages for things that are
installed on the system but are not actually installable by Composer. This
includes PHP itself, PHP extensions and some system libraries.

Composer имеет пакеты платформ, которые являются виртуальными пакетами для предметов, которые установлены в системе, но не фактически устанавливаемые через Composer. В это понятие включается сам PHP, PHP расширений и некоторые системные библиотеки.

* `php` represents the PHP version of the user, allowing you to apply
  constraints, e.g. `>=5.4.0`. To require a 64bit version of php, you can
  require the `php-64bit` package.
  
* `php` представляет версию PHP пользователя, позволяя применить ограничения
  , например `>=5.4.0`. Если требуется 64bit версия php, Вы можете
  потребовать пакет `php-64bit`

* `hhvm` represents the version of the HHVM runtime (aka HipHop Virtual
  Machine) and allows you to apply a constraint, e.g., '>=2.3.3'.
  
* `hhvm` представляет версию среды выполнения HHVM (aka HipHop Virtual
  Machine) и позволяет применять ограничения, например, '>=2.3.3'.

* `ext-<name>` allows you to require PHP extensions (includes core
  extensions). Versioning can be quite inconsistent here, so it's often
  a good idea to just set the constraint to `*`.  An example of an extension
  package name is `ext-gd`.
  
* `ext-<name>` позволяет требовать расширения PHP (включая расширения ядра).
  Управление версиями может быть вполне согласованно здесь, так что это часто
  хорошая идея, для того, чтобы просто установить ограничение `*`. Примером расширения
  с именем пакета является `ext-gd`.

* `lib-<name>` allows constraints to be made on versions of libraries used by
  PHP. The following are available: `curl`, `iconv`, `icu`, `libxml`,
  `openssl`, `pcre`, `uuid`, `xsl`.
  
* `lib-<name>` позволяет накладывать ограничения на версии библиотек, используемых
  PHP. Доступны следующие: `curl`, `iconv`, `icu`, `libxml`,
  `openssl`, `pcre`, `uuid`, `xsl`.

You can use [`show --platform`](03-cli.md#show) to get a list of your locally
available platform packages.

Вы можете использовать [`show --platform`](03-cli.md#show) для получения списка локальных пакетов доступных на Вашей платформе.

## Specifying the version
## Указание версии

When you publish your package on Packagist, it is able to infer the version
from the VCS (git, svn, hg, fossil) information. This means you don't have to
explicitly declare it. Read [tags](#tags) and [branches](#branches) to see how
version numbers are extracted from these.

Когда вы публикуете Ваш пакет на Packagist, он выводит информацию о версии
из VCS (git, svn, hg, fossil). Это означает, что Вам не придется
явно объявлять это. Читайте [tags](#tags) (теги) и [branches](#branches) (ветки) чтобы увидеть, как
номера версий извлекаются из них.

If you are creating packages by hand and really have to specify it explicitly,
you can just add a `version` field:

Если Вы создаете пакеты вручную и действительно нужно указать его явно, то
можно просто добавить поле `version` (версии):

```json
{
    "version": "1.0.0"
}
```

> **Note:** You should avoid specifying the version field explicitly, because
> for tags the value must match the tag name.

> **Примечание:** Вы должны избегать явных указаний полей версий потому,
> что для тегов значение должно совпадать с именем тега.

### Tags
### Теги

For every tag that looks like a version, a package version of that tag will be
created. It should match 'X.Y.Z' or 'vX.Y.Z', with an optional suffix of
`-patch` (`-p`), `-alpha` (`-a`), `-beta` (`-b`) or `-RC`. The suffix can also
be followed by a number.

Для каждого тега, который выглядит как версия, пакет с тегом этой версии будет
создан. Он должен соответствовать 'X.Y.Z' или 'vX.Y.Z', с необязательным суффиксом
`-patch` (`-p`), `-alpha` (`-a`), `-beta` (`-b`) или `-RC`. За номером также может следовать суффикс.

Here are a few examples of valid tag names:
Вот несколько примеров допустимых тегов имен:

- 1.0.0
- v1.0.0
- 1.10.5-RC1
- v4.4.4-beta2
- v2.0.0-alpha
- v2.0.4-p1

> **Note:** Even if your tag is prefixed with `v`, a
> [version constraint](01-basic-usage.md#package-versions) in a `require`
> statement has to be specified without prefix (e.g. tag `v1.0.0` will result
> in version `1.0.0`).

> **Примечание:** Даже если Ваш тег с префиксом `v`,
> [ограничение версии](01-basic-usage.md#package-versions) (version constraint) в `require`
> должно быть указано без префикса (например тег `v1.0.0` будет приведён в `1.0.0`).

### Branches
### Ветви

For every branch, a package development version will be created. If the branch
name looks like a version, the version will be `{branchname}-dev`. For example,
the branch `2.0` will get the `2.0.x-dev` version (the `.x` is added for
technical reasons, to make sure it is recognized as a branch). The `2.0.x`
branch would also be valid and be turned into `2.0.x-dev` as well. If the
branch does not look like a version, it will be `dev-{branchname}`. `master`
results in a `dev-master` version.

Для каждой ветви будет создана версия пакета разработки. Если имя ветви выглядит как версия, 
версия будет `{branchname}-dev` (branchname - имя ветви). Например ветка `2.0` получится версия `2.0.x-dev`
( `.x` добавляется по техническим причинам, чтобы убедиться, что она распознается как ветвь).
Ветка `2.0.x` также будет действительна и превратится соответственно в `2.0.x-dev`.
Если ветка не выглядит как версия, она будет `dev-{branchname}`.
Ветка `master` приводит к версии `dev-master`.

Here are some examples of version branch names:
Ниже приведены некоторые примеры имен версий веток:

- 1.x
- 1.0 (приравнивается к 1.0.x)
- 1.1.x

> **Note:** When you install a development version, it will be automatically
> pulled from its `source`. See the [`install`](03-cli.md#install) command
> for more details.

> **Примечание:** При установке версии разработки (development version), 
> это автоматически вытащит её из исходников `source`. Смотрите команду [`install`](03-cli.md#install) (установка)
> для более подробной информации.

### Aliases
### Псевдонимы

It is possible to alias branch names to versions. For example, you could alias
`dev-master` to `1.0.x-dev`, which would allow you to require `1.0.x-dev` in
all the packages.

Это возможность создания псевдонимов имён веток версий. Например, Вы можете
создать псевдоним для `dev-master` как `1.0.x-dev`, что позволит Вам добавлять
зависимость в пакетах как `1.0.x-dev`.

See [Aliases](articles/aliases.md) for more information.
Смотрите [Псевдонимы](articles/aliases.md) для более подробной информации.

## Lock file
## Файл блокировки

For your library you may commit the `composer.lock` file if you want to. This
can help your team to always test against the same dependency versions.
However, this lock file will not have any effect on other projects that depend
on it. It only has an effect on the main project.

Для вашей библиотеки Вы можете зафиксировать файл `composer.lock` если хотите.
Это поможет Вашей команде проверить отношения одной и той же версии зависимостей.
Однако этот файл блокировки не будет иметь никакого влияния на другие проекты, которые зависят от него.
Он только влияет на основной проект.

If you do not want to commit the lock file and you are using git, add it to
the `.gitignore`.

Если Вы не хотите фиксировать файл блокировки и используете git, добавьте его в
`.gitignore`.

## Publishing to a VCS
## Публикация в VCS

Once you have a VCS repository (version control system, e.g. git) containing a
`composer.json` file, your library is already composer-installable. In this
example we will publish the `acme/hello-world` library on GitHub under
`github.com/username/hello-world`.

Как только у Вас появится VCS репозиторий (система управления версиями, например git) содержащий
файл `composer.json`, Ваша библиотека уже может быть установлена через Composer. В этом
примере, мы будем публиковать библиотеку `acme/hello-world` на GitHub под
`github.com/username/hello-world`.

Now, to test installing the `acme/hello-world` package, we create a new
project locally. We will call it `acme/blog`. This blog will depend on
`acme/hello-world`, which in turn depends on `monolog/monolog`. We can
accomplish this by creating a new `blog` directory somewhere, containing a
`composer.json`:

Теперь, чтобы проверить установку пакета `acme/hello-world`, мы создаем новый
локальный проект. Будем называть его `acme/blog`. Этот блог будет зависеть от
`acme/hello-world`, который в свою очередь зависит от `monolog/monolog`. Мы можем
добиться этого путем создания где-то нового каталога `blog`,
содержащего `composer.json` со следующим содержанием:

```json
{
    "name": "acme/blog",
    "require": {
        "acme/hello-world": "dev-master"
    }
}
```

The name is not needed in this case, since we don't want to publish the blog
as a library. It is added here to clarify which `composer.json` is being
described.

Имя в этом случае не требуется, поскольку мы не хотим публиковать блог
как библиотеку. Имя здесь добавляется чтобы уточнить, какие `composer.json`
в настоящее время описываются.

Now we need to tell the blog app where to find the `hello-world` dependency.
We do this by adding a package repository specification to the blog's
`composer.json`:

Теперь мы должны сообщить приложению блог где найти зависимость `hello-world`.
Мы делаем это путем добавления репозитория в спецификации пакета блога
`composer.json`:

```json
{
    "name": "acme/blog",
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/username/hello-world"
        }
    ],
    "require": {
        "acme/hello-world": "dev-master"
    }
}
```

For more details on how package repositories work and what other types are
available, see [Repositories](05-repositories.md).

Для более подробной информации о том, как работают репозитории пакетов и какие другие типы
доступны, см. [Репозитории](05-repositories.md).

That's all. You can now install the dependencies by running Composer's
[`install`](03-cli.md#install) command!

Вот и все. Теперь можно установить зависимости, запустив команду Composer
[`install`](03-cli.md#install) !

**Recap:** Any git/svn/hg/fossil repository containing a `composer.json` can be
added to your project by specifying the package repository and declaring the
dependency in the [`require`](04-schema.md#require) field.

**Резюме:** Любой репозиторий git/svn/hg/fossil содержащий `composer.json` может быть
добавлен в Ваш проект, указав репозитории пакетов и объявив зависимости в поле [`require`](04-schema.md#require).

## Publishing to packagist
## Публикация в packagist

Alright, so now you can publish packages. But specifying the VCS repository
every time is cumbersome. You don't want to force all your users to do that.

Хорошо, теперь Вы можете публиковать пакеты. Но указывать VCS репозитории
каждый раз слишком громоздко. Вы же не хотите заставлять всех пользователей делать это.

The other thing that you may have noticed is that we did not specify a package
repository for `monolog/monolog`. How did that work? The answer is Packagist.

Вы наверное заметили, что мы не указывали репозиторий пакетов для `monolog/monolog`.
Как это работает? Ответ - Packagist.

[Packagist](https://packagist.org/) is the main package repository for
Composer, and it is enabled by default. Anything that is published on
Packagist is available automatically through Composer. Since
[Monolog is on Packagist](https://packagist.org/packages/monolog/monolog), we
can depend on it without having to specify any additional repositories.

[Packagist](https://packagist.org/) является основным репозиторием пакетов для
Composer и он включен по умолчанию. Всё, что публикуется на
Packagist автоматически доступно через Composer. Начиная с
[Monolog находится на Packagist](https://packagist.org/packages/monolog/monolog), мы
можем положиться на него без указания каких-либо дополнительных репозиториев.

If we wanted to share `hello-world` with the world, we would publish it on
Packagist as well. Doing so is really easy.

Если мы захотим поделиться с миром пакетом `hello-world`, мы опубликуем его на packagist.
Сделать это очень легко.

You simply visit [Packagist](https://packagist.org) and hit the "Submit"
button. This will prompt you to sign up if you haven't already, and then
allows you to submit the URL to your VCS repository, at which point Packagist
will start crawling it. Once it is done, your package will be available to
anyone!

Просто посетите [Packagist](https://packagist.org) и нажмите
кнопку "Submit" (Отправить). Это поможет Вам зарегистрироваться, если Вы еще не сделали этого, а затем
позволит добавить URL в ваш VCS репозиторий, с этой точки Packagist
начнёт сканирование его. Как только это будет сделано, Ваш пакет будет доступен для всех!

&larr; [Базовое использование](01-basic-usage.md) |  [Интерфейс командной строки](03-cli.md) &rarr;
