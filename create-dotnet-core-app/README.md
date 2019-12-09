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

*Внимание!* Всегда проверяйте ваши изменения перед тем как сделать commit!

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
$ dotnet build MySolution.sln
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
$ dotnet build
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 35.79 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
  MyConsoleApp -> d:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.00
```

Вывод предупреждений блокируется, так как для метода установлен атрибут SuppressMessage.

*Внимание!* Используйте SuppressMessage только для отключения вывода предупреждений и ошибок анализатора в конкретном месте текста приложения. Избегайте частого применения этого атрибута.

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

*Внимание!* Всегда проверяйте ваши изменения на разных стадиях - unstaged, staged и после коммита!

### Настройка анализатора с помощью набора правил

Изменять настройки анализаторов можно с помощью [набора правил](https://docs.microsoft.com/en-us/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules), который представляет собой файл формата XML с расширением _.ruleset_.

16. Создайте рядом с файлом MySolution.sln новый файл code-analysis.ruleset.

Содержимое файла:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RuleSet Name="Custom Rulset" Description="Custom Rulset" ToolsVersion="14.0">
    <Rules AnalyzerId="Microsoft.Analyzers.ManagedCodeAnalysis" RuleNamespace="Microsoft.Rules.Managed">
        <Rule Id="CA1303" Action="None" /><!-- Do not pass literals as localized parameters -->
        <Rule Id="CA1801" Action="Info" /><!-- Review unused parameters -->
    </Rules>
</RuleSet>
```

