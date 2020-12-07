---
title: .NET 中的代码分析
titleSuffix: ''
description: 了解 .NET SDK 中的源代码分析。
ms.date: 08/22/2020
ms.topic: overview
ms.custom: updateeachrelease
helpviewer_keywords:
- code analysis
- code analyzers
ms.openlocfilehash: 8efac4d5e3fddcb9fdc6e08bcc933f2776420ced
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739969"
---
# <a name="overview-of-net-source-code-analysis"></a>.NET 源代码分析概述

.NET Compiler Platform (Roslyn) 分析器会检查 C# 或 Visual Basic 代码的代码质量和代码样式问题。 从 .NET 5.0 开始，这些分析器包含在 .NET SDK 中。 如果不想移到 .NET 5 + SDK，或者想要使用基于 NuGet 包的模型，则可以在 `Microsoft.CodeAnalysis.NetAnalyzers` [nuget 包](https://www.nuget.org/packages/Microsoft.CodeAnalysis.NetAnalyzers)中使用该分析器。 对于按需版本更新，你可能更倾向于使用基于包的模型。

> [!NOTE]
> .NET 分析器与目标平台无关。 也就是说，你的项目不需要面向特定的 .NET 平台。 分析器适用于目标为的项目以及 `net5.0` 更早的 .net 版本，如 `netcoreapp` 、 `netstandard` 和 `net472` 。

