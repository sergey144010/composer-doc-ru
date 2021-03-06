[Оригинал](https://github.com/composer/composer/blob/master/doc/03-cli.md)
[Оглавление](https://github.com/sergey144010/composer-doc-ru/blob/master/README.md#Оглавление)

# Command-line interface / Commands
# Интерфейс командной строки / Команды

You've already learned how to use the command-line interface to do some
things. This chapter documents all the available commands.

Вы уже научились использовать интерфейс командной строки для выполнения некоторых
операций. В этой главе задокументированы все доступные команды.

To get help from the command-line, simply call `composer` or `composer list`
to see the complete list of commands, then `--help` combined with any of those
can give you more information.

Чтобы получить справку в командной строке, просто вызовите `composer` или `composer list`
и увидите полный список доступных команд, затем `--help` в сочетании с любой из этих команд
даст Вам более подробную информацию по команде.

## Global Options
## Глобальные общие параметры

The following options are available with every command:
Следующие параметры доступны с каждой командой:

* **--verbose (-v):** Increase verbosity of messages.
* **--help (-h):** Display help information.
* **--quiet (-q):** Do not output any message.
* **--no-interaction (-n):** Do not ask any interactive question.
* **--no-plugins:** Disables plugins.
* **--working-dir (-d):** If specified, use the given directory as working directory.
* **--profile:** Display timing and memory usage information
* **--ansi:** Force ANSI output.
* **--no-ansi:** Disable ANSI output.
* **--version (-V):** Display this application version.

* **--verbose (-v):** Увеличение детализации сообщений.
* **--help (-h):** Отображения справочной информации.
* **--quiet (-q):** Не выводить никаких сообщений.
* **--no-interaction (-n):** Не спрашивать ничего в интерактивном режиме.
* **--no-plugins:** Отключить плагины.
* **--working-dir (-d):** Если указано, использовать заданный каталог в качестве рабочего каталога.
* **--profile:** Отображает сведения об использовании времени и памяти.
* **--ansi:** Принудительный вывод в ANSI.
* **--no-ansi:** Отключить ANSI вывод.
* **--version (-V):** Отобразить версию приложения.

## Process Exit Codes
## Коды выхода процесса - Коды возврата

* **0:** OK
* **1:** Generic/unknown error code
* **2:** Dependency solving error code

* **0:** OK
* **1:** Generic/unknown error code - Общая/неизвестная ошибка
* **2:** Dependency solving error code - Неразрешенные зависимости

## Команда init

In the [Libraries](02-libraries.md) chapter we looked at how to create a
`composer.json` by hand. There is also an `init` command available that makes
it a bit easier to do this.

В разделе [Библиотеки](02-libraries.md) мы рассмотрели, как создать
`composer.json` вручную. Существует также доступная команда `init`, которая делает
этот процесс немного проще.

When you run the command it will interactively ask you to fill in the fields,
while using some smart defaults.

Когда Вы запускаете команду она интерактивно попросит Вас заполнить некоторые поля,
а некоторые параметры использует по умолчанию.

```sh
php composer.phar init
```

### Options
### Параметры

* **--name:** Name of the package.
* **--description:** Description of the package.
* **--author:** Author name of the package.
* **--homepage:** Homepage of the package.
* **--require:** Package to require with a version constraint. Should be
  in format `foo/bar:1.0.0`.
* **--require-dev:** Development requirements, see **--require**.
* **--stability (-s):** Value for the `minimum-stability` field.
* **--repository:** Provide one (or more) custom repositories. They will be stored
  in the generated composer.json, and used for auto-completion when prompting for
  the list of requires. Every repository can be either an HTTP URL pointing
  to a `composer` repository or a JSON string which similar to what the
  [repositories](04-schema.md#repositories) key accepts.
  
* **--name:** Название пакета.
* **--description:** Описание пакета.
* **--author:** Имя автора пакета.
* **--homepage:** Домашняя страница пакета.
* **--require:** Зависимоть пакета с ограничением версии. Должно быть в формате `foo/bar:1.0.0`.
* **--require-dev:** Development зависимость, см. **--require**.
* **--stability (-s):** Значение для поля `minimum-stability`.
* **--repository:** Включает один (или более) пользовательских репозиториев. Они будут храниться
в созданном composer.json и используются автоматически при выполнении запроса для
списка зависимостей. Каждый репозиторий может быть либо HTTP URL и указывать
на репозиторий `composer` либо JSON строка, которая похожа на ключи в
[репозитории](04-schema.md#repositories).

## Команда install

The `install` command reads the `composer.json` file from the current
directory, resolves the dependencies, and installs them into `vendor`.

Команда `install` считывает файл `composer.json` из текущего каталога,
разрешает зависимости и устанавливает их в `vendor`.

```sh
php composer.phar install
```

If there is a `composer.lock` file in the current directory, it will use the
exact versions from there instead of resolving them. This ensures that
everyone using the library will get the same versions of the dependencies.

Если в текущем каталоге есть файл `composer.lock`, будут установлены точные версии зависимостей из этого файла.
Это гарантирует, что каждый кто использует данную библиотеку получит те же версии зависимостей этой библиотеки.

If there is no `composer.lock` file, Composer will create one after dependency
resolution.

Если отсутствует файл `composer.lock`, Composer создаст его после разрешения зависимостей.

### Options
### Параметры

* **--prefer-source:** There are two ways of downloading a package: `source`
  and `dist`. For stable versions Composer will use the `dist` by default.
  The `source` is a version control repository. If `--prefer-source` is
  enabled, Composer will install from `source` if there is one. This is
  useful if you want to make a bugfix to a project and get a local git
  clone of the dependency directly.
* **--prefer-dist:** Reverse of `--prefer-source`, Composer will install
  from `dist` if possible. This can speed up installs substantially on build
  servers and other use cases where you typically do not run updates of the
  vendors. It is also a way to circumvent problems with git if you do not
  have a proper setup.
* **--ignore-platform-reqs:** ignore `php`, `hhvm`, `lib-*` and `ext-*`
  requirements and force the installation even if the local machine does not
  fulfill these. See also the [`platform`](06-config.md#platform) config option.
* **--dry-run:** If you want to run through an installation without actually
  installing a package, you can use `--dry-run`. This will simulate the
  installation and show you what would happen.
* **--dev:** Install packages listed in `require-dev` (this is the default behavior).
* **--no-dev:** Skip installing packages listed in `require-dev`. The autoloader
  generation skips the `autoload-dev` rules.
* **--no-autoloader:** Skips autoloader generation.
* **--no-scripts:** Skips execution of scripts defined in `composer.json`.
* **--no-progress:** Removes the progress display that can mess with some
  terminals or scripts which don't handle backspace characters.
* **--optimize-autoloader (-o):** Convert PSR-0/4 autoloading to classmap to get a faster
  autoloader. This is recommended especially for production, but can take
  a bit of time to run so it is currently not done by default.
* **--classmap-authoritative (-a):** Autoload classes from the classmap only.
  Implicitly enables `--optimize-autoloader`.
  
* **--prefer-source:** Существует два способа загрузки пакета: `source` и `dist`.
  Для стабильных версий Composer будет использовать `dist` по умолчанию.
  `source` является репозиторием управления версиями.
  Если `--prefer-source` разрешено, Composer будет устанавливать из `source` если таковой имеется.
  Это полезно, если вы хотите внести исправление в проект и получить местные git 
  клон зависимости непосредственно.
* **--prefer-dist:** Противоположность `--prefer-source`, Composer будет устанавливать
  из `dist` если это возможно. Это может существенно ускорить устанавку на сборках
  серверов и в других случаях, где обычно не запускаются обновления вендоров.
  Это также способ, для обхода проблем с git, если Вы не имеете возможности для правильной установки.
* **--ignore-platform-reqs:** Игнорировать `php`, `hhvm`, `lib-*` and `ext-*`
  зависимости и принудительно установить даже если локальная машина не выполнит это.
  Смотрите также конфигурационный параметр [`platform`](06-config.md#platform).
* **--dry-run:** Если Вы хотите запустить установку пакета без фактической
  установки, Вы можете использовать `--dry-run`. Это симитирует установку и
  покажет Вам, что произойдет.
* **--dev:** Установить пакеты перечисленные в `require-dev` (это поведение по умолчанию).
* **--no-dev:** Пропустить установку пакетов перечисленных в `require-dev`. Автозагрузчик пропустит сгенерированные правила `autoload-dev`.
* **--no-autoloader:** Пропустить сгенерированный автозагрузчик.
* **--no-scripts:** Пропускать выполнение скриптов определенных в `composer.json`.
* **--no-progress:** Удаляет отображение прогресса, который может возиться с некоторыми
  терминалами или сценариями, которые не обрабатывают символы возврата.
* **--optimize-autoloader (-o):** Преобразует PSR-0/4 classmap автозагрузчики, чтобы получить быстрый автозагрузчик. Это особенно рекомендуется для режима production, но может занять немного больше времени для запуска, так что в настоящее время это не делается по умолчанию.
* **--classmap-authoritative (-a):** Автозагрузка классов только из classmap.
  Неявно включает `--optimize-autoloader`.

## Команда update

In order to get the latest versions of the dependencies and to update the
`composer.lock` file, you should use the `update` command.

Для того, чтобы получить самые последние версии зависимостей и обновить
файл `composer.lock`, Вы должны использовать команду `update`.

```sh
php composer.phar update
```

This will resolve all dependencies of the project and write the exact versions
into `composer.lock`.

Это разрешит все зависимости проекта и запишет точные версии
в `composer.lock`.

If you just want to update a few packages and not all, you can list them as such:

Если Вы просто хотите обновить не все, а несколько пакетов, Вы можете перечислить их таким образом:

```sh
php composer.phar update vendor/package vendor/package2
```

You can also use wildcards to update a bunch of packages at once:

Также можно использовать подстановочные знаки для обновления кучи пакетов одновременно:

```sh
php composer.phar update vendor/*
```

### Options
### Параметры

* **--prefer-source:** Install packages from `source` when available.
* **--prefer-dist:** Install packages from `dist` when available.
* **--ignore-platform-reqs:** ignore `php`, `hhvm`, `lib-*` and `ext-*`
  requirements and force the installation even if the local machine does not
  fulfill these. See also the [`platform`](06-config.md#platform) config option.
* **--dry-run:** Simulate the command without actually doing anything.
* **--dev:** Install packages listed in `require-dev` (this is the default behavior).
* **--no-dev:** Skip installing packages listed in `require-dev`. The autoloader generation skips the `autoload-dev` rules.
* **--no-autoloader:** Skips autoloader generation.
* **--no-scripts:** Skips execution of scripts defined in `composer.json`.
* **--no-progress:** Removes the progress display that can mess with some
  terminals or scripts which don't handle backspace characters.
* **--optimize-autoloader (-o):** Convert PSR-0/4 autoloading to classmap to get a faster
  autoloader. This is recommended especially for production, but can take
  a bit of time to run so it is currently not done by default.
* **--classmap-authoritative (-a):** Autoload classes from the classmap only.
  Implicitly enables `--optimize-autoloader`.
* **--lock:** Only updates the lock file hash to suppress warning about the
  lock file being out of date.
* **--with-dependencies:** Add also all dependencies of whitelisted packages to the whitelist.
* **--root-reqs:** Restricts the update to your first degree dependencies.
* **--prefer-stable:** Prefer stable versions of dependencies.
* **--prefer-lowest:** Prefer lowest versions of dependencies. Useful for testing minimal
  versions of requirements, generally used with `--prefer-stable`.
  
* **--prefer-source:** Установить пакеты из `source` если это возможно.
* **--prefer-dist:** Установить пакеты из `dist` если это возможно.
++++++++ повторяется +++++++++
* **--ignore-platform-reqs:** ignore `php`, `hhvm`, `lib-*` and `ext-*`
  requirements and force the installation even if the local machine does not
  fulfill these. See also the [`platform`](06-config.md#platform) config option.
* **--dry-run:** Simulate the command without actually doing anything.
* **--dev:** Install packages listed in `require-dev` (this is the default behavior).
* **--no-dev:** Skip installing packages listed in `require-dev`. The autoloader generation skips the `autoload-dev` rules.
* **--no-autoloader:** Skips autoloader generation.
* **--no-scripts:** Skips execution of scripts defined in `composer.json`.
* **--no-progress:** Removes the progress display that can mess with some
  terminals or scripts which don't handle backspace characters.
* **--optimize-autoloader (-o):** Convert PSR-0/4 autoloading to classmap to get a faster
  autoloader. This is recommended especially for production, but can take
  a bit of time to run so it is currently not done by default.
* **--classmap-authoritative (-a):** Autoload classes from the classmap only.
  Implicitly enables `--optimize-autoloader`.
+++++++++++++
* **--lock:** Обновляет только хэш файла блокировки для подавления предупреждений о устаревшем файле блокировки.
* **--with-dependencies:** Добавляет также все зависимости пакетов whitelisted в белый список (whitelist).
* **--root-reqs:** Ограничивает обновление Ваших первых уровней зависимостей.
* **--prefer-stable:** Отдавать предпочтение стабильным версиям зависимостей.
* **--prefer-lowest:** Отдавать предпочтение более низким версиям зависимостей. Используется для тестирования работы с зависимостями минимальных версий, как правило используется с `--prefer-stable`.

## Команда require

The `require` command adds new packages to the `composer.json` file from
the current directory. If no file exists one will be created on the fly.

Команда `require` добавляет новые пакеты в файл `composer.json` из текущей директории.
Если файла не существует он будет создан на лету.

```sh
php composer.phar require
```

After adding/changing the requirements, the modified requirements will be
installed or updated.

После добавления/изменения зависимостей, измененные зависимости будут
установлены или обновлены.

If you do not want to choose requirements interactively, you can just pass them
to the command.

Если Вы не хотите набирать требования в интерактивном режиме, Вы можете просто передать их в команду.

```sh
php composer.phar require vendor/package:2.* vendor/package2:dev-master
```

### Options
### Параметры

параметры повторяются

* **--prefer-source:** Install packages from `source` when available.
* **--prefer-dist:** Install packages from `dist` when available.
* **--ignore-platform-reqs:** ignore `php`, `hhvm`, `lib-*` and `ext-*`
  requirements and force the installation even if the local machine does not
  fulfill these. See also the [`platform`](06-config.md#platform) config option.
* **--dev:** Add packages to `require-dev`.
* **--no-update:** Disables the automatic update of the dependencies.
* **--no-progress:** Removes the progress display that can mess with some
  terminals or scripts which don't handle backspace characters.
* **--no-scripts:** Skips execution of scripts defined in `composer.json`.
* **--update-no-dev:** Run the dependency update with the `--no-dev` option.
* **--update-with-dependencies:** Also update dependencies of the newly
  required packages.
* **--sort-packages:** Keep packages sorted in `composer.json`.
* **--optimize-autoloader (-o):** Convert PSR-0/4 autoloading to classmap to
  get a faster autoloader. This is recommended especially for production, but
  can take a bit of time to run so it is currently not done by default.
* **--classmap-authoritative (-a):** Autoload classes from the classmap only.
  Implicitly enables `--optimize-autoloader`.
* **--prefer-stable:** Prefer stable versions of dependencies.
* **--prefer-lowest:** Prefer lowest versions of dependencies. Useful for testing minimal
  versions of requirements, generally used with `--prefer-stable`.

## Команда remove

The `remove` command removes packages from the `composer.json` file from
the current directory.

Команда `remove` удаляет пакеты из файла `composer.json` в текущей директории.

```sh
php composer.phar remove vendor/package vendor/package2
```

After removing the requirements, the modified requirements will be
uninstalled.

После удаления зависимостей из файла, эти зависимости будут удалены из проекта.

### Options
### Параметры

параметры повторяются

* **--ignore-platform-reqs:** ignore `php`, `hhvm`, `lib-*` and `ext-*`
  requirements and force the installation even if the local machine does not
  fulfill these. See also the [`platform`](06-config.md#platform) config option.
* **--dev:** Remove packages from `require-dev`.
* **--no-update:** Disables the automatic update of the dependencies.
* **--no-progress:** Removes the progress display that can mess with some
  terminals or scripts which don't handle backspace characters.
* **--no-scripts:** Skips execution of scripts defined in `composer.json`.
* **--update-no-dev:** Run the dependency update with the --no-dev option.
* **--update-with-dependencies:** Also update dependencies of the removed packages.
* **--optimize-autoloader (-o):** Convert PSR-0/4 autoloading to classmap to
  get a faster autoloader. This is recommended especially for production, but
  can take a bit of time to run so it is currently not done by default.
* **--classmap-authoritative (-a):** Autoload classes from the classmap only.
  Implicitly enables `--optimize-autoloader`.

## Команда global

The global command allows you to run other commands like `install`, `require`
or `update` as if you were running them from the [COMPOSER_HOME](#composer-home)
directory.

Команда global позволяет запускать другие команды такие как `install`, `require`
или `update` как если бы Вы запускали их из директории [COMPOSER_HOME](#composer-home).

This is merely a helper to manage a project stored in a central location that
can hold CLI tools or Composer plugins that you want to have available everywhere.

!!! Перевести
Это просто вспомогательный для управления проектом, хранится в центральном расположении,
может содержать инструменты CLI или Composer плагинов, которые вы хотите иметь доступны везде.
!!!

This can be used to install CLI utilities globally. Here is an example:

Это может использоваться для установки глобальных утилит CLI. Вот пример:

```sh
php composer.phar global require fabpot/php-cs-fixer
```

Now the `php-cs-fixer` binary is available globally. Just make sure your global
[vendor binaries](articles/vendor-binaries.md) directory is in your `$PATH`
environment variable, you can get its location with the following command :

Теперь `php-cs-fixer` бинарник доступен глобально. Просто убедитесь, что Ваш глобальный
[vendor binaries](articles/vendor-binaries.md) находится в вашей переменной среды `$PATH`,
Вы можете получить его местоположение с помощью следующей команды:

```sh
php composer.phar global config bin-dir --absolute
```

If you wish to update the binary later on you can just run a global update:

Если Вы захотите потом обновить бинарник, можете просто запустить глобальное обновление:

```sh
php composer.phar global update
```

## Команда search

The search command allows you to search through the current project's package
repositories. Usually this will be just packagist. You simply pass it the
terms you want to search for.

Команда search (поиск) позволяет искать через репозитории пакеты текущего проекта. Обычно это будет только packagist. Вы просто передаёте условия, которые нужны для поиска.

```sh
php composer.phar search monolog
```

You can also search for more than one term by passing multiple arguments.

Вы также можете искать более чем один пакет, передав несколько аргументов.

### Options
### Параметры

* **--only-name (-N):** Искать только в названии.

## Команда show

To list all of the available packages, you can use the `show` command.
Чтобы получить список всех доступных пакетов, можно использовать команду `show`.

```sh
php composer.phar show
```

To filter the list you can pass a package mask using wildcards.

Для фильтрации списка можно передать масу пакетов с использованием подстановочных знаков.

```sh
php composer.phar show monolog/*

monolog/monolog 1.19.0 Sends your logs to files, sockets, inboxes, databases and various web services
```

If you want to see the details of a certain package, you can pass the package
name.

Если хотите увидеть детали определенного пакета, можете передать имя пакета.

```sh
php composer.phar show monolog/monolog

name     : monolog/monolog
versions : master-dev, 1.0.2, 1.0.1, 1.0.0, 1.0.0-RC1
type     : library
names    : monolog/monolog
source   : [git] https://github.com/Seldaek/monolog.git 3d4e60d0cbc4b888fe5ad223d77964428b1978da
dist     : [zip] https://github.com/Seldaek/monolog/zipball/3d4e60d0cbc4b888fe5ad223d77964428b1978da 3d4e60d0cbc4b888fe5ad223d77964428b1978da
license  : MIT

autoload
psr-0
Monolog : src/

requires
php >=5.3.0
```

You can even pass the package version, which will tell you the details of that
specific version.

Вы даже можете передать версию пакета, чтобы получить сведения о конкретной версии.

```sh
php composer.phar show monolog/monolog 1.0.2
```

### Options
### Параметры

* **--latest (-l):** List all installed packages including their latest version.
* **--all (-a):** List all packages available in all your repositories.
* **--installed (-i):** List the packages that are installed (this is enabled by default, and deprecated).
* **--platform (-p):** List only platform packages (php & extensions).
* **--self (-s):** List the root package info.
* **--tree (-t):** List your dependencies as a tree. If you pass a package name it will show the dependency tree for that package.
* **--name-only (-N):** List package names only.
* **--path (-P):** List package paths.
* **--outdated (-o):** Implies --latest, but this lists *only* packages that have a newer version available.
* **--direct (-D):** Restricts the list of packages to your direct dependencies.


* **--latest (-l):** Список всех установленных пакетов, включая их последнюю версию.
* **--all (-a):** Список всех доступных пакетов в вашем репозиториии.
* **--installed (-i):** Список установленных пакетов (это включено по умолчанию и нерекомендуется).
* **--platform (-p):** Список только пакетов платформы (php & расширений).
* **--self (-s):** Список информации корневого пакета.
* **--tree (-t):** Список зависимостей в виде дерева. Если передать имя пакета, будет показано дерево зависимостей для этого пакета.
* **--name-only (-N):** Список только имён пакетов.
* **--path (-P):** Список путей пакетов.
* **--outdated (-o):** Подразумевается --latest (последняя), но это список *только* пакетов, которые имеют более новые версии.
* **--direct (-D):** Ограничивает список пакетов для прямых зависимостей.

## Команда outdated

The `outdated` command shows a list of installed packages that have updates available,
including their current and latest versions. This is basically an alias for
`composer show -lo`.

Команда `outdated` показывает список установленных пакетов, для которых доступны обновления, включая их текущие и последние версии обновлений. В основном это псевдоним для `composer show -lo`.

The color coding is as such:

Цвета говорят о следующем:

- **green**: Dependency is in the latest version and is up to date.
- **yellow**: Dependency has a new version available that includes backwards compatibility breaks according to semver, so upgrade when
  you can but it may involve work.
- **red**: Dependency has a new version that is semver-compatible and you should upgrade it.

- **зелёный**: Это последняя версия зависимости на сегодняшний день.
- **жёлтый**: Зависимость имеет новую версию, которая включает в себя обратную совместимость, поэтому обновляйтесь, когда сможете и можете включать её в работу.
- **красный**: Зависимость имеет новую версию которая semver-compatible и Вы должны обновить её.

### Options
### Параметры

* **--all (-a):** Show all packages, not just outdated (alias for `composer show -l`).
* **--direct (-D):** Restricts the list of packages to your direct dependencies.

* **--all (-a):** Показать все пакеты, не только устаревшие (псевдоним для `composer show -l`).
* **--direct (-D):** Ограничивает список пакетов для прямых зависимостей.

## Команда browse / home

The `browse` (aliased to `home`) opens a package's repository URL or homepage
in your browser.

Команда `browse` (псевдоним `home`) открывает URL репозитория пакетов или домашнюю страницу в браузере.

### Options
### Параметры

* **--homepage (-H):** Open the homepage instead of the repository URL.

* **--homepage (-H):** Открывает домашнюю страницу вместо URL хранилища.

## Команда suggests

Lists all packages suggested by currently installed set of packages. You can
optionally pass one or multiple package names in the format of `vendor/package`
to limit output to suggestions made by those packages only.

Список всех пакетов установленных в настоящее время. Вы можете при необходимости передать имена одного или нескольких пакетов в формате `vendor/package`, что бы ограничить вывод только тех пакетов которые требуются.

Use the `--by-package` or `--by-suggestion` flags to group the output by
the package offering the suggestions or the suggested packages respectively.

Используйте флаги `--by-package` или `--by-suggestion` для группирования выходных пакетов.

### Options
### Параметры

* **--by-package:** Groups output by suggesting package.
* **--by-suggestion:** Groups output by suggested package.
* **--no-dev:** Excludes suggestions from `require-dev` packages.
* **--verbose (-v):** Increased verbosity adds suggesting package name and
  reason for suggestion.
  
* **--by-package:** Группирует вывод предлагаемых пакетов.
* **--by-suggestion:** Группирует вывод предлагаемых пакетов.
* **--no-dev:** Исключает варианты из `require-dev` пакетов.
* **--verbose (-v):** Повышает детализацию добавляя предлагаемое имя пакета и причину.

## Команда depends

The `depends` command tells you which other packages depend on a certain
package. As with installation `require-dev` relationships are only considered
for the root package.

Команда `depends` говорит, что другие пакеты зависят от определенного
пакета. Как с установкой `require-dev` отношения рассматриваются только
для корневого пакета.

```sh
php composer.phar depends doctrine/lexer
 doctrine/annotations v1.2.7 requires doctrine/lexer (1.*)
 doctrine/common      v2.6.1 requires doctrine/lexer (1.*)
```

You can optionally specify a version constraint after the package to limit the
search.

Можно указать ограничение версии после пакета, чтобы сузить поиск.

Add the `--tree` or `-t` flag to show a recursive tree of why the package is
depended upon, for example:

Добавьте флаги `--tree` или `-t`, чтобы увидеть дерево рекурсии зависимостей пакета, например:

```sh
php composer.phar depends psr/log -t
psr/log 1.0.0 Common interface for logging libraries
|- aboutyou/app-sdk 2.6.11 (requires psr/log 1.0.*)
|  `- __root__ (requires aboutyou/app-sdk ^2.6)
|- monolog/monolog 1.17.2 (requires psr/log ~1.0)
|  `- laravel/framework v5.2.16 (requires monolog/monolog ~1.11)
|     `- __root__ (requires laravel/framework ^5.2)
`- symfony/symfony v3.0.2 (requires psr/log ~1.0)
   `- __root__ (requires symfony/symfony ^3.0)
```

### Options
### Параметры

* **--recursive (-r):** Recursively resolves up to the root package.
* **--tree (-t):** Prints the results as a nested tree, implies -r.

* **--recursive (-r):** Рекурсия разрешается до корня пакета.
* **--tree (-t):** Выводит результаты в виде вложенного дерева, подразумевает -r.

## Команда prohibits

The `prohibits` command tells you which packages are blocking a given package
from being installed. Specify a version constraint to verify whether upgrades
can be performed in your project, and if not why not. See the following
example:

Команда `prohibits` говорит о том, какие пакеты блокируют установку данного пакета.
Укажите ограничение версии для проверки могут ли быть выполнены обновления в вашем проекте и если нет то почему.
Смотрите следующий пример: 

```sh
php composer.phar prohibits symfony/symfony 3.1
 laravel/framework v5.2.16 requires symfony/var-dumper (2.8.*|3.0.*)
```

Note that you can also specify platform requirements, for example to check
whether you can upgrade your server to PHP 8.0:

Обратите внимание, что можно также указать требования к платформе для проверки
поддерживает ли Ваш сервер PHP 8.0:

```sh
php composer.phar prohibits php:8
 doctrine/cache        v1.6.0 requires php (~5.5|~7.0)
 doctrine/common       v2.6.1 requires php (~5.5|~7.0)
 doctrine/instantiator 1.0.5  requires php (>=5.3,<8.0-DEV)
```

As with `depends` you can request a recursive lookup, which will list all
packages depending on the packages that cause the conflict.

Как и с `depends` Вы можете запросить рекурсивный поиск, который выведет список всех
пакетов которые вызывают конфликт.

### Options
### Параметры

* **--recursive (-r):** Recursively resolves up to the root package.
* **--tree (-t):** Prints the results as a nested tree, implies -r.

* **--recursive (-r):** Рекурсивно разрешает до корня пакета.
* **--tree (-t):** Выводит результаты в виде вложенного дерева, подразумевает -r.

## Команда validate

You should always run the `validate` command before you commit your
`composer.json` file, and before you tag a release. It will check if your
`composer.json` is valid.

Всегда следует выполнять команду `validate` (проверить) прежде чем фиксировать 
файл `composer.json`, и до того, как делать релиз. Он будет проверять является ли допустимым Ваш файл
`composer.json`.

```sh
php composer.phar validate
```

### Options
### Параметры

* **--no-check-all:** Do not emit a warning if requirements in `composer.json` use unbound version constraints.
* **--no-check-lock:** Do not emit an error if `composer.lock` exists and is not up to date.
* **--no-check-publish:** Do not emit an error if `composer.json` is unsuitable for publishing as a package on Packagist but is otherwise valid.

* **--no-check-all:** Не выбрасывать предупреждение если зависимости в `composer.json` не связаны.
* **--no-check-lock:** Не выбрасывать ошибку если `composer.lock` существует и не является актуальным.
* **--no-check-publish:** Не выбрасывать ошибку если `composer.json` непригоден для публикации как пакет на Packagist, но действителен.

## Команда status

If you often need to modify the code of your dependencies and they are
installed from source, the `status` command allows you to check if you have
local changes in any of them.

Если часто требуется изменять код Ваших зависимостей и они
устанавливались из исходников, команда `status` позволяет проверить, есть ли у Вас
локальные изменения в любой из них.

```sh
php composer.phar status
```

With the `--verbose` option you get some more information about what was
changed:

С параметром `--verbose` Вы получите более подробную информацию о том, что изменено:

```sh
php composer.phar status -v

You have changes in the following dependencies:
vendor/seld/jsonlint:
    M README.mdown
```

## Команда self-update

To update Composer itself to the latest version, just run the `self-update`
command. It will replace your `composer.phar` with the latest version.

Чтобы обновить Composer до последней версии, просто запустите команду `self-update`.
Она заменит Ваш `composer.phar` до последней версии.

```sh
php composer.phar self-update
```

If you would like to instead update to a specific release simply specify it:

Если Вы хотите вместо этого обновления конкретную версию просто укажите её:

```sh
php composer.phar self-update 1.0.0-alpha7
```

If you have installed Composer for your entire system (see [global installation](00-intro.md#globally)),
you may have to run the command with `root` privileges

Если Вы установили Composer для всей Вашей системы (см. [глобальная установка](00-intro.md#globally)), то может потребоваться выполнить команду с правами `root`

```sh
sudo -H composer self-update
```

### Options
### Параметры

* **--rollback (-r):** Rollback to the last version you had installed.
* **--clean-backups:** Delete old backups during an update. This makes the
  current version of Composer the only backup available after the update.
  
* **--rollback (-r):** Откат последней версии, которую вы установили.
* **--clean-backups:** Удаляет старые бекапы во время обновления. После обновления становится доступна только текущая версия Composer.

## Команда config

The `config` command allows you to edit composer config settings and repositories
in either the local `composer.json` file or the global `config.json` file.

Команда `config` позволяет редактировать параметры конфигурации Composer и репозиториев в локальных `composer.json` файлах или глобальном `config.json` файле.

Additionally it lets you edit most properties in the local `composer.json`.

Кроме того она позволяет редактировать большинство свойств в локальном `composer.json`.

```sh
php composer.phar config --list
```

### Usage
### Использование

`config [options] [setting-key] [setting-value1] ... [setting-valueN]`

`setting-key` is a configuration option name and `setting-value1` is a
configuration value.  For settings that can take an array of values (like
`github-protocols`), more than one setting-value arguments are allowed.

`setting-key` это название параметра конфигурации, а `setting-value1` это
значение параметра конфигурации. Для параметров, которые могут принимать массив значений (как `github-protocols`) разрешено более чем одно значение параметра.

You can also edit the values of the following properties:

Вы также можете редактировать значения следующих свойств:

`description`, `homepage`, `keywords`, `license`, `minimum-stability`,
`name`, `prefer-stable`, `type` and `version`.

See the [Config](06-config.md) chapter for valid configuration options.

Смотрите раздел [Конфиг](06-config.md) допустимые параметры конфигурации.

### Options
### Параметры

* **--global (-g):** Operate on the global config file located at
  `$COMPOSER_HOME/config.json` by default.  Without this option, this command
  affects the local composer.json file or a file specified by `--file`.
* **--editor (-e):** Open the local composer.json file using in a text editor as
  defined by the `EDITOR` env variable.  With the `--global` option, this opens
  the global config file.
* **--unset:** Remove the configuration element named by `setting-key`.
* **--list (-l):** Show the list of current config variables.  With the `--global`
  option this lists the global configuration only.
* **--file="..." (-f):** Operate on a specific file instead of composer.json. Note
  that this cannot be used in conjunction with the `--global` option.
* **--absolute:** Returns absolute paths when fetching *-dir config values
  instead of relative.
  
* **--global (-g):** Глобальный конфигурационный файл по умолчанию расположен в `$COMPOSER_HOME/config.json`.
  Без этого параметра команда config влияет на местный composer.json файл или файл указанный в `--file`.
* **--editor (-e):** Откроет локальный composer.json файл при помощи текстового редактора определённого в переменной окружения `EDITOR`.
  С параметром `--global` открывает глобальный конфигурационный файл.
* **--unset:** Удалит элемент конфигурации названный через `setting-key`.
* **--list (-l):** Покажет список текущих переменных конфигурации. С параметром `--global` покажет только список глобальной конфигурации.
* **--file="..." (-f):** Работать с определённым файлом вместо composer.json.
  Обратите внимание, что это не может использоваться в сочетании с `--global` вариантом.
* **--absolute:** Возвращает абсолютные пути при выборке *-dir конфигурационных значений вместо относительных.

### Modifying Repositories
### Изменения репозиториев

In addition to modifying the config section, the `config` command also supports making
changes to the repositories section by using it the following way:

В дополнение к изменению разделов конфигурации, команда `config` также поддерживает внесение изменений в разделы репозиториев, используется следующим образом:

```sh
php composer.phar config repositories.foo vcs https://github.com/foo/bar
```

If your repository requires more configuration options, you can instead pass its JSON representation :

Если Вашему репозиторию необходимы дополнительные параметры конфигурации, можно передать их в JSON формате:

```sh
php composer.phar config repositories.foo '{"type": "vcs", "url": "http://svn.example.org/my-project/", "trunk-path": "master"}'
```

### Modifying Extra Values
### Изменения дополнительных значений

In addition to modifying the config section, the `config` command also supports making
changes to the extra section by using it the following way:

В дополнение к изменению разделов конфигурации, команда `config` также поддерживает внесение изменений в дополнительный раздел, используется следующим образом:

```sh
php composer.phar config extra.foo.bar value
```

The dots indicate array nesting, a max depth of 3 levels is allowed though. The above
would set `"extra": { "foo": { "bar": "value" } }`.

Точки обозначают массив вложенности, допускается максимальная глубина на 3 уровня. Выше
будет `"extra": { "foo": { "bar": "value" } }`.

## Команда create-project

You can use Composer to create new projects from an existing package. This is
the equivalent of doing a git clone/svn checkout followed by a "composer install"
of the vendors.

Composer можно использовать для создания новых проектов из существующего пакета. Это
эквивалентно git clone/svn далее "composer install".

There are several applications for this:

Существует несколько приложений для этого:

1. You can deploy application packages.
2. You can check out any package and start developing on patches for example.
3. Projects with multiple developers can use this feature to bootstrap the
   initial application for development.
   
1. Можно развернуть пакеты приложений.
2. Можете проверить любой пакет и начать разработку патчей к примеру.
3. Проекты с несколькими разработчиками могут использовать эту функцию для загрузки начального приложения для дальнейшей разрабоки.

To create a new project using Composer you can use the "create-project" command.
Pass it a package name, and the directory to create the project in. You can also
provide a version as third argument, otherwise the latest version is used.

Для создания нового проекта с использованием Composer можно использовать команду "create-project".
Передайте имя пакета и каталог для создания проекта. Вы также можете
предоставить версию в качестве третьего аргумента, в противном случае используется последняя версия.

If the directory does not currently exist, it will be created during installation.

Если каталог не существует в настоящее время, он будет создан во время установки.

```sh
php composer.phar create-project doctrine/orm path 2.2.*
```

It is also possible to run the command without params in a directory with an
existing `composer.json` file to bootstrap a project.

Также можно запустить команду без параметров в каталоге с
существующим файлом `composer.json` для загрузки проекта.

By default the command checks for the packages on packagist.org.

По умолчанию команда проверяет пакеты на packagist.org.

### Options
### Параметры

* **--repository:** Provide a custom repository to search for the package,
  which will be used instead of packagist. Can be either an HTTP URL pointing
  to a `composer` repository, a path to a local `packages.json` file, or a
  JSON string which similar to what the [repositories](04-schema.md#repositories)
  key accepts.
* **--stability (-s):** Minimum stability of package. Defaults to `stable`.
* **--prefer-source:** Install packages from `source` when available.
* **--prefer-dist:** Install packages from `dist` when available.
* **--dev:** Install packages listed in `require-dev`.
* **--no-install:** Disables installation of the vendors.
* **--no-scripts:** Disables the execution of the scripts defined in the root
  package.
* **--no-progress:** Removes the progress display that can mess with some
  terminals or scripts which don't handle backspace characters.
* **--keep-vcs:** Skip the deletion of the VCS metadata for the created
  project. This is mostly useful if you run the command in non-interactive
  mode.
* **--ignore-platform-reqs:** ignore `php`, `hhvm`, `lib-*` and `ext-*`
  requirements and force the installation even if the local machine does not
  fulfill these.
  
* **--repository:** Указывает на пользовательское хранилище для поиска пакета, которое будет использоваться вместо packagist. Может быть либо URL HTTP указывающий на репозиторий `composer`, либо путём к локальному файлу `packages.json`, либо JSON строкой, которая похожа на ключи [репозитория](04-schema.md#repositories).
* **--stability (-s):** Минимальная стабильность пакета. По умолчанию `stable`.
* **--prefer-source:** Установка пакетов из `source` если возможно.
* **--prefer-dist:** Установка пакетов из `dist` если возможно.
* **--dev:** Установить пакеты, перечисленные в `require-dev`.
* **--no-install:** Отключает установку поставщиков.
* **--no-scripts:** Отключает выполнение сценариев, определенных в корневом пакете.
* **--no-progress:** Удаляет отображения прогресса, который может возиться с некоторыми терминалами или сценариями, которые не обрабатывают символы возврата.
* **--keep-vcs:** Пропустить удаление VCS метаданных для созданного проекта. Это особенно полезно, если Вы запускаете команду в неинтерактивном режиме.
* **--ignore-platform-reqs:** игнорировать `php`, `hhvm`, `lib-*` and `ext-*`
  зависимости и принудительно установить, даже если локальный компьютер не выполнит эти.

## Команда dump-autoload

If you need to update the autoloader because of new classes in a classmap
package for example, you can use "dump-autoload" to do that without having to
go through an install or update.

Если Вам нужно обновить автозагрузчик из-за новых классов в classmap,
Вы можете использовать "dump-autoload" чтобы сделать это без необходимости прохождения через установку или обновление.

Additionally, it can dump an optimized autoloader that converts PSR-0/4 packages
into classmap ones for performance reasons. In large applications with many
classes, the autoloader can take up a substantial portion of every request's
time. Using classmaps for everything is less convenient in development, but
using this option you can still use PSR-0/4 for convenience and classmaps for
performance.

Кроме того это также может выбросить оптимизированный автозагрузчик, который преобразует PSR-0/4 пакеты в classmap из соображений производительности. В больших приложениях с многими классами автозагрузчик может занять значительную часть времени каждого запроса. Использовать classmaps для всего этого менее удобно, в области развития, но с помощью этого параметра можно использовать PSR-0/4 для удобства и classmaps для производительности.

### Options
### Параметры

* **--optimize (-o):** Convert PSR-0/4 autoloading to classmap to get a faster
  autoloader. This is recommended especially for production, but can take
  a bit of time to run so it is currently not done by default.
* **--classmap-authoritative (-a):** Autoload classes from the classmap only.
  Implicitly enables `--optimize`.
* **--no-dev:** Disables autoload-dev rules.

* **--optimize (-o):** Преобразует PSR-0/4 автозагрузчики classmap чтобы получить более быстрый автозагрузчик. Это особенно актуально для режима production, но может занимать немного больше времени при запуске, так что в настоящее время это не делается по умолчанию.
* **--classmap-authoritative (-a):** Автозагрузка классов только из classmap. Неявно включает `--optimize`.
* **--no-dev:** Запрещает правила autoload-dev.

## Команда clear-cache

Deletes all content from Composer's cache directories.

Удаляет все содержимое из кэш директорий Composer.

## Команда licenses

Lists the name, version and license of every package installed. Use
`--format=json` to get machine readable output.

Содержит имя, версию и лицензию каждого пакета. Использовать
`--format=json` чтобы получить машиночитаемый формат.

### Options
### Параметры

* **--no-dev:** Remove dev dependencies from the output
* **--format:** Format of the output: text or json (default: "text")

* **--no-dev:** Удаляет dev зависимости из выходных данных
* **--format:** Формат вывода: текст или json (по умолчанию: "text")

## Команда run-script

### Options
### Параметры

* **--timeout:** Set the script timeout in seconds, or 0 for no timeout.
* **--no-dev:** Disable dev mode
* **--list:** List user defined scripts

* **--timeout:** Установить таймаут сценария в секундах, или 0 - нет таймаута.
* **--no-dev:** Запретить dev режим
* **--list:** Список пользовательских скриптов

To run [scripts](articles/scripts.md) manually you can use this command,
just give it the script name and optionally any required arguments.

Для запуска [сценариев](articles/scripts.md) вручную, Вы можете использовать эту команду,
просто передайте имя сценария и при необходимости любые необходимые аргументы.

## Команда exec

Executes a vendored binary/script. You can execute any command and this will
ensure that the Composer bin-dir is pushed on your PATH before the command
runs.

!!!
Выполняет бинарник/скрипт поставщика. Вы можете выполнить любую команду, и это будет гарантировать, что композитор Бен dir помещается на вашем пути, прежде чем выполняется команда.
!!!

### Options
### Параметры

* **--list:** List the available composer binaries

* **--list:** Список доступных Composer двоичных файлов

## Команда diagnose

If you think you found a bug, or something is behaving strangely, you might
want to run the `diagnose` command to perform automated checks for many common
problems.

Если Вы думаете, что нашли ошибку, или что-то ведет себя странно, Вы можете 
запустить команду `diagnose` для выполнения автоматизированных проверок на многие распространенные проблемы.

```sh
php composer.phar diagnose
```

## Команда archive

This command is used to generate a zip/tar archive for a given package in a
given version. It can also be used to archive your entire project without
excluded/ignored files.

Эта команда используется для создания zip/tar архивов для данного пакета в
данной версии. Он также может использоваться для архивации всего проекта без
игнорируемых/исключённых файлов.

```sh
php composer.phar archive vendor/package 2.0.21 --format=zip
```

### Options
### Параметры

* **--format (-f):** Format of the resulting archive: tar or zip (default:
  "tar")
* **--dir:** Write the archive to this directory (default: ".")


* **--format (-f):** Формат конечного архива: tar или zip (по умолчанию: "tar")
* **--dir:** Запись архива в каталог (по умолчанию: ".")

## Команда help

To get more information about a certain command, just use `help`.

Чтобы получить дополнительные сведения об определенной команде, просто используйте `help`.

```sh
php composer.phar help install
```

## Завершение командной строки (Command-line completion)

Command-line completion can be enabled by following instructions
[on this page](https://github.com/bamarni/symfony-console-autocomplete).

Завершение командной строки может быть включено, следуя инструкциям
[на этой странице](https://github.com/bamarni/symfony-console-autocomplete).

## Environment variables
## Переменные окружения

You can set a number of environment variables that override certain settings.
Whenever possible it is recommended to specify these settings in the `config`
section of `composer.json` instead. It is worth noting that the env vars will
always take precedence over the values specified in `composer.json`.

Можно установить несколько переменных окружения, которые переопределяют определенные параметры.
По возможности рекомендуется указывать эти параметры в файле `composer.json`
раздел `config`. Стоит отметить, что переменные окружения будут
всегда иметь приоритет над значениями, указанными в `composer.json`.

### COMPOSER

By setting the `COMPOSER` env variable it is possible to set the filename of
`composer.json` to something else.

По умолчанию переменная окружения `COMPOSER` разрешает установить вместо имени файла `composer.json` что-нибудь другое.

For example:
Например:

```sh
COMPOSER=composer-other.json php composer.phar install
```

The generated lock file will use the same name: `composer-other.lock` in this example.

Созданный файл блокировки будет использовать тоже имя: `composer-other.lock` в этом примере.

### COMPOSER_ROOT_VERSION

By setting this var you can specify the version of the root package, if it can
not be guessed from VCS info and is not present in `composer.json`.

Установив этот переменную можно указать версию корня пакета, если VCS информация не верна и не присутствует в `composer.json`.

### COMPOSER_VENDOR_DIR

By setting this var you can make Composer install the dependencies into a
directory other than `vendor`.

Установив эту переменную, Composer будет устанавливать зависимости в
каталог, отличный от `vendor`.

### COMPOSER_BIN_DIR

By setting this option you can change the `bin` ([Vendor Binaries](articles/vendor-binaries.md))
directory to something other than `vendor/bin`.

Установив этот параметр можно изменить каталог `bin` ([Vendor Binaries](articles/vendor-binaries.md)) на что-то другое отличное от `vendor/bin`

### http_proxy или HTTP_PROXY

If you are using Composer from behind an HTTP proxy, you can use the standard
`http_proxy` or `HTTP_PROXY` env vars. Simply set it to the URL of your proxy.
Many operating systems already set this variable for you.

Если Вы используете Composer за HTTP прокси, Вы можете использовать стандартные переменные окружения `http_proxy` или `HTTP_PROXY`. Просто установите их на URL адрес вашего прокси-сервера. Многие операционные системы уже установили эту переменную для вас.

Using `http_proxy` (lowercased) or even defining both might be preferable since
some tools like git or curl will only use the lower-cased `http_proxy` version.
Alternatively you can also define the git proxy using
`git config --global http.proxy <proxy url>`.

Использовать `http_proxy` (строчные) или даже оба параметра может быть предпочтительно после некоторых инструментов таких как git или curl, он будет использовать только в нижнем регистре `http_proxy` параметр. В качестве альтернативы можно также определить прокси для git с помощью `git config --global http.proxy <proxy url>`.

### no_proxy

If you are behind a proxy and would like to disable it for certain domains, you
can use the `no_proxy` env var. Simply set it to a comma separated list of
domains the proxy should *not* be used for.

Если Вы находитесь за прокси сервером и хотели бы отключить его для некоторых доменов, Вы
можете использовать переменную окружения `no_proxy`. Просто установите ей список доменов разделенных запятыми, *не* использовать прокси для.

The env var accepts domains, IP addresses, and IP address blocks in CIDR
notation. You can restrict the filter to a particular port (e.g. `:80`). You
can also set it to `*` to ignore the proxy for all HTTP requests.

Переменная окружения принимает домены, IP адреса и IP адреса блоков в CIDR
нотации. Вы можете ограничить фильтр к конкретному порту (например `:80`). Вы 
также можете установить `*` - игнорировать прокси для всех HTTP-запросов.

### HTTP_PROXY_REQUEST_FULLURI

If you use a proxy but it does not support the request_fulluri flag, then you
should set this env var to `false` or `0` to prevent Composer from setting the
request_fulluri option.

Если Вы используете прокси, а он не поддерживает флаг request_fulluri, то Вам
следует установить эту переменную окружения в `false` или `0` для предотвращения установки Composer из настроек
request_fulluri опции.

### HTTPS_PROXY_REQUEST_FULLURI

If you use a proxy but it does not support the request_fulluri flag for HTTPS
requests, then you should set this env var to `false` or `0` to prevent Composer
from setting the request_fulluri option.

Если Вы используете прокси, а он не поддерживает request_fulluri флаг для HTTPS
запросов, то Вы должны установить эту переменную окружения в `false` или `0` для предотвращения Composer
от параметра request_fulluri.

### COMPOSER_HOME

The `COMPOSER_HOME` var allows you to change the Composer home directory. This
is a hidden, global (per-user on the machine) directory that is shared between
all projects.

Переменная `COMPOSER_HOME` позволяет изменить домашний каталог Composer. Это
скрытый глобальный каталог (для пользователя на компьютере), который делится между
всеми проектами.

By default it points to `C:\Users\<user>\AppData\Roaming\Composer` on Windows
and `/Users/<user>/.composer` on OSX. On *nix systems that follow the [XDG Base
Directory Specifications](http://standards.freedesktop.org/basedir-spec/basedir-spec-latest.html),
it points to `$XDG_CONFIG_HOME/composer`. On other *nix systems, it points to
`/home/<user>/.composer`.

По умолчанию это `C:\Users\<user>\AppData\Roaming\Composer` для Windows,
для OSX это `/Users/<user>/.composer`. На *nix системах которые следуют спецификации
[XDG Base Directory Specifications](http://standards.freedesktop.org/basedir-spec/basedir-spec-latest.html)
это `$XDG_CONFIG_HOME/composer`. На других *nix система это `/home/<user>/.composer`.

#### COMPOSER_HOME/config.json

You may put a `config.json` file into the location which `COMPOSER_HOME` points
to. Composer will merge this configuration with your project's `composer.json`
when you run the `install` and `update` commands.

Вы можете положить файл `config.json` в расположение `COMPOSER_HOME`. Composer объединит эту конфигурацию с конфигурацией вашего проекта `composer.json` при запуске команд `install` и `update`.

This file allows you to set [repositories](05-repositories.md) and
[configuration](06-config.md) for the user's projects.

Этот файл позволяет установить [репозитории](05-repositories.md) и
[конфигурацию](06-config.md) для проектов пользователя.

In case global configuration matches _local_ configuration, the _local_
configuration in the project's `composer.json` always wins.

В случае глобальной конфигурации соответствующей конфигурации _local_, _local_
конфигурация проекта `composer.json` всегда выигрывает.

### COMPOSER_CACHE_DIR

The `COMPOSER_CACHE_DIR` var allows you to change the Composer cache directory,
which is also configurable via the [`cache-dir`](06-config.md#cache-dir) option.

Переменная `COMPOSER_CACHE_DIR` позволяет изменять каталог кэша Composer,
который также настраивается через параметр ['cache-dir'](06-config.md#cache-dir).

By default it points to $COMPOSER_HOME/cache on \*nix and OSX, and
`C:\Users\<user>\AppData\Local\Composer` (or `%LOCALAPPDATA%/Composer`) on Windows.

По умолчанию, он указывает на $COMPOSER_HOME/cache на \*nix и OSX, и на
`C:\Users\<user>\AppData\Local\Composer` (или `%LOCALAPPDATA%/Composer`) в Windows.

### COMPOSER_PROCESS_TIMEOUT

This env var controls the time Composer waits for commands (such as git
commands) to finish executing. The default value is 300 seconds (5 minutes).

Эта переменная окружения контролирует время ожидания команд (таких как команды git) для завершения выполнения. Значение по умолчанию составляет 300 секунд (5 минут).

### COMPOSER_CAFILE

By setting this environmental value, you can set a path to a certificate bundle
file to be used during SSL/TLS peer verification.

Установив это значение переменной, можно задать путь к комплекту файлов сертификатов, которые будут использоваться при одноранговой проверке SSL/TLS.

### COMPOSER_AUTH

The `COMPOSER_AUTH` var allows you to set up authentication as an environment variable.
The contents of the variable should be a JSON formatted object containing http-basic,
github-oauth, bitbucket-oauth, ... objects as needed, and following the
[spec from the config](06-config.md#gitlab-oauth).

Переменная `COMPOSER_AUTH` позволяет настроить проверку подлинности как переменную окружения.
Содержимое переменной должно быть отформатированный объект JSON содержащий http-basic,
github-oauth, bitbucket-oauth ... объекты, и следуют
[спецификации из конфигурации](06-config.md#gitlab-oauth).

### COMPOSER_DISCARD_CHANGES

This env var controls the [`discard-changes`](06-config.md#discard-changes) config option.

Эта переменная окружения управляет параметром конфигурации [`discard-changes`](06-config.md#discard-changes).

### COMPOSER_NO_INTERACTION

If set to 1, this env var will make Composer behave as if you passed the
`--no-interaction` flag to every command. This can be set on build boxes/CI.

Если установлено в 1, эта переменная окружения сделает поведение Composer как если бы передавался флаг
`--no-interaction` для каждой команды. Это может быть настроено при сборке boxes/CI.

### COMPOSER_DISABLE_XDEBUG_WARN

If set to 1, this env disables the warning about having xdebug enabled.

Если установлено в 1, эта переменная отключает предупреждение о том, что включен xdebug.

### COMPOSER_ALLOW_SUPERUSER

If set to 1, this env disables the warning about running commands as root/super user.
It also disables automatic clearing of sudo sessions, so you should really only set this
if you use Composer as super user at all times like in docker containers.

Если установлено в 1, эта переменная отключает предупреждение о запуске команды как root/super пользователь.
Она также отключает автоматическую очистку сессий sudo, поэтому Вы должны задавать эту переменную только тогда, когда
Вы действительно используете Composer как супер пользователь всё время как в docker контейнерах.

&larr; [Библиотеки](02-libraries.md)  |  [Схема](04-schema.md) &rarr;
