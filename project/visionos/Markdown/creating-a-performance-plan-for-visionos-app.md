# 为你的 visionOS 应用创建性能计划

确定应用的性能和功耗目标，并制定计划来衡量和评估这些目标。

## 概述

无论在哪个平台，性能调优都是开发过程中的一个重要环节。性能调优意味着尽可能提高应用的运行效率，使其在更短的时间内利用更少的系统资源完成更多的工作。在可以支持多个沉浸式体验应用的设备上，效率尤为重要。消耗过多资源的应用会使设备超出散热限制。当出现这种情况时，系统会采取措施降温到更可接受的水平。这可能会产生明显的视觉影响，并使佩戴者感到迷惑。

在开始开发时，应设定积极的目标，并在整个开发周期中反复评估进度。尽可能地自动收集性能指标，并随时间推移查看数据，以了解性能是在提高还是在下降。一旦发现性能大幅下降，应立即采取措施加以纠正。如果在开发初期就开始微调，就会有更多时间对算法和处理方式进行必要的修改。

有关性能调优的更多信息，请参阅[提高应用的性能](https://developer.apple.com/documentation/Xcode/improving-your-app-s-performance)。

### 设定性能和功耗目标

性能并不是你衡量和改进的单一指标。通常情况下，你会选择多个指标，并为每个指标设定目标。例如你可以考虑：

应用启动和加载时间  
    确保你的应用快速启动；这是给用户留下良好印象的第一个机会。

响应速度和延迟  
    即使在进行其他工作时，你的界面也需要快速响应交互。尽量缩短启动任务所需的时间。例如，确保音频和视频启动时没有明显的延迟。

图形渲染  
    要通过实时渲染获得沉浸式的体验，保持稳定的高帧频非常重要。避免不必要的更改导致频繁地更新共享渲染服务器，从而帮助保持帧频。测量渲染服务器和应用的更新率、停滞和挂起等情况。只渲染需要的内容，并优化绘制过程中使用的纹理和其他资源。

使用电量  
    当设备开始达到散热极限时，系统会降低 CPU 或 GPU 的使用率，性能也会随之下降。避免到达散热上限的方法是：确定优先级并分散工作，限制应用同时运行的线程数，以及在不需要时关闭如 [Core Location](https://developer.apple.com/documentation/CoreLocation) 等与硬件相关的功能。

任务效率  
    让应用使用最少的硬件资源完成尽可能多的工作。尽量减少基于任务的开销。

内存占用和带宽  
    尽可能少使用可用内存。不要在关键操作过程中分配或取消分配内存，这可能会使应用显得很慢。

在选择了所需的指标后，应制定切实可行的目标并对其进行优先排序，以便了解哪些指标最重要。性能调优通常需要在相互竞争的目标之间进行权衡。例如，如果通过缓存计算数据或预载资源来降低 CPU 使用率并以提高响应速度，就会增加应用的内存使用量。请谨慎进行此类权衡，并始终衡量任何更改的结果，以了解它们是否成功。在某些情况下，你可能会发现得到的优化并不值得做出的割舍。

考虑用户将如何使用你的应用。如果你的应用在共享空间中运行，请考虑更为保守的系统资源目标和指标。如果你希望用户长时间使用你的应用，那么在选择指标时，请将这种长时间使用因素纳入你的目标中。

### 确定要测试的代码流和用户场景

选择要收集的指标后，再决定要测试应用的哪些部分。选择可重复、可测量和可靠的功能进行测试。可重复的自动测试可让你对结果进行比较，并知道比较结果代表的是完全相同的任务。重点关注应用执行代码的位置，但也不要忽略应用向系统发送数据和等待的位置。如果你的应用需要花费大量时间等待信息，请考虑完全取消请求或对请求进行批处理，以获得更好的性能。

将调优重点放在应用中用户使用最多或对系统整体性能影响最大的部分，这包括：

- 面向用户的工作流程
- 关键算法
- 分配或取消分配内存的任务
- 后台和基于网络的任务
- 自定义 Meta 着色器

选择用户经常执行的操作或与重要功能相对应的操作。例如，如果你的应用允许用户添加新的联系人，则应测试创建联系人、编辑联系人和保存结果的工作流程。在启用和禁用特定功能的情况下测试应用，以确定该功能是否是影响性能的唯一原因。选择轻量级工作流，例如应用在空闲时的性能，以及重量级工作流，例如涉及用户交互和应用响应的工作流。对于启动时间，收集热启动和冷启动的指标，即应用已驻留内存和未驻留内存时的指标。

### 考虑散热和环境因素

考虑环境因素对应用的影响。物理环境的特性会影响系统负载和设备热量。考虑环境室温、其他人的存在以及现实世界中物体的数量和类型对应用算法的影响。尝试在不同的设置下进行测试，以了解是否需要针对这些场景进行优化。

使用 Xcode 的热感应器来模拟设备达到其散热极限，并考虑你的应用如何响应一般、严重和临界的热通知。在热压力下，你可能需要有不同的性能目标，并优先优化功耗，或想办法动态降低应用的复杂性以应对热压力，从而提供更流畅的体验，即便延迟会稍高。

### 选择收集性能数据的工具

你可以使用许多工具和应用接口来为你的 visionOS 应用收集性能相关数据。使用各种工具以确保能获得所需数据：

调试仪表  
    监控调试导航器中的 CPU、内存、磁盘和网络仪表，以跟踪系统资源利用情况。

Instruments  
    对应用进行剖析，收集大多数指标的性能数据。通过 Instruments，你可以剖析应用的代码执行、查找内存泄漏、跟踪内存分配、分析文件系统或图形性能、SwiftUI 性能等。使用 RealityKit 跟踪模板来监控和调查 visionOS 上的呈现服务器停滞和瓶颈。

XCTest  
    使用 [XCTest](https://developer.apple.com/documentation/xctest) API 收集性能数据。

MetricKit  
    使用 [MetricKit](https://developer.apple.com/documentation/MetricKit) 收集设备上的应用诊断并生成报告。

Organizer  
    在 Xcode Organizer 中查看有关挂起、磁盘和能源使用以及崩溃的诊断日志。

Reality Composer Pro  
    查看 RealityKit 场景内容的统计信息。利用这些信息优化你的 3D 模型和纹理。

Signposts  
    将路标添加到代码中，以生成可在 Instruments 中查看的计时信息。有关详细信息，请参阅[记录性能数据](https://developer.apple.com/documentation/os/logging/recording_performance_data)。

日志信息  
    包含日志信息以报告重要事件和这些事件的相关数据。更多信息，请参阅[从代码生成日志信息](https://developer.apple.com/documentation/os/logging/generating_log_messages_from_your_code)。

TestFlight  
    从测试人员处获取关于测试版应用使用体验的反馈。填写测试版的测试信息页面，并要求测试人员提供有关应用性能的反馈。

### 在物理设备上进行剖析

一般来说，应尽量在物理设备上而不是在模拟器中进行性能分析。即使某些功能在模拟器中运行良好，在设备上也可能无法满足所有使用情况。模拟器不支持某些硬件功能和 API。在 macOS 上运行的模拟器的渲染管道存在差异，因此渲染性能特性也会不同。其他管道（如输入传送和音频或视频播放）也有所不同。不过，在模拟器中进行剖析可以获得一些见解，例如 CPU 停顿，这有助于发现需要调查和解决的问题。

### 构建自动化测试用例并定期运行

Xcode 自带的工具可以帮助你自动收集性能数据：

- 使用 [XCTest](https://developer.apple.com/documentation/xctest) 框架构建测试用例来收集性能指标。XCTest 可让你收集多种不同的指标，包括执行操作所需的时间、测试期间 CPU 的活动量、内存或存储使用的详细信息等。
- 使用 Instruments 收集与应用进行特定交互的指标。记录这些交互，稍后回放以收集一组新的指标。
- 编写自定义脚本，使用系统命令行工具收集性能相关数据。将这些脚本集成到项目的构建流程中，使其自动执行。

配置 Xcode 以在每次构建应用时运行测试用例，或创建一个单独的目标以按需运行测试用例或自定义脚本。将性能测试集成到 Xcode Cloud 工作流或你自己的自定义持续集成解决方案中。

> 注意  
> 使用你应用的正式版本收集性能数据，以获得更准确的结果。调试版本包含支持调试操作和日志记录的附加代码。你也可以从调试版本中收集数据，但要将这些指标与生产版本指标区分开。

有关如何为应用编写测试用例的信息，请参阅[测试覆盖率](https://developer.apple.com/documentation/Xcode/test-coverage)。有关如何使用 Xcode Cloud 自动测试的信息，请参阅 [Xcode Cloud](https://developer.apple.com/documentation/Xcode/Xcode-Cloud)。

---

## 另见

### 性能

- [分析 visionOS 应用的性能](https://developer.apple.com/documentation/visionos/analyzing-the-performance-of-your-visionos-app)  
使用 Instruments 中的 RealityKit Trace 模板来评估和提高你的 visionOS 应用的性能。
- [降低用户界面在 visionOS 上的渲染成本](https://developer.apple.com/documentation/visionos/reducing-the-rendering-cost-of-your-ui-on-visionos)  
在 visionOS 上优化 2D 用户界面渲染。
- [在 visionOS 上降低 RealityKit 内容的渲染成本](https://developer.apple.com/documentation/visionos/reducing-the-rendering-cost-of-realitykit-content-on-visionos)  
优化你的应用的 3D 增强现实内容，以便在 visionOS 上高效渲染。
- [了解 visionOS 渲染管道](https://developer.apple.com/documentation/visionos/understanding-the-visionos-render-pipeline)  
比较 visionOS 在处理事件和管理渲染循环方面与其他 Apple 平台的不同之处。
- [配置应用图标](https://developer.apple.com/documentation/Xcode/configuring-your-app-icon)  
添加应用图标变化，以便在设置、搜索结果和 App Store 等地方展示你的应用。