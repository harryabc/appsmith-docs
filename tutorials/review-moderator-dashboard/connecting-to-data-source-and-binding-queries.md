---
description: 第一部分
---

# 连接到数据源和绑定查询

### **连接到 Postgres 数据库**

我们支持各种数据源，并允许您在这些数据源上编写查询，以从应用程序执行不同的操作。在本教程中，您将连接到具有以下表的 Postgres 数据源：

1. **Business Table**: 这包含了从Yelp数据中筛选出来的一些企业的详细信息。
2. **Review Table**: 此表包含与业务表中列出的业务相关联的评论。

现在，让我们使用这个模拟数据源，按照以下步骤为应用程序获取所有数据：

1. 首先，单击`数据源`旁边的`+`图标。
2. 接下来，您将看到可以连接到的数据源选项的列表。
3. 选择Postgres并将数据源重命名为`Postgres Mock DB`。
4. 接下来，使用以下信息连接数据源。

```
Connection Mode: Read / Write
Host Address: mockdb.internal.appsmith.com
Port: 5432
Database Name: yelp
User: yelp
Password: that-annoying-yelper
```

要验证此数据源是否有效，您可以单击右下角的`测试`按钮。您应该会看到一个显示连接状态的弹出窗口。

下面是显示如何连接到数据源的GIF：

![Connecting to a Datasource on Appsmith](https://lh6.googleusercontent.com/wrgdx\_gBWDc3vMr6GrrC90ELXuNTTvPrBlcNpXlemvf3uWJ50KTbbaj3IqgsrP0F1-UHK9RwSVyLJOp0icGxOuJA84Mr--3VowK-zMzuBkNr9E9ECjzYkaN5FFkyhxXCVbhmMtb-)

![Testing the Datasource on Appsmith](https://lh3.googleusercontent.com/6dd0e0oudKxsfS5yFuj4pBlDI0RUSBRj1V5KxBTeZYScZ\_GRyV4cR7SZ\_nb7MbbjW2mfRi\_Yq973wDdVLPGyzXEdpk9vh2wk61eVpjo9hJolbLCl60Xbr14F5oO8xHKxVvO6totY)

### 编写第一个查询

数据源连接成功；现在，让我们编写一个简单的数据库查询来从业务表中获取所有业务。

请按照以下步骤进行操作：

1. 首先，单击`数据源`旁边的`+`图标。
2. 在选项卡下找到已创建的`Postgres Mock DB`数据源，然后单击新建查询。
3. 这将创建一个新的数据库查询，您可以在整个页面中使用它。
4. 将该查询命名为`getBusinessData`，然后单击Select。
5. 在查询输入框中使用以下查询语句获取数据：

```sql
select * from yelp_business;
```

* 要运行此查询，请单击右上角的`运行`按钮。

就像这样，我们可以在下面的响应窗口中看到来自数据库查询的响应。

如下图：

![Running Queries on Appsmith](https://lh4.googleusercontent.com/gzno-n4ukb9e8UqPaVxomkelkZO3ktVn23bvnvTPPGJ2UJxxkRdVwRt4teyn7TYeJBXBetrvs1G41ElAKtjcEASTgVOPg1IYlTc0NT0Zb3xRUnVjZZ1rNKcT6Y3ZB\_yeQVeP-g-4)

让我们将这些数据绑定到强大的`表格`组件。

### 将查询绑定到组件

在上一节中，您已经创建了一个名为`getBusinessData`的数据库查询。让我们按照以下步骤将查询绑定到`表格`组件上：

1. 单击资源管理器中`组件`旁边的`+`图标。
2. 您将在这里找到一组很棒的UI组件，您可以使用它们来构建应用程序。
3. 将`表格`拖放到画布上。

现在，只要将`表格`组件放到画布上，您就会发现一个新的浮动窗口。您可以将其称为组件属性窗口。在这里，您可以找到表格和自定义属性的所有配置。下面是表格组件及其属性窗口的屏幕截图：

![Appsmith Table Widget and Property Pane](https://lh6.googleusercontent.com/n\_uOOPk4lVhZ8W\_a6KEIRMOsRHLbG2DNbsM0kS0zH9rbFNfCzvA8B2Qfg8\_SeIXqYVy81e18OQw\_Pz6N5wgF-gjPssUioYDpMU4QVaW\_NDZ3eQjR9JVMqOX9Hgi3N4HfnLHUjxIg)

现在，让我们看一下表格的属性：

**数据**: 要向表格中添加数据，我们可以更新属性窗口的`数据`属性。默认情况下，它有一些初始配置；您可以根据自己的喜好进行更新。但还要确保它只接受数组数据类型。

**数据列**: 在`数据列`属性下，您可以配置所有列数据。您可以单击齿轮图标并单独设置列数据类型。

这是表格组件所需的两个基本属性。但是，许多其他属性允许您添加不同的操作和自定义UI。

现在，在表数据属性中，让我们绑定来自数据库查询的响应。为此，您必须使用`大括号`操作符。

{% hint style="success" %}
在这里中，你可以在任何地方使用`大括号`操作符`{{ /* This is JavaScript */ }}`来编写JavaScript。例如，将数据绑定到组件上，将参数发送到API，跨页面共享数据等等。
{% endhint %}

现在将以下内容复制到表格的数据属性中：

```javascript
{{ getBusinessData.data  }}
```

当您将其复制到表数据时，您应该会看到数据神奇地填充到表中。然后，根据您的偏好，您可以自定义列名称，并使用其他属性设置表格组件的样式。下面是表格的截图：

![](https://lh6.googleusercontent.com/-6nc-MyTFtR61saffFNb4sTAOj\_XJn81A\_alkq3ofkLmBhlHTmOp1yjmMWQzrjM1rbtfIkO\_KzHgVypRtiSb6ppoOs7PLtnW5AKD2-qLrm7macsddznbYRPkv30OuysQ9gvzcgJp)

但这里刚刚发生了什么？

在这里，我们使用查询名称`getBusinessData`访问整个查询对象，并且`.data`允许我们附加数据。

将表格的数据设置为`{{ getBusinessData.data }}`还可以确保无论何时加载页面，`getBusinessData`都会自动运行。但是，您可以通过切换`getBusinessData`的设置选项卡上的`页面加载完成后执行`字段来更改此默认行为。

### 变量和名称

在前面的部分中，我们使用了名称来访问组件和查询。例如，您通过访问查询名称的属性来访问查询的结果。从这个意义上说，可以将组件、API和数据库查询视为具有名称的变量。类似于其他编程语言中的变量：

1. 它们代表一个对象，无论是组件、API 对象还是查询对象
2. 它们支持一组方法
3. 他们有一个范围； 它们只能从其父页面中访问
4. 页面中的所有名称都必须是唯一的，包括组件名称、查询名称或API名称。

正如您将在下一节中看到的，与此相反的情况也是可能的，查询也可以访问组件的状态。此外，页面的所有构建块（组件、数据库查询和API）都可以使用其名称访问彼此的数据和/或状态。
