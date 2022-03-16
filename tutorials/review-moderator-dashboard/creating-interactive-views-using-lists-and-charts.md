---
description: 第三部分
---

# 创建交互式视图，使用列表和图表

### 在上下文对象中存储数据

您现在几乎可以使用超酷的数据面板了。 接下来，让我们添加一个将重定向到新页面的按钮。 此外，在新页面上，您将需要 `business_id` 来过滤评论表中的评论。 因此，现在将值存储在上下文对象中并将其用作参考。 请按照以下步骤操作：

1. 在业务详细信息下拖放一个按钮组件
2. 打开按钮属性窗口并将按钮文字更改为查看评论
3. 在`动作`下，切换`JS`按钮并粘贴以下代码：

```javascript
{{
(function () {
    storeValue("business_id", businessTable.selectedRow.business_id);
    navigateTo("Business Reviews") })() 
}}
```

太棒了，你已经成功的编写了你的第一个JS函数。正如您所注意到的，这些是主要的调用函数，这意味着它在定义后立即运行。在函数内部，我们首先使用`storeValue`函数将`businessTable`中的业务键存储到`business_id`变量中。接下来，使用`navigateTo`函数重定向到新页面。

{% hint style="info" %}
要在另一个页面上访问小部件的属性或APIs/DB查询的结果，有两种方法：

1. 使用`storeValue`函数将数据存储在浏览器缓存中，这样即使用户移动到应用程序中的另一个页面，也可以访问这些数据。
2. 在将用户重定向到的页面的URL中，将数据作为查询参数传递。这可以使用`navigateTo`函数来完成。
{% endhint %}