Возможные значения параметра Action см. [Use the code analysis rule set editor](https://docs.microsoft.com/en-us/visualstudio/code-quality/working-in-the-code-analysis-rule-set-editor).

17. Отредактируйте файл _MyConsoleApp.csproj_:
* Задайте относительный путь к файлу с помощью параметра [CodeAnalysisRuleSet](https://roslyn-analyzers.readthedocs.io/en/latest/config-analyzer.html#net-core-and-net-standard-projects).
* Добавьте в проект файл _code-analysis.ruleset_ как дополнительный.

```xml
<PropertyGroup>
    <CodeAnalysisRuleSet>..\code-analysis.ruleset</CodeAnalysisRuleSet>
</PropertyGroup>
<ItemGroup>
    <AdditionalFiles Include="..\code-analysis.ruleset" Link="Properties\code-analisys.ruleset" />
</ItemGroup>
```

18. Удалите атрибуты SuppressMessage.

19. Соберите проект.

```sh
$ dotnet build
```

Предупреждений компилятора быть не должно.

20. Зафиксируйте изменения.

```sh
$ git status
$ git diff
$ git add *.cs *.csproj *.ruleset
$ git status
$ git diff --staged
$ git commit -m "Add ruleset file and configure MyConsoleApp project to use the code-analisys.ruleset."
$ git status
$ git log --oneline
33d21ce (HEAD -> adding-application-skeleton) Add ruleset file and configure MyConsoleApp project to use the code-analisys.ruleset.
c7e5d78 Add FxCopAnalyzers package. Suppress warnings in Program.cs.
ab59d60 Add MyConsoleApp project and MySolution solution.
d90c94f (origin/master, origin/HEAD, master) Initial commit
```

### StyleCop

[Анализатор StyleCop](https://github.com/StyleCop/StyleCop) предназначен для анализа исходного кода с целью обеспечения стилистического единства. Анализатор существует в двух вариантах - nuget-пакет и VS-extension. См. [StyleCop: A Detailed Guide to Starting and Using It](https://blog.submain.com/stylecop-detailed-guide/).

21. Добавьте пакет [StyleCop](https://www.nuget.org/packages/StyleCop/) в проект _MyConsoleApp_.

```sh
$ dotnet add MyConsoleApp\MyConsoleApp.csproj package StyleCop.Analyzers
info : Adding PackageReference for package 'StyleCop.Analyzers' into project 'MyConsoleApp\MyConsoleApp.csproj'.
info : Restoring packages for d:\MyProject\MyTestRepository\MyConsoleApp\MyConsoleApp.csproj...
info :   GET https://api.nuget.org/v3-flatcontainer/stylecop.analyzers/index.json
info :   OK https://api.nuget.org/v3-flatcontainer/stylecop.analyzers/index.json 534ms
info : Package 'StyleCop.Analyzers' is compatible with all the specified frameworks in project 'MyConsoleApp\MyConsoleApp.csproj'.
info : PackageReference for package 'StyleCop.Analyzers' version '1.1.118' added to file 'd:\MyProject\MyConsoleApp\MyConsoleApp.csproj'.
info : Committing restore...
info : Generating MSBuild file d:\MyProject\MyConsoleApp\obj\MyConsoleApp.csproj.nuget.g.props.
info : Writing assets file to disk. Path: d:\MyProject\MyConsoleApp\obj\project.assets.json
log  : Restore completed in 1.34 sec for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
```

22. Соберите проект.

```sh
$ dotnet build
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 117.08 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
Program.cs(1,1): warning SA1200: Using directive should appear within a namespace declaration [d:\MyProject\MyTestRepository\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(5,11): warning SA1400: Element 'Program' should declare an access modifier [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(7,21): warning SA1400: Element 'Main' should declare an access modifier [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(1,1): warning SA1633: The file header is missing or not located at the top of the file. [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
CSC : warning SA0001: XML comment analysis is disabled due to project configuration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
  MyConsoleApp -> d:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
```

Прочитайте описание ошибок в документации - [SA1200](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1200.md), [SA1400](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1400.md), [SA1633](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1633.md).

23. Параметры правил StyleCop можно настроить в файле правил. Добавьте в _code-analysis.ruleset_:

```xml
<Rules AnalyzerId="StyleCop.Analyzers" RuleNamespace="StyleCop.Analyzers">
    <Rule Id="SA1200" Action="Error" /><!-- SA1200UsingDirectivesMustBePlacedCorrectly -->
    <Rule Id="SA1400" Action="Error" /><!-- SA1400AccessModifierMustBeDeclared -->
    <Rule Id="SA1633" Action="None" /><!-- SA1633FileMustHaveHeader -->
</Rules>
```

24. Соберите проект.

```sh
$ dotnet build --no-incremental
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 30.74 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
Program.cs(1,1): error SA1200: Using directive should appear within a namespace declaration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(5,11): error SA1400: Element 'Program' should declare an access modifier [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(7,21): error SA1400: Element 'Main' should declare an access modifier [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
CSC : warning SA0001: XML comment analysis is disabled due to project configuration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
```

Правила SA1200 и SA1400 теперь работают как ошибки, а предупреждение SA1633 блокируется компилятором.

25. Добавьте модификаторы доступа для класса Program и метода Main(). Соберите проект.

```cs
public class Program {
    public static void Main(string[] args) { }
}
```

```sh
$ dotnet build --no-incremental
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 27.17 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
Program.cs(5,18): warning CA1052: Type 'Program' is a static holder type but is neither static nor NotInheritable [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(1,1): error SA1200: Using directive should appear within a namespace declaration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
CSC : warning SA0001: XML comment analysis is disabled due to project configuration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]

Build FAILED.
```

Сработал анализатор Roslyn, компилятор выдает ошибку - [CA1052](https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1052).

26. Добавьте правило в _code-analysis.ruleset_ для анализатора _Microsoft.Analyzers.ManagedCodeAnalysis_. Соберите проект.

```xml
<Rule Id="CA1052" Action="Error" /><!-- Static holder types should be Static or NotInheritable -->
```

```sh
$ dotnet build --no-incremental
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 27.17 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
Program.cs(5,18): error CA1052: Type 'Program' is a static holder type but is neither static nor NotInheritable [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(1,1): error SA1200: Using directive should appear within a namespace declaration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
CSC : warning SA0001: XML comment analysis is disabled due to project configuration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]

Build FAILED.
```

Компилятор выдает ошибку вместо предупреждения.

27. Исправьте ошибку - добавьте модификатор _static_ для класса _Program_.

```cs
public static class Program { }
```

```sh
$ dotnet build --no-incremental
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 26.7 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
Program.cs(1,1): error SA1200: Using directive should appear within a namespace declaration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
CSC : warning SA0001: XML comment analysis is disabled due to project configuration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]

Build FAILED.
```

Компилятор больше не выдает ошибку CA1052.

28. Кроме файла с набором правил, StyleCop использует настройки из файла [stylecop.json](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/Configuration.md). Создайте новый файл _stylecop.json_ в каталоге, где находится _MySolution.sln_, добавьте содержимое:

```json
{
  "$schema": "https://raw.githubusercontent.com/DotNetAnalyzers/StyleCopAnalyzers/master/StyleCop.Analyzers/StyleCop.Analyzers/Settings/stylecop.schema.json",
  "settings": {
    "orderingRules": {
      "systemUsingDirectivesFirst": true,
      "usingDirectivesPlacement": "outsideNamespace"
    }
  }
}
```

Добавьте файл _stylecop.json_ в MyConsoleApp.csproj как дополнительный:

```xml
<ItemGroup>
  <AdditionalFiles Include="..\code-analysis.ruleset" Link="Properties\code-analisys.ruleset" />
  <AdditionalFiles Include="..\stylecop.json" Link="Properties\stylecop.json" />
</ItemGroup>
```

29. Соберите проект.

```sh
$ dotnet build
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 24.38 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
CSC : warning SA0001: XML comment analysis is disabled due to project configuration [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
  MyConsoleApp -> d:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
```

Компилятор больше не выдает ошибку SA1200, так как использует настройку из _stylecop.json_, которая задает местоположение директив using.

30. Включите генерацию документации в файле проекта _MyConsoleApp.csproj_ (GenerateDocumentationFile) и [отключите некоторые предупреждения компилятора](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA0001.md) (NoWarn).

```csproj
<PropertyGroup>
  <CodeAnalysisRuleSet>..\code-analysis.ruleset</CodeAnalysisRuleSet>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <NoWarn>$(NoWarn),1573,1591,1712</NoWarn>
</PropertyGroup>
```

Соберите проект.

```sh
$ dotnet build --no-incremental
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 23.22 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
Program.cs(5,25): warning SA1600: Elements should be documented [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
Program.cs(7,28): warning SA1600: Elements should be documented [d:\MyProject\MyConsoleApp\MyConsoleApp.csproj]
  MyConsoleApp -> d:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
```

31. Добавьте XML-документацию для класса _Program_ и метода _Main_. Соберите проект.

```cs
/// <summary>
/// An application entry point.
/// </summary>
public static class Program
{
    /// <summary>
    /// Starts an application.
    /// </summary>
    /// <param name="args">Application arguments.</param>
    public static void Main(string[] args) { }
}
```

```sh
$ dotnet build
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 25.4 ms for d:\MyProject\MyConsoleApp\MyConsoleApp.csproj.
  MyConsoleApp -> d:\MyProject\MyConsoleApp\bin\Debug\netcoreapp3.0\MyConsoleApp.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.36
```

Компилятор больше не выдает ошибок и предупреждений.

32. Зафиксируйте изменения.

```sh
$ git status
$ git diff
$ git add *.cs *.csproj *.ruleset *.json
$ git status
$ git diff --staged
$ git commit -m "Add StyleCop and enable documentation generation."
$ git status
$ git log --oneline
36b66a0 (HEAD -> adding-application-skeleton) Add StyleCop and enable documentation generation.
33d21ce Add ruleset file and configure MyConsoleApp project to use the code-analisys.ruleset.
c7e5d78 Add FxCopAnalyzers package. Suppress warnings in Program.cs.
ab59d60 Add MyConsoleApp project and MySolution solution.
d90c94f (origin/master, origin/HEAD, master) Initial commit
```

## Файл настроект .editorconfig

Файл _.editorconfig_ используется для хранения настроек стиля исходного текста приложений. См. [Editorconfig для Visual Studio 2017](https://blog.zwezdin.com/2017/vs-2017-editorconfig/) и [Editorconfig](https://editorconfig.org/).

33. Создайте новый файл _.editorconfig_ в каталоге, в котором находится _MySolution.sln_, и скопируйте в него пример [Example EditorConfig file](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference#example-editorconfig-file).

34. Зафиксируйте изменения.

```sh
$ git status
$ git add .editorconfig
$ git status
$ git diff --staged
$ git commit -m "Add .editorconfig file."
$ git status
$ git log --oneline
1c17838 (HEAD -> adding-application-skeleton) Add .editorconfig file.
36b66a0 Add StyleCop and enable documentation generation.
33d21ce Add ruleset file and configure MyConsoleApp project to use the code-analisys.ruleset.
c7e5d78 Add FxCopAnalyzers package. Suppress warnings in Program.cs.
ab59d60 Add MyConsoleApp project and MySolution solution.
d90c94f (origin/master, origin/HEAD, master) Initial commit
```

### Visual Studio

Visual Studio поддерживает файл настроек .editorconfig и умеет обрабатывать большое количество параметров. См. [Code style preferences](https://docs.microsoft.com/en-us/visualstudio/ide/code-styles-and-code-cleanup) и [Create portable, custom editor settings with EditorConfig](https://docs.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options).

Также, вы можете выполнить лабораторную работу [Working with EditorConfig in Visual Studio 2019](https://www.azuredevopslabs.com/labs/devopsserver/editorconfig), чтобы лучше понять работу файла настроек.

### Visual Studio Code

По-умолчанию VS Code не умеет работать с _.editorconfig_. Расширение [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) добавляет поддержку некоторых параметров файла настроек.

## Файл .gitignore

В файл .gitignore помещаются маски файлов и пути, которые будут игнорироваться при работе команды _git add_. Бинарные файлы, которые являются продуктом работы компилятора, редко помещают под управление системы контроля версий.

Например, файл _.gitignore_, который был добавлен автоматически при создании репозитория отключает все файлы, которые находятся в каталогах, которые создает компилятор C#:

```
# Build results
[Dd]ebug/
[Dd]ebugPublic/
[Rr]elease/
[Rr]eleases/
x64/
x86/
[Aa][Rr][Mm]/
[Aa][Rr][Mm]64/
bld/
[Bb]in/
[Oo]bj/
[Ll]og/
[Ll]ogs/
```

Таким образом, файлы в каталогах _MyConsoleApp\bin_ и _MyConsoleApp\obj_ не будут добавлены под управление _Git_ при выполнении команды _git add_.

*Внимание!* Проверьте, что файл _.gitignore_ добавлен в корневой каталог репозитория и каталоги _bin_ и _obj_ не добавлены под управление _Git_.

Нет необходимости помещать под управление системы контроля версий файлы, которые являются продуктом работы IDE.

```
# User-specific files
*.rsuser
*.suo
*.user
*.userosscache
*.sln.docstates

# Visual Studio 2015/2017 cache/options directory
.vs/
```

Файлы в каталоге _.vs_ и с указанными расширенями должны игнорироваться при работе _Git_.

## Сохранение изменений в удаленный репозиторий

35. Сохраните все изменения рабочей ветки в удаленный репозиторий.

```sh
$ git push --set-upstream origin adding-application-skeleton
```

Проверьте, что в репозитории _Github_ появилась ветка с указанным названием.

36. Переместите все изменения из рабочей ветки в удаленную ветку _master_.

```sh
$ git checkout master
$ git merge adding-application-skeleton --ff
$ git push
```

## Настройка Continuous Integration

Continuous Integration - это практика разработки, при которой изменения часто помещаются в общий репозиторий. Сразу после этого должна быть запущена сборка проекта и запуск тестов (если есть).

### GitHub Actions

См. видео [How to get started with GitHub Actions for Azure](https://www.youtube.com/watch?v=zcYqejz6Iig), также см. документацию [Creating continuous integration workflows](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-continuous-integration-workflows).

Пример конфигурационного файла для сборки проекта .NET Core 3.0:

```yaml
name: .NET Core

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1
    - name: Setup .NET Core
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 3.0.100
    - name: Build with dotnet
      run: dotnet build --configuration Release
```

### Travis CI

См. документацию [Building a C#, F#, or Visual Basic Project](https://docs.travis-ci.com/user/languages/csharp/#net-core). Используйте сервис [travis-ci.com](https://travis-ci.com/) для запуска сборки проекта и наблюдения за результатом.

Пример конфигурационного файла .travis.yml (нужно положить в корневой каталог репозитория) для автоматической сборки проекта .NET Core 3.0:

```yaml
  
os:
  - linux
language: csharp
mono: none
solution: MySolution.sln
dotnet: 3.0.100
install:
  - dotnet restore
script:
  - dotnet build
```