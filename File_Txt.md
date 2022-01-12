> ### 前述

读取txt很简单，File类，FileStream类，StreamReader类，StreamWriter 类都可以读取。主要分两种形式，一种是文件读取形式，一种是流数据的形式。读取txt的类很多，形式多样，有的整篇读取，有的逐行读取。

读取是第一步，保存数据是第二步，读取的方式也影响了保存数据的方式，如果是逐行读取，那么可以逐行解析，然后保存到数据集合中。如果是整篇读取，就需要对整篇逐行读取，然后通过分隔符进行分割保存。

> ### 检查文件存在性

```
private static string GetPath(string fileName)
{
    string path = Application.streamingAssetsPath + "/" + fileName;
    if (File.Exists(path) == false)
    {
        File.Create(path).Dispose(); // 不加Dispose() 会报错 IOException: Sharing violation on path
    }
    return path;
}
```

> ### 写入数据

```
/// <summary>通过File类写入数据--全篇写入</summary>
private static void WriteWithFileWriteAllText()
{
    string path = GetPath("TestTxt0.txt");
    File.WriteAllText(path, "我是通过File.WriteAllText写入的\n哈哈哈");
}

/// <summary>通过File类写入数据--逐行写入</summary>
private static void WriteWithFileWriteAllLines()
{
    string path = GetPath("TestTxt1.txt");
    string[] content = {"11111", "22222", "33333"};
    File.WriteAllLines(path, content);
}

/// <summary>通过文件流写入数据</summary>
private static void WriteWithFileStream()
{
    string path = GetPath("TestTxt2.txt");
    string content = "22222";
    using (FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Write))
    {
        byte[] bytes = Encoding.UTF8.GetBytes(content);
        fs.Write(bytes, 0, bytes.Length);
        fs.Close();
    }
}

/// <summary>通过流写入</summary>
private static void WriteWithStreamWrite()
{
    string path = GetPath("TestTxt3.txt");
    string content = "33333";
    using (StreamWriter sr = new StreamWriter(path))
    {
        sr.WriteLine(content);
        sr.Close();
        sr.Dispose();
    }
}
```

> ### 读取文件

```
/// <summary>通过TextAsset类读取文档</summary>
private static void ReadWithTextAsset()
{
    TextAsset textAsset = Resources.Load<TextAsset>("TestTxt0"); // 在Resources 目录找
    if (textAsset == null)
    {
        Debug.Log($"ReadWithTextAsset 读取失败");
    }
    else
    {
        Debug.Log($"ReadWithTextAsset 读取成功 {textAsset.text}");
    }
}

/// <summary>通过File类全篇读取文档</summary>
private static void ReadWithFileReadAllText()
{
    string path = GetPath("TestTxt0.txt");
    string text = File.ReadAllText(path);
    Debug.Log($"ReadWithFileReadAllText 读取成功 {text}");
}

/// <summary>通过File类逐行读取文档</summary>
private static void ReadWithFileReadAllLines()
{
    string path = GetPath("TestTxt1.txt");
    string[] textArr = File.ReadAllLines(path);
    if (textArr.Length > 0)
    {
        Debug.Log($"ReadWithFileReadAllLines 读取成功: {string.Join(",", textArr)}");
    }
    else
    {
        Debug.Log($"ReadWithFileReadAllLines 读取失败: {path}");
    }
    // for (int i = 0; i < textArr.Length; i++)
    // {
    //     Debug.Log($"ReadWithFileReadAllLines 读取成功 {i}: {textArr[i]}");
    // }
}

/// <summary>通过FileStream类逐行读取文档</summary>
private static void ReadWithFileStream()
{
    string path = GetPath("TestTxt2.txt");
    using (FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Read))
    {
        byte[] bytes = new byte[fs.Length];
        fs.Read(bytes, 0, bytes.Length);
        fs.Close();
        if (bytes.Length > 0)
        {
            string text = Encoding.UTF8.GetString(bytes);
            Debug.Log($"ReadWithFileStream 读取成功 {text}");
        }
        else
        {
            Debug.Log($"ReadWithFileStream 读取失败 {path}");
        }
    }
}

private static void ReadWithFileOpenRead()
{
    string path = GetPath("TestTxt3.txt");
    using (FileStream fs = File.OpenRead(path))
    {
        byte[] bytes = new byte[fs.Length];
        fs.Read(bytes, 0, bytes.Length);
        fs.Close();
        if (bytes.Length > 0)
        {
            string text = Encoding.UTF8.GetString(bytes);
            Debug.Log($"ReadWithFileOpenRead 读取成功 {text}");
        }
        else
        {
            Debug.Log($"ReadWithFileOpenRead 读取失败 {path}");
        }
    }
}
```

