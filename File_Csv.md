> ### csv简介

Csv文件又称逗号分割值文件，是以纯文本的形式存储表格数据。纯文本意味着该文件必须像二进制文件那样解析，使用记事本打开，可以看到数据都是以逗号分割。csv文件由任意条数据组成，记录间以换行符分割，每条数据字段间以逗号分割。csv文件跟Excel文件虽然都是表格文件，但是格式有很大不同，Excel文件用文本编辑器打开是一堆乱码。csv文件的出现就是为了实现简单的数据存储，是一种纯文本文件，最广泛的应用就是在程序之间转移表格数据。

实际中可能不按逗号分割而是自定义符号分割，并且默认英文逗号,和英文引号"需要特殊处理

> ### 创建csv文件

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

private static void WriteWithFileStream1()
{
    string path = GetPath("TestCsv1.csv");
    using (FileStream fs = new FileStream(path, FileMode.Create, FileAccess.Write))
    {
        using (StreamWriter sw = new StreamWriter(fs, System.Text.Encoding.UTF8))
        {
            string data = "";
            DataTable dataTable = GetDataTable();
            // 写入表头
            for (int i = 0; i < dataTable.Columns.Count; i++)
            {
                data += dataTable.Columns[i].ColumnName.ToString();
                if (i < dataTable.Columns.Count - 1)
                {
                    data += ",";
                }
            }

            sw.WriteLine(data);

            // 写入每行的各列数据
            for (int i = 0; i < dataTable.Rows.Count; i++)
            {
                data = ""; // 直接利用了上面的声明局部变量
                for (int j = 0; j < dataTable.Columns.Count; j++)
                {
                    string str = dataTable.Rows[i][j].ToString();
                    data += str;
                    if (j < dataTable.Columns.Count - 1)
                    {
                        data += ",";
                    }
                }

                sw.WriteLine(data);
            }

            sw.Close();
            fs.Close();
        }
    }
}

private static void WriteWithFileStream2()
{
    DataTable dataTable = GetDataTable();
    // 判断数据存在性
    if (dataTable.Rows.Count < 1 || dataTable.Columns.Count < 0)
    {
        return;
    }

    // 创建一个StringBuilder存储数据
    StringBuilder stringBuilder = new StringBuilder();

    // 读取数据
    for (int i = 0; i < dataTable.Columns.Count; i++)
    {
        if (i < dataTable.Columns.Count - 1)
        {
            stringBuilder.Append(dataTable.Columns[i].ColumnName + ",");
        }
    }

    stringBuilder.Append("\r\n");

    for (int i = 0; i < dataTable.Rows.Count; i++)
    {
        for (int j = 0; j < dataTable.Columns.Count; j++)
        {
            if (j < dataTable.Columns.Count - 1)
            {
                // 使用 , 分割每一个数值
                stringBuilder.Append(dataTable.Rows[i][j] + ",");
            }
        }
        // 使用换行符分割每一行
        stringBuilder.Append("\r\n");
    }


    string path = GetPath("TestCsv2.csv");
    // 写入文件
    using (FileStream fileStream = new FileStream(path, FileMode.Create, FileAccess.Write))
    {
        // using (TextWriter textWriter = new StreamWriter(fileStream, Encoding.UTF8))
        using (StreamWriter textWriter = new StreamWriter(fileStream, Encoding.UTF8))
        {
            textWriter.Write(stringBuilder.ToString());
        }
    }
}

/// <summary>构建DataTable</summary>
/// <returns>DataTable数据</returns>
private static DataTable GetDataTable()
{
    // 创建表 设置表名
    DataTable dataTable = new DataTable("Sheet1");
    // 创建列 三列
    dataTable.Columns.Add("名字");
    dataTable.Columns.Add("年龄");
    dataTable.Columns.Add("性别");
    // 创建行 每行有三列数据
    DataRow dataRow1 = dataTable.NewRow();
    dataRow1["名字"] = "张，三"; // 禁止出现英文逗号和引号，否则特殊处理
    dataRow1["年龄"] = "20";
    dataRow1["性别"] = "男";
    // 创建行 每行有三列数据
    DataRow dataRow2 = dataTable.NewRow();
    dataRow2["名字"] = "李华";
    dataRow2["年龄"] = "18";
    dataRow2["性别"] = "女";
    dataTable.Rows.Add(dataRow1);
    dataTable.Rows.Add(dataRow2);
    return dataTable;
}
```

> ### 读取csv文件

```
static public void StartRead()
{
    DataTable dataTable = new DataTable();
    string path = GetPath("TestCsv1.csv");
    using (FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Read))
    {
        using (StreamReader sr = new StreamReader(fs, Encoding.UTF8))
        {
            // 记录每次读取的一行记录
            string lineStr = "";
            // 记录每行记录中各字段内容
            string[] lineArray = null;
            string[] tableHead = null;
            // 标识列数
            int columnCount = 0;
            // 标识是否是读取的第一行
            bool isFirst = true;
            // 逐行读取csv中的数据
            while ((lineStr = sr.ReadLine()) != null)
            {
                if (isFirst == true)
                {
                    tableHead = lineStr.Split(',');
                    isFirst = false;
                    columnCount = tableHead.Length;
                    
                    // 创建列
                    for (int i = 0; i < columnCount; i++)
                    {
                        DataColumn dataColumn = new DataColumn(tableHead[i]);
                        dataTable.Columns.Add(dataColumn);
                    }
                }
                else
                {
                    lineArray = lineStr.Split(',');
                    DataRow dataRow = dataTable.NewRow();
                    for (int i = 0; i < columnCount; i++)
                    {
                        dataRow[i] = lineArray[i];
                    }

                    dataTable.Rows.Add(dataRow);
                }

                if (lineArray != null && lineArray.Length > 0)
                {
                    dataTable.DefaultView.Sort = tableHead[0] + " " + "asc";
                }
                sr.Close();
                fs.Close();
                
                Debug.Log(dataTable.Rows[0][0]);
                Debug.Log(dataTable.Rows[0][1]);
                Debug.Log(dataTable.Rows[0][2]);
            }
        }
    }
}
```

* [csv读取数据，兼容单元格中包含逗号](https://blog.csdn.net/luoyikun/article/details/80165946)