# How To: Создание нового приложения .NET Core

## Создание репозитория для приложения

1. [Создайте](https://github.com/new) новый **private** репозиторий. При создании репозитория укажите настройку для файла .gitignore - VisualStudio.
2. Клонируйте репозиторий на локальный диск.

```sh
$ git clone https://github.com/my-github-account/my-project MyProject
```

3. Создайте в клонированном репозитории новую ветку _adding-application-skeleton_ и переключитесь на нее.

```sh
$ git checkout -b adding-application-skeleton
```

### Особенности Git Bash для Windows

Системы Windows используют "backslash" в файловых путях, а системы Linux и Mac - "slash". Поэтому для корректной работы командной строки в cmd.exe нужно использовать backslash. Однако, при работе в Windows через [Bash Shell (mingw64)](https://www.logicbig.com/tutorials/misc/git/git-bash-shell.html) нужно использовать "slash".

Например, правильный вызов команды для сборки проекта с применением "slash":

```sh
username@HOSTNAME MINGW64 /d/MyProject (adding-application-skeleton)
$ dotnet build MyConsoleApp/MyConsoleApp.csproj
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 24.29 ms for D:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
  FileCabinetApp -> D:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)
```

Использование "backslash" приведет к ошибке:

```sh
username@HOSTNAME MINGW64 /d/MyProject (adding-application-skeleton)
$ dotnet build MyConsoleApp\MyConsoleApp.csproj
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

MSBUILD : error MSB1009: Project file does not exist.
Switch: MyConsoleAppMyConsoleApp.csproj
```

## Создание консольного приложения

4. Создайте в каталоге клонированного репозитория новый проект .NET Core _MyConsoleApp_.

```sh
$ dotnet new console --name MyConsoleApp
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on MyConsoleApp\MyConsoleApp.csproj...
  Restore completed in 165.74 ms for D:\MyProject\MyConsoleApp\MyConsoleApp.csproj.

Restore succeeded.
```

5. Создайте новый solution _MySolution_ и добавьте в него проект _MyConsoleApp_.

```sh
$ dotnet new sln --name MySolution
The template "Solution File" was created successfully.

$ dotnet sln MySolution.sln add MyConsoleApp\MyConsoleApp.csproj
Project `MyConsoleApp\MyConsoleApp.csproj` added to the solution.
```

6. Заберите зависимости и соберите проект.

```sh
$ dotnet restore MySolution.sln
  Restore completed in 81.34 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.

$ dotnet build MySolution.sln
Microsoft (R) Build Engine version 16.2.32702+c4012a063 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 35.03 ms for D:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
  MyConsoleApp -> D:\MyProject\MyConsoleApp\bin\Debug\netcoreapp2.2\MyConsoleApp.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:02.46
```

7. Проверьте список измененных файлов в репозитории.

```sh
$ git status
On branch adding-application-skeleton
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        MySolution.sln
        MyConsoleApp/
```

8. Добавьте изменения в файлах в _stage_.

```sh
$ git add *.cs *.csproj *.sln

$ git status
On branch adding-application-skeleton
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   MySolution.sln
        new file:   MyConsoleApp/MyConsoleApp.csproj
        new file:   MyConsoleApp/Program.cs
```

9. Посмотрите на изменения в _stage_. 

```sh
$ git diff --staged
```

*Внимание* Всегда просматривайте ваши изменения перед тем как сделать commit!

10. Сделайте commit.

```sh
$ git commit -m "Add MyConsoleApp project and MySolution solution."
```

11. Проверьте, что _stage_ пуст.

```sh
git status
On branch adding-application-skeleton
nothing to commit, working tree clean
```