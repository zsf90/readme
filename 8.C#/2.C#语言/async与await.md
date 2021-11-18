## async与await

> C# 支持含两个关键字的**异步程序**：**async** 和 **await**。 将 **async** 修饰符添加到**方法声明**中，以声明这是**异步方法**。 **await** 运算符通知编译器异步等待结果完成。 控件返回给调用方，该方法返回一个管理异步工作状态的结构。 结构通常是 System.Threading.Tasks.Task<TResult>，但可以是任何支持 awaiter 模式的类型。 这些功能使你能够编写这样的代码：以其同步对应项的形式读取，但以异步方式执行。 例如，以下代码会下载 Microsoft Docs 的主页：

```csharp
public async Task<int> RetrieveDocsHomePage()
{
    var client = new HttpClient();
    byte[] content = await client.GetByteArrayAsync("https://docs.microsoft.com/");
    
    Console.WriteLine($"{nameof(RetrieveDocsHomePage)}: Finished downloading.");
    return content.Length; 
}
```

