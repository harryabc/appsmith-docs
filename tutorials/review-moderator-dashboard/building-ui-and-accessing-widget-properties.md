---
description: 第二部分
---

# 构建UI和访问组件属性

### 自定义表格组件

表格组件现在有来自查询的大量数据，但假设您可能希望只显示几列并隐藏其余列以获得更清晰的展示，并且您可以简单地打开属性窗口，找到这些列并切换`眼睛图标`。这将隐藏表中的列。现在打开表格属性窗口，仅显示以下列：

1. Name
2. Address
3. City

此外，让我们通过使用容器组件来使UI更加美观。使用它，我们可以在最小的白色背景上对多个组件进行分组。为此，请执行以下步骤：

1. 将容器组件拖放到画布上
2. 将表格组件移动到容器组件中
3. 此外，在表格组件的顶部添加一个文本组件以添加标题。

如下图:

![](https://lh5.googleusercontent.com/uavbi64o75sNEAHxGBC7LhBT50q2OXPz6H0z47-Ul9JgHFMy4f07l3EhctQ3F-0-9hyIfbqPXsp0X-fuiot-DwCeewalDbMLr\_WqL6Gx7i9p6VYWo78kqHCLCbqbYPew2repqAE4)

嗯，这令人印象深刻，就像你能够更新用户界面一样。接下来，让我们添加一些额外的组件，每当在表行上选择特定业务时，这些组件都将显示信息。为此，让我们首先将表格组件从`Table1`重命名为`BusinessTable`。

### 添加文本组件和绑定数据

好的，接下来让我们添加一些文本组件。为此，您可以将文本组件拖放到画布上，并添加其关联的名称和值，如：

Name: `{{businessTable.selectedRow.name}}`

Address: `{{businessTable.selectedRow.address}}`

City: `{{businessTable.selectedRow.city}}`

Business ID: `{{businessTable.selectedRow.business_id}}`

Business Rating: `{{businessTable.selectedRow.stars}}`

Categories Rating: `{{businessTable.selectedRow.categories}}`

![](https://lh6.googleusercontent.com/djLe2OB\_2ReB6rgUXqd9uc8riGkR848FHB98zn7gzrP5eH2fluy3SBrsuisxU2QJ5Iq\_ihhIuwi\_rL01xMmTZwUt8Zxo-NyjQpez1WiJW1lp-IoYgCFyFcuoGqJfV1bfQKYuiGsa)

在下一部分中，您将学习如何在组件中编写JS函数来创建交互式视图。您还将使用列表和图表组件根据所选业务分析评论表中的评论。
