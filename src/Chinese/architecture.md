# 架构
让我们从基础开始！您可能已经非常熟悉图形用户界面了。
您可以在手机、计算机和大多数交互式电子设备上找到它们。事实上，您很可能正在使用一个来阅读这本书！

从本质上讲，图形用户界面是向用户图形化**显示**一些信息的应用程序。然后用户可以选择与应用程序**交互**——通常使用某种设备；比如键盘、鼠标或触摸屏。

<div align="center">
  <img alt="界面显示，用户交互" src="../resources/gui-displays-user-interacts.svg">
</div>

用户交互可能导致应用程序更新并因此显示新信息，这反过来可能导致进一步的用户交互，进而导致进一步的更新...等等。这种快速的反馈循环是产生*交互性*感觉的原因。

> 注意：在本书中，我们将图形用户界面称为 **GUI**、**UI**、**用户界面**或简称**界面**。从技术上讲，并非所有界面都是图形化的或面向用户的；但是，考虑到本书的上下文，我们将交替使用所有这些术语。

## 剖析界面
由于我们有兴趣创建用户界面，让我们仔细看看它们。我们将从一个非常简单的开始：经典的计数器界面。它由什么组成？

<div align="center">
  <img alt="经典计数器界面" src="../resources/counter-interface.svg">
</div>

正如我们可以清楚地看到的，这个界面有三个视觉上不同的元素：两个按钮中间有一个数字。我们将用户界面的这些视觉上不同的元素称为**组件**或**元素**。

一些**组件**可能是交互式的，比如按钮。在计数器界面中，按钮可以用来触发某些**交互**。具体来说，顶部的按钮可以用来增加计数器值，而底部的按钮可以用来减少它。

我们也可以说用户界面是*有状态的*——有一些**状态**在交互之间持续存在。计数器界面显示一个表示计数器值的数字。显示的数字将根据我们按按钮的次数而改变。按一次增加按钮将导致与按两次不同的显示值。

<div align="center">
  <img alt="剖析的计数器界面" src="../resources/counter-interface-annotated.svg">
</div>

## GUI 三要素
我们的快速剖析成功识别了用户界面中的三个基本思想：

- **组件** — 界面的不同视觉元素。
- **交互** — 可能由某些组件触发的动作。
- **状态** — 界面的底层条件或信息。

这些思想相互连接，形成另一个反馈循环！

**组件**在用户与它们交互时产生**交互**。这些**交互**然后改变界面的**状态**。改变的**状态**传播并决定必须显示的新**组件**。这些新**组件**然后可能产生新的**交互**，这可以再次改变**状态**...等等。

<div align="center">
  <img alt="GUI 三要素" src="../resources/the-gui-trinity.svg">
</div>

这些思想及其连接构成了用户界面的基本架构。因此，创建用户界面必然包括定义这些**组件**、**交互**和**状态**；以及它们之间的连接。

## 不同的思想，不同的性质
界面的三个基本思想在可重用性方面差异很大。

界面的状态和交互对应用程序及其目的非常特定。如果我告诉您我有一个具有数值和增加和减少交互的界面，您很容易猜到我在谈论计数器界面。

然而，如果我告诉您我有一个具有两个按钮和一个数字的界面...您猜测我在谈论什么样的界面就相当困难了。它可能是任何东西！

这是因为组件通常非常通用，因此更可重用。大多数界面显示熟悉组件的组合——比如按钮和数字。事实上，用户期望熟悉的组件总是以某种方式行为。如果它们行为不当，界面将不直观并具有糟糕的[用户体验]。

虽然组件通常非常可重用；但由应用程序状态及其交互决定的特定组件配置是非常特定于应用程序的。按钮是通用的；但具有"+"标签并在按下时导致值增加的按钮是非常特定的。

所有这些意味着，当我们创建特定的用户界面时，我们不想专注于实现每个熟悉的组件及其行为。相反，我们想要利用组件作为可重用的构建块——独立于我们的应用程序并由某个库提供——同时将我们的重点放在基本架构的应用程序特定部分：状态、交互、交互如何改变状态，以及状态如何决定组件。

<div align="center">
  <img alt="GUI 的应用程序特定部分" src="../resources/the-gui-trinity-focused.svg">
</div>

[用户体验]: https://en.wikipedia.org/wiki/User_experience

## Elm 架构
事实证明，界面架构的四个应用程序特定部分也是[Elm 架构]的四个基本思想。

> Elm 架构是一种用于构建交互式程序的模式，它在 [Elm] 中自然出现，Elm 是一种用于可靠 Web 应用程序的令人愉悦的纯函数式编程语言。
>
> 在纯函数式编程语言中出现的模式和思想在 Rust 中往往工作得很好，因为它们利用了不可变性和[引用透明性]——这两个非常理想的属性不仅使代码易于推理，而且与借用检查器配合得很好。
>
> 此外，Elm 架构不仅在 Elm 中自然出现，而且在简单地剖析用户界面并形式化其内部工作时也会出现；就像我们刚才在本章中所做的那样。

Elm 架构为其基本部分使用不同的——如果不是更精确的——命名法：

- **模型** — 应用程序的状态。
- **消息** — 应用程序的交互。
- **更新逻辑** — 消息如何改变状态。
- **视图逻辑** — 状态如何决定组件。

这些是不同的名称，但它们指向我们已经发现的完全相同的基本思想，因此可以互换使用。

<div align="center">
  <img alt="Elm 架构" src="../resources/the-elm-architecture.svg">
</div>

> 注意：在 iced 中，**状态**和**消息**这些名称比**模型**和**交互**使用得更频繁。

[Elm 架构]: https://guide.elm-lang.org/architecture/
[Elm]: https://elm-lang.org/
[引用透明性]: https://en.wikipedia.org/wiki/Referential_transparency