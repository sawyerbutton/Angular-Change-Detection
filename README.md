# Angular-Change-Detection
- Slides for Angular change detection
- 变更检测的基本任务是获取程序的内部状态，并使其以某种方式对用户界面可见
- 这种状态可以是任何类型的对象，数组，主要类型或者说 原始类型 只是任何类型的JavaScript数据结构，都可以算作状态
- 这种状态可能最终成为用户界面中的段落，表单，链接或按钮，特别是在Web上，它是文档对象模型（DOM）。所以基本上我们将数据结构作为输入并生成DOM输出以将其显示给用户。我们将此过程称为渲染
- 我们如何弄清楚模型中的变化，以及我们需要更新DOM的位置
- 访问DOM树总是很昂贵，因此我们不仅需要找出需要更新的位置，而且我们还希望尽可能地减少开销。
- 面的组件只显示两个属性，并提供了一种方法，可以在单击模板中的按钮时更改它们。单击此特定按钮的那一刻是应用程序状态发生更改的时刻，因为它会更改组件的属性。那是我们想要更新视图的那一刻
- 基本上每当执行一些异步操作时，我们的应用程序状态可能已经改变。这时就是angular被通知出现变更需要检测的时刻
