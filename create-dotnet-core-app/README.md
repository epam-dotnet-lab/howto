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
$ git status
On branch adding-application-skeleton
nothing to commit, working tree clean
```

## Добавление анализаторов кода

Статические анализаторы кода используются для автоматической проверки исходного текста приложений с целью выявления ошибок и недочетов, которые могут ухудшить свойства как самого исходного кода, так и стать причиной серьезных неполадок в работе системы в процессе ее эксплуатации. Более подробно см. [Статический анализ кода](https://www.viva64.com/ru/t/0046/).

### FxCopAnalyzers

[При установке пакета FxCopAnalyzers](https://docs.microsoft.com/en-us/visualstudio/code-quality/install-fxcop-analyzers#nuget-package) происходит добавление целой группы [анализаторов Roslyn](https://github.com/dotnet/roslyn-analyzers), которые проверяют код на стилистические ошибки, выявляют проблемы с дизайном, качеством и поддерживаемостью кода. См. [Roslyn Analyzers. Как писать код быстро и безошибочно](https://habr.com/ru/company/microsoft/blog/459982/), а также документацию - [Source Code Analysis](https://docs.microsoft.com/en-us/visualstudio/code-quality/roslyn-analyzers-overview).

12. Добавьте пакет [Microsoft.CodeAnalysis.FxCopAnalyzers](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers) в проект _MyConsoleApp_.

```sh
$ dotnet add MyConsoleApp\MyConsoleApp.csproj package Microsoft.CodeAnalysis.FxCopAnalyzers
info : Adding PackageReference for package 'Microsoft.CodeAnalysis.FxCopAnalyzers' into project 'MyConsoleApp\MyConsoleApp.csproj'.
info : Restoring packages for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj...
...
info : Installing Microsoft.CodeQuality.Analyzers 2.9.8.
info : Installing Microsoft.NetCore.Analyzers 2.9.8.
info : Installing Microsoft.NetFramework.Analyzers 2.9.8.
info : Installing Microsoft.CodeAnalysis.VersionCheckAnalyzer 2.9.8.
info : Installing Microsoft.CodeAnalysis.FxCopAnalyzers 2.9.8.
info : Package 'Microsoft.CodeAnalysis.FxCopAnalyzers' is compatible with all the specified frameworks in project 'MyConsoleApp\MyConsoleApp.csproj'.
info : PackageReference for package 'Microsoft.CodeAnalysis.FxCopAnalyzers' version '2.9.8' added to file 'd:\MyProject\MyConsoleApp\MyConsoleApp.csproj'.
info : Committing restore...
info : Generating MSBuild file d:\MyProject\MyConsoleApp\obj\MyConsoleApp.csproj.nuget.g.props.
info : Writing assets file to disk. Path: d:\MyProject\MyConsoleApp\obj\project.assets.json
log  : Restore completed in 11.69 sec for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
```

Анализаторы Roslyn также могут быть установлены как [расширение для Visual Studio](https://docs.microsoft.com/en-us/visualstudio/code-quality/install-roslyn-analyzers?view=vs-2019#to-install-vsix-analyzers) - см. расширение [Microsoft Code Analysis 2019](https://marketplace.visualstudio.com/items?itemName=VisualStudioPlatformTeam.MicrosoftCodeAnalysis2019) в Visual Studio Marketplace.

13. Соберите проект.

```sh
λ dotnet build MySolution.sln
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 33.56 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
Program.cs(9,31): warning CA1303: Method 'void Program.Main(string[] args)' passes a literal string as parameter 'value' of a call to 'void Console.WriteLine(string value)'. Retrieve the following string(s) from a resource table instead: "Hello World!". [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(7,35): warning CA1801: Parameter args of method Main is never used. Remove the parameter or use it in the method body. [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
  MyConsoleApp -> d:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
```

Компилятор выдает 2 ошибки с кодом CA ("Code Analysis") - [CA1303](https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1303) и [CA1801](https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1801). Изучите описание ошибок.

14. Правилами можно управлять с помощью атрибута [SuppressMessage](https://docs.microsoft.com/en-us/visualstudio/code-quality/in-source-suppression-overview). Примените атрибут к методу _Main_ и соберите проект.

```cs
[method:System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Design", "CA1303", Justification = "Should be turned off.")]
[method:System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Design", "CA1801", Justification = "Code that is using args will be added later.")]
static void Main(string[] args) { }
```

```sh
$ dotnet build --no-incremental
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 35.79 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
  MyConsoleApp -> d:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.00
```

Компилятор предупреждений не выводит.

15. Зафиксируйте изменения.

```sh
$ git status
On branch adding-application-skeleton
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   MyConsoleApp/MyConsoleApp.csproj
        modified:   MyConsoleApp/Program.cs

$ git diff
...

$ git add *.cs *.csproj

$ git status
On branch adding-application-skeleton
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   MyConsoleApp/MyConsoleApp.csproj
        modified:   MyConsoleApp/Program.cs

$ git diff --staged
...

$ git commit -m "Add FxCopAnalyzers package. Suppress warnings in Program.cs."

$ git status
...

$ git log --oneline
c7e5d78 (HEAD -> adding-application-skeleton) Add FxCopAnalyzers package. Suppress warnings in Program.cs.
ab59d60 Add MyConsoleApp project and MySolution solution.
d90c94f (origin/master, origin/HEAD, master) Initial commit
```