![Storing Value and Redirecting to a Page](https://lh6.googleusercontent.com/fhRvo5jcgs7LOYIsmytxmhj0F2x9TnKiPUYB5\_ElSCIA\_qMlChit2Hr\_BL9m8\_i0fj8e1kbeGZ0CHP5KVMj5XC\_8GS4ZV0r9TtfA4nKhFV1qbn\_3AWPN9NFe2futv7wKrAItVxVA)

正如您在这里所看到的，只要单击按钮，它现在就会导航到一个新页面！此外，我们还保存了选定行中的`business_id`。

### 在数据库查询中使用存储值

现在，让我们再编写一个数据库查询，以根据所选的`business_id`，按照以下步骤从表中筛选评论：

1. 首先，单击`数据源`旁边的`+`图标。
2. 在选项卡下找到已创建的`Postgres Mock DB`数据源，然后单击`新建查询`。
3. 将查询重命名`filterBusinessReviews`.
4. 现在将以下查询语句复制粘贴到查询窗口中：

```sql
select * from yelp_review where business_id = {{appsmith.store.business_id}}
```

在这里，我们从`yelp_review`表中选择所有行，并在导航到新页面时通过保存在存储值中的`business_id`变量对它们进行筛选。您可以使用`大括号语法`将其用于WHERE子语句。现在运行吧！

{% hint style="info" %}
提示: 您还可以使用 `CMD + return` 或 `CTRL + enter` 快捷方式来运行查询。
{% endhint %}

通过运行查询，我们可以看到基于所选`business_id`的所有评论，此外，在设置选项卡中，请确保开启`页面加载完成后执行`选项。

![](https://lh5.googleusercontent.com/wqJ9SwtblDQw397mgyAv1ZzwFf6lUut\_CHlx9QFhkVeTIbz2bhwD1mMNLN5bAkZQ207QIVXGz-IlwBZBDDoF5thWGTeVxSB2ovSJJFajtOH6d2LPt-MMvztGxiNHwayxU1ivG\_sL)

此外，您需要在此页面上再次获取业务详细信息，这使得在数据面板上显示数据库查询的完整详细信息变得更加容易。请按照以下步骤操作：

1. 首先，单击`数据源`旁边的`+`图标。
2. 在选项卡下找到已创建的`Postgres Mock DB`数据源，然后单击`新建查询`。
3. 将查询重命名 `getBusinessDetails`.
4. 现在将以下查询语句复制粘贴到查询窗口中：

```sql
select * from yelp_business where business_id={{appsmith.store.business_id}}
```

如果运行此命令，您将只看到一行数据，该行根据存储在上下文对象中的`business_id`从业务表中获取所有业务详细信息。

现在，让我们构建一个看板，它将显示从表中获取的所有评论。

1. 首先，拖放一个容器组件并为整个页面重新排列它。
2. 接下来，添加一个文本组件并将以下内容粘贴到`文本`属性窗口中：

```sql
Found {{filterBusinessReviews.data.length}} Reviews for {{getBusinessDetails.data[0].name}}
```

![](https://lh4.googleusercontent.com/azCYvUSRkqHgu7wChBeh7CspMfcQZoyxVV903H1MG2qD1FZB7EvCAlkjpWINkt6MCvTkaU4UGlwosULyF2xrecLmIX9g2ZE18I0ojMLU1E8pPX4unLC2ZnAhsvJilpwuGNs9TZHF)

### 使用列表组件显示评论

现在，让我们使用一个列表组件来显示`filterBusinessReviews`查询中的所有评论。请按照以下步骤操作：

1. 将列表组件拖放到画布上。
2. 将`filterBusinessReviews`中的数据绑定到列表组件上。
3. 为此，请打开列表组件属性窗口，并将以下内容粘贴到`列表数据`属性中：

```
{{filterBusinessReviews.data}}
```

1. 现在，删除列表组件中已经填充的数据，并添加一些文本组件。
2. 您应该注意到，这些组件将自动添加到列表组件的其他项中。
3. 最后，您可以使用`currentItem`属性将这些项绑定到这些组件中。
4. 在文本组件中，使用`大括号语法`并绑定数据，如`{{currentItem.text}}`。

下面的图显示了如何将数据绑定到列表组件中：

![Using the List Widget](https://lh6.googleusercontent.com/9NYc90cu7lpMJAmyEHYY2uvvmdIkKfnn2NZlx4wMY\_nN9WaQ2yNYeS3VLqY9HBzUa-4n2ZGNKKbaV1Hqoz0A-x2ERBGMpZ-kFIw6tr0wvLYBiJaSr567VSA4BusyM2SwE\_HrurrN)

很好，现在您可以尝试使用`currentItem`属性将其余数据显示到列表小部件上。

### **显示日期**

将文本组件拖放到表上，并在其属性窗口的文本中添加以下代码片段：

`currentItem.date`

在这里，如果你注意到，日期不是可读的方式。您可以使用\`moment.js\`来转换它，它已经在环境中进行了配置。现在将该值更新为：

```
{{moment(currentItem.date).format("LL")}}
```

日期根据我们在.`.format()`方法中给出的类型自动格式化。

### 显示评级

显示日期后，我们可以在列表组件中的文本组件里填写以下值，将评论文本绑定到一个或多个文本小部件。

`currentItem.text`

接下来，通过在文本组件中设置以下内容来添加评论：

Stars Ratings: **``{{ `Stars: ${currentItem.stars}` }}``**

Funny Ratings: **``{{ `Funny: ${currentItem.funny}` }}``**

Useful Ratings: **``{{ `Useful: ${currentItem.useful}` }}``**

Cool Ratings: **``{{ `Cool: ${currentItem.cool}` }}``**

完成此操作后，您还可以自定义文本组件，在文本组件属性窗口中找到背景颜色属性，并添加任何背景颜色。这是应用程序的外观：

![Customising the List Widget](https://lh3.googleusercontent.com/CFG4g63CO47naltKSacaa7DEDMXWShccKnsjK6CtZ0z1w5YthoFqeMX6U1YK5ipkg8SGIIBqrlUzkwgUQXnSkVp2wkhaAm2m\_1wp3SxSFoZ2IDBQKm5Klayz4Bkc-pTJHmNrifaz)

### 添加图表组件

图表组件用于查看数据的图形展示。它可用于多种配置，但是，如果您想要进行高级可视化，您可以选择自定义配置并使用`Custom Fusion Chart Configuration`

现在，按照以下步骤创建一个图表，用于根据评论对企业的评级进行可视化。

1. 将图表组件拖放到画布上
2. 打开图表组件属性窗口，并将图表类型设置为折线图。
3. 接下来，将标题设置为业务的星级，并使用以下代码片段来传递图表数据的坐标。

```
{{filterBusinessReviews.data.map((item, index)=>{return {x:moment(item.date).format("L"), y:item.stars}})}}
```

好样的！这样，您应该能够看到图表小部件上绘制的所有数据。同样，我们可以通过单击`ADD SERIES`选项来绘制其他评级。
