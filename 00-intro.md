# Introduction
# Введение

Composer is a tool for dependency management in PHP. It allows you to declare
the libraries your project depends on and it will manage (install/update) them
for you.

Composer является инструментом управления зависимостями в PHP. Позволяет объявить
библиотеки от которых зависит Ваш проект, и будет управлять (Установка/обновление) ими для Вас.

## Dependency management
## Управление зависимостями

Composer is **not** a package manager in the same sense as Yum or Apt are. Yes,
it deals with "packages" or libraries, but it manages them on a per-project
basis, installing them in a directory (e.g. `vendor`) inside your project. By
default it does not install anything globally. Thus, it is a dependency
manager. It does however support a "global" project for convenience via the
[global](03-cli.md#global) command.

Composer это **не** менеджер пакетов в том же смысле как Apt или Yum. Да,
он занимается "пакетами" или библиотеками, но он управляет ими на основе каждого проекта
, их установка в каталог (например `vendor`) внутри вашего проекта. По
умолчанию он не устанавливает что-либо глобально. Таким образом он является менеджером зависимостей.
Однако он поддерживает проект "global" (глобальный) для удобства, через
[global](03-cli.md#global) команды.

This idea is not new and Composer is strongly inspired by node's
[npm](https://npmjs.org/) and ruby's [bundler](http://bundler.io/).

Эта идея не нова и Composer сильно вдохновлён Node
[npm](https://npmjs.org/) и Ruby [bundler](http://bundler.io/).

Suppose:
Предположим, что:

1. You have a project that depends on a number of libraries.
1. Some of those libraries depend on other libraries.

1. У Вас есть проект, который зависит от некоторого числа библиотек.
1. И некоторые из этих библиотек зависит от других библиотек.

Composer:

1. Enables you to declare the libraries you depend on.
1. Finds out which versions of which packages can and need to be installed, and
   installs them (meaning it downloads them into your project).
   
1. Позволяет объявлять библиотеки, от которых зависит Ваш проект.
1. Выясняет, какие версии каких пакетов можно и необходимо установить, и 
устанавливает их (то есть он загружает их в Ваш проект).

See the [Basic usage](01-basic-usage.md) chapter for more details on declaring
dependencies.

Смотрите раздел [Базовое использование](01-basic-usage.md) для более подробной информации о декларировании
зависимостей.

## System Requirements
## Системные требования

Composer requires PHP 5.3.2+ to run. A few sensitive php settings and compile
flags are also required, but when using the installer you will be warned about
any incompatibilities.

Composer требует для запуска PHP 5.3.2+. Немного чувствителен к свойствам php, компиляционным
флагам, а также зависимостям, но при использовании установщика Вы будете предупреждены о
любых несовместимостях.

To install packages from sources instead of simple zip archives, you will need
git, svn, fossil or hg depending on how the package is version-controlled.

Чтобы установить пакеты из источников вместо простых zip архивов, Вам понадобится
git, svn, fossil или hg в зависимости от того, какой пакет в какой системе управления версиями находится.

Composer is multi-platform and we strive to make it run equally well on Windows,
Linux and OSX.

Composer — мультиплатформенный и мы стремимся сделать его выполнение одинаково хорошим на Windows,
Linux и OSX.

## Installation - Linux / Unix / OSX
## Установка - Linux / Unix / OSX

### Downloading the Composer Executable
### Загрузка исполняемого файла Composer

Composer offers a convenient installer that you can execute directly from the
commandline. Feel free to [download this file](https://getcomposer.org/installer)
or review it on [GitHub](https://github.com/composer/getcomposer.org/blob/master/web/installer)
if you wish to know more about the inner workings of the installer. The source
is plain PHP.

Composer предлагает удобный установщик, который можно выполнять непосредственно из
командной строки. Не стесняйтесь [загрузить этот файл](https://getcomposer.org/installer)
или просмотреть его на [GitHub](https://github.com/composer/getcomposer.org/blob/master/web/installer)
Если Вы хотите узнать больше о внутренней работе установщика. Исходнник это простой PHP.

There are in short, two ways to install Composer. Locally as part of your
project, or globally as a system wide executable.

В кратце есть два пути установки Compose. Локально как часть вашего
проекта, или глобально как исполняемый файл системы.

#### Locally
#### Локальная установка

Installing Composer locally is a matter of just running the installer in your
project directory. See [the Download page](https://getcomposer.org/download/)
for instructions.

Установка Composer локально является просто запуском установщика в каталоге вашего проекта.
Сморите [Страница Загрузки](https://getcomposer.org/download/) для получения инструкций.

The installer will just check a few PHP settings and then download
`composer.phar` to your working directory. This file is the Composer binary. It
is a PHAR (PHP archive), which is an archive format for PHP which can be run on
the command line, amongst other things.

Программа установки будет просто проверять несколько PHP параметров и затем скачает
`composer.phar` в рабочий каталог. Этот файл является двоичным. Это PHAR (архив PHP),
который является форматом архива для PHP, который можно запускать в командной строке, среди всего другого.

Now just run `php composer.phar` in order to run Composer.

Теперь просто запустите `php composer.phar` для того чтобы запустить Composer.

You can install Composer to a specific directory by using the `--install-dir`
option and additionally (re)name it as well using the `--filename` option. When
running the installer when following
[the Download page instructions](https://getcomposer.org/download/) add the
following parameters:

Вы можете установить Composer в определенный каталог, используя опцию `--install-dir`
и кроме того переименовать его с помощью опции `--filename`.
При запуске программы установки, следуя [Инструкциям страницы загрузки](https://getcomposer.org/download/) добавьте следующие параметры:

```sh
php composer-setup.php --install-dir=bin --filename=composer
```

Now just run `php bin/composer` in order to run Composer.

Теперь просто запустите `php bin/composer` для того чтобы запустить Composer.

#### Globally
#### Глобальная установка

You can place the Composer PHAR anywhere you wish. If you put it in a directory
that is part of your `PATH`, you can access it globally. On unixy systems you
can even make it executable and invoke it without directly using the `php`
interpreter.

Вы можете разместить  Composer PHAR везде, где пожелаете.
Если Вы поместите его в каталог, который является частью Вашего `PATH`, Вы можете получить глобальный доступ. На Unix системах Вы можете даже сделать его исполняемым и ссылаться на него напрямую, не используя интерпретатор `php`.

After running the installer following [the Download page instructions](https://getcomposer.org/download/)
you can run this to move composer.phar to a directory that is in your path:

После запуска установщика следуя [Инструкциям страницы загрузки](https://getcomposer.org/download/)
можно переместить composer.phar в каталог, который находится в вашей директории:

```sh
mv composer.phar /usr/local/bin/composer
```

> **Note:** If the above fails due to permissions, you may need to run it again
> with sudo.

> **Note:** On some versions of OSX the `/usr` directory does not exist by
> default. If you receive the error "/usr/local/bin/composer: No such file or
> directory" then you must create the directory manually before proceeding:
> `mkdir -p /usr/local/bin`.

> **Note:** For information on changing your PATH, please read the
> [Wikipedia article](https://en.wikipedia.org/wiki/PATH_(variable)) and/or use Google.

Now just run `composer` in order to run Composer instead of `php composer.phar`.

## Installation - Windows

### Using the Installer

This is the easiest way to get Composer set up on your machine.

Download and run
[Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe). It will
install the latest Composer version and set up your PATH so that you can just
call `composer` from any directory in your command line.

> **Note:** Close your current terminal. Test usage with a new terminal: This is
> important since the PATH only gets loaded when the terminal starts.

### Manual Installation

Change to a directory on your `PATH` and run the installer following
[the Download page instructions](https://getcomposer.org/download/)
to download `composer.phar`.

Create a new `composer.bat` file alongside `composer.phar`:

```sh
C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat
```

Add the directory to your PATH environment variable if it isn't already.
For information on changing your PATH variable, please see
[this article](http://www.computerhope.com/issues/ch000549.htm) and/or
use Google.

Close your current terminal. Test usage with a new terminal:

```sh
C:\Users\username>composer -V
Composer version 1.0.0 2016-01-10 20:34:53
```

## Using Composer

Now that you've installed Composer, you are ready to use it! Head on over to the
next chapter for a short and simple demonstration.

[Basic usage](01-basic-usage.md) &rarr;