- [ ( "CAxxxx" 规则的代码质量分析) ](#code-quality-analysis)
- [ ( "IDExxxx" 规则的代码样式分析) ](#code-style-analysis)

如果分析器发现规则冲突，则会将其报告为建议、警告或错误，具体情况视每个规则的 [配置](configuration-options.md)方式而定。 代码分析冲突以前缀 "CA" 或 "IDE" 显示，以便将它们与编译器错误区分开来。

> [!TIP]
>
> - 你还可以安装第三方分析器，如 [StyleCop](https://www.nuget.org/packages/StyleCop.Analyzers/)、 [Roslynator](https://www.nuget.org/packages/Roslynator.Analyzers/)、 [XUnit 分析器](https://www.nuget.org/packages/xunit.analyzers/)和 [sonar.projectname Analyzer](https://www.nuget.org/packages/SonarAnalyzer.CSharp/)。
> - 如果你使用的是 Visual Studio，许多分析器规则都有关联的代码修复程序，你可以应用这些 *修补程序* 来纠正此问题。 代码修复显示在灯泡图标菜单中。

## <a name="code-quality-analysis"></a>代码质量分析

_代码质量分析 ( "CA" ) 规则_ 检查您的 c # 或 Visual Basic 代码以获得安全性、性能、设计和其他问题。 默认情况下，对于面向 .NET 5.0 或更高版本的项目，分析处于启用状态。 通过将 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性设置为，可以对面向早期 .net 版本的项目启用代码分析 `true` 。 你还可以通过将设置为来对你的项目禁用代码分析 `EnableNETAnalyzers` `false` 。

> [!TIP]
> 在 Visual Studio 中，你可以使用项目属性窗口来启用或禁用代码分析。 若要属性窗口访问项目，请在解决方案资源管理器中右键单击项目，然后选择 " **属性**"。 接下来，选择 " **代码分析** " 选项卡，然后选中或清除相应的复选框以 **启用 .net 分析器**。

### <a name="enabled-rules"></a>启用规则

默认情况下，在 .NET 5.0 Preview 8 中启用以下规则。

| 诊断 ID | 类别 | 严重性 | 说明 |
| - | - | - | - |
| [CA1416](/visualstudio/code-quality/ca1416) | 互操作性 | 警告 | 平台兼容性分析器 |
| [CA1417](/visualstudio/code-quality/ca1417) | 互操作性 | 警告 | 不要 `OutAttribute` 在 P/invoke 的字符串参数上使用 |
| [CA1831](/visualstudio/code-quality/ca1831) | 性能 | 警告 | `AsSpan`适当时使用字符串而不是基于范围的索引器 |
| [CA2013](/visualstudio/code-quality/ca2013) | 可靠性 | 警告 | 不要将 `ReferenceEquals` 与值类型一起使用 |
| [CA2014](/visualstudio/code-quality/ca2014) | 可靠性 | 警告 | 不要 `stackalloc` 在循环中使用 |
| [CA2015](/visualstudio/code-quality/ca2015) | 可靠性 | 警告 | 不要定义派生自的类型的终结器 <xref:System.Buffers.MemoryManager%601> |
| [CA2200](/visualstudio/code-quality/ca2200) | 使用情况 | 警告 | 再次引发以保留堆栈详细信息
| [CA2247](/visualstudio/code-quality/ca2247) | 使用情况 | 警告 | 传递给 TaskCompletionSource 构造函数的参数应为 <xref:System.Threading.Tasks.TaskCreationOptions> 枚举，而不是 <xref:System.Threading.Tasks.TaskContinuationOptions> |

你可以更改这些规则的严重性，以禁用这些规则或将它们提升为错误。

有关可用代码质量规则的完整列表，请参阅 [代码质量规则](quality-rules/index.md)。

### <a name="enable-additional-rules"></a>启用其他规则

从 .NET 5.0 RC2 开始，.NET SDK 附带了所有[“CA”代码质量规则](/visualstudio/code-quality/code-analysis-for-managed-code-warnings)。 有关每个 .NET SDK 版本附带的规则的完整列表，请参阅 [分析器版本](https://github.com/dotnet/roslyn-analyzers/blob/master/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)。

#### <a name="default-analysis-mode"></a>默认分析模式

在默认的分析模式下，某些规则 [默认启用](#enabled-rules) 为生成警告。 某些其他规则默认情况下启用为 Visual Studio IDE 建议和相应的代码修复。 默认情况下，将禁用其余规则。 [规则的完整列表](https://github.com/dotnet/roslyn-analyzers/blob/master/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)包括每个规则的默认严重性，以及默认情况下是否在默认分析模式下启用规则。

#### <a name="custom-analysis-mode"></a>自定义分析模式

您可以 [配置代码分析规则](configuration-options.md) ，以启用或禁用单个规则或规则类别。 此外，还可以使用 [AnalysisMode](../../core/project-sdk/msbuild-props.md#analysismode) 属性切换到以下自定义分析模式之一：

- _积极_ 或 _退出_ 模式：默认情况下，所有规则都作为生成警告启用。 可以选择[选择退出](configuration-options.md)各条规则，以禁用它们。
- _保守_ 或 _选择加入_ 模式：默认情况下禁用所有规则。 可以选择[选择加入](configuration-options.md)各条规则，以启用它们。

### <a name="treat-warnings-as-errors"></a>视警告为错误

如果在 `-warnaserror` 生成项目时使用标志，则所有代码分析警告也被视为错误。 如果你不希望将代码质量警告 (CAxxxx) 在存在时被视为错误 `-warnaserror` ，可以 `CodeAnalysisTreatWarningsAsErrors` `false` 在项目文件中将 MSBuild 属性设置为。

```xml
<PropertyGroup>
  <CodeAnalysisTreatWarningsAsErrors>false</CodeAnalysisTreatWarningsAsErrors>
</PropertyGroup>
```

你仍会看到任何代码分析警告，但不会中断你的生成。

### <a name="latest-updates"></a>最新更新

默认情况下，升级到较新版本的 .NET SDK 时，你将获得最新的代码分析规则和默认规则严重性。 如果不想要此行为，例如，如果想要确保没有启用或禁用任何新规则，可以通过以下方式之一来替代它：

- 将 `AnalysisLevel` MSBuild 属性设置为一个特定值，以将警告锁定到该设置。 升级到较新的 SDK 时，仍然会获得这些警告的 bug 修复，但不会启用新的警告，并且不会禁用现有的警告。 例如，若要将规则集锁定为版本5.0 的 .NET SDK 随附的规则集，请将以下条目添加到项目文件。

  ```xml
  <PropertyGroup>
    <AnalysisLevel>5.0</AnalysisLevel>
  </PropertyGroup>
  ```

  > [!TIP]
  > 属性的默认值 `AnalysisLevel` 为 `latest` ，这意味着，当你移动到较新版本的 .net SDK 时，始终会获得最新的代码分析规则。

  有关详细信息，以及要查看可能值的列表，请参阅 [AnalysisLevel](../../core/project-sdk/msbuild-props.md#analysislevel)。

- 安装 [CodeAnalysis NetAnalyzers NuGet 包](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) 以将规则更新与 .net SDK 更新分离。 如果 SDK 包含的分析器程序集版本比 NuGet 包的版本新，则安装包将关闭内置 SDK 分析器并生成生成警告。

## <a name="code-style-analysis"></a>代码样式分析

[代码样式分析](/visualstudio/ide/editorconfig-code-style-settings-reference) ( "IDExxxx" 规则) 使你可以在基本代码中定义和维护一致的代码样式。 下面是默认设置：

- 命令行生成：默认情况下，对于命令行生成上的所有 .NET 项目，代码样式分析均处于禁用状态。
- Visual studio：默认情况下，对于 Visual Studio 中的所有 .NET 项目，代码样式分析均为 [代码重构快速操作](/visualstudio/ide/code-generation-in-visual-studio)启用。

启动 .NET 5.0 RC2 后，可以在命令行和 Visual Studio 内部启用生成代码样式分析。 代码样式冲突显示为带有 "IDE" 前缀的警告或错误。 这使您能够在生成时强制执行一致的代码样式。

> [!NOTE]
> 代码样式分析功能是实验性的，可能会在 .NET 5 和 .NET 6 版本之间发生更改。

对生成启用代码样式分析的步骤：

1. 将 MSBuild 属性 [EnforceCodeStyleInBuild](../../core/project-sdk/msbuild-props.md#enforcecodestyleinbuild) 设置为 `true` 。

1. 在 *editorconfig* 文件中， [配置](configuration-options.md) 希望在生成时运行的每个 "IDE" 代码样式规则（警告或错误）。 例如：

   ```ini
   [*.{cs,vb}]
   # IDE0040: Accessibility modifiers required (escalated to a build warning)
   dotnet_diagnostic.IDE0040.severity = warning
   ```

   或者，您也可以将整个 "样式" 类别配置为警告或错误，默认情况下，您可以选择关闭您不想在生成时运行的规则。 例如：

   ```ini
   [*.{cs,vb}]

   # Default severity for analyzer diagnostics with category 'Style' (escalated to build warnings)
   dotnet_analyzer_diagnostic.category-Style.severity = warning

   # IDE0040: Accessibility modifiers required (disabled on build)
   dotnet_diagnostic.IDE0040.severity = silent
   ```

## <a name="suppress-a-warning"></a>禁止显示警告

若要取消规则冲突，请 `none` 在 EditorConfig 文件中将该规则 ID 的严重性选项设置为。 例如：

```ini
dotnet_diagnostic.CA1822.severity = none
```

Visual Studio 提供了其他方式来禁止显示代码分析规则中的警告。 有关详细信息，请参阅 [取消冲突](/visualstudio/code-quality/use-roslyn-analyzers#suppress-violations)。

有关规则严重性的详细信息，请参阅 [配置规则严重性](configuration-options.md#severity-level)。

## <a name="see-also"></a>请参阅

- [代码质量分析规则参考](quality-rules/index.md)
- [代码样式分析规则引用](style-rules/index.md)
- [Visual Studio 中的代码分析](/visualstudio/code-quality/roslyn-analyzers-overview)
- [.NET 编译器平台 SDK](../../csharp/roslyn-sdk/index.md)
- [教程：编写第一个分析器和代码修补程序](../../csharp/roslyn-sdk/tutorials/how-to-write-csharp-analyzer-code-fix.md)