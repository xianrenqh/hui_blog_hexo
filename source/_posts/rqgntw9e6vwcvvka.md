---
title: 通过 Python|PHP 生成mysql数据字典
urlname: rqgntw9e6vwcvvka
date: '2024-05-21 09:03:33 +0000'
tags: []
categories: []
---

tags: [mysql,数据字典]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.cc

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

> <font style="color:rgb(51, 51, 51);">  
> </font><font style="color:rgb(51, 51, 51);">使用PHP和python生成数据字典，直接上代码，并保存到markdown里面</font>

<font style="color:rgb(51, 51, 51);"></font>

## <font style="color:rgb(51, 51, 51);">php</font>

```php
<?php
  $database_name = '改成your database name';
$output_file = $database_name . '.md'; // Markdown 文件名

// 修改配置
$conn = new mysqli('127.0.0.1', 'root', '123456', $database_name, 3306);

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

$output = ''; // 用于存储 Markdown 内容

$query_tables = "SELECT TABLE_NAME, TABLE_COMMENT FROM information_schema.TABLES WHERE table_type='BASE TABLE' AND TABLE_SCHEMA='$database_name'";
$result_tables = $conn->query($query_tables);

if ($result_tables->num_rows > 0) {
  while ($table = $result_tables->fetch_assoc()) {
    $table_name = $table['TABLE_NAME'];
    $table_comment = $table['TABLE_COMMENT'];

    $query_columns = "SELECT ORDINAL_POSITION, COLUMN_NAME, COLUMN_TYPE, IS_NULLABLE, COLUMN_COMMENT FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='$database_name' AND TABLE_NAME='$table_name'";
    $result_columns = $conn->query($query_columns);

    $output .= "\n\n\n### $table_name ($table_comment) \n| 序号 | 字段名称 | 数据类型 | 是否为空 | 字段说明 |\n| :--: |----| ---- | ---- | ---- |\n";

    $count = 1;
    while ($column = $result_columns->fetch_assoc()) {
      $ordinal_position = $column['ORDINAL_POSITION'];
      $column_name = $column['COLUMN_NAME'];
      $column_type = $column['COLUMN_TYPE'];
      $is_nullable = $column['IS_NULLABLE'];
      $column_comment = $column['COLUMN_COMMENT'];

      $output .= "| $count | $column_name | $column_type | $is_nullable | $column_comment |\n";
      $count++;
    }
  }
}

$conn->close();

// 将 Markdown 内容写入文件
file_put_contents($output_file, $output);
?>
```

## python

```python
def generate(database_name):
    """
    生成数据库字典表
    """
    importlib.reload(sys)

    # 使用前修改配置
    conn = mysql.connector.connect(
        host='127.0.0.1',
        port='3306',
        user='root',
        password='123456',
        use_pure=True
    )

    cursor = conn.cursor()

    cursor.execute(
        "SELECT TABLE_NAME, TABLE_COMMENT FROM information_schema.TABLES WHERE table_type='BASE TABLE' AND TABLE_SCHEMA='%s'" % database_name
    )

    tables = cursor.fetchall()

    markdown_table_header = """\n\n\n### %s (%s) \n| 序号 | 字段名称 | 数据类型 | 是否为空 | 字段说明 |\n| :--: |----| ---- | ---- | ---- |\n"""
    markdown_table_row = """| %s | %s | %s | %s | %s |"""

    f = open(database_name + '.md', 'w')

    for table in tables:

        cursor.execute(
            "SELECT ORDINAL_POSITION, COLUMN_NAME, COLUMN_TYPE, IS_NULLABLE, COLUMN_COMMENT "
            "FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='%s' AND TABLE_NAME='%s'" % (
                database_name, table[0]
            )
        )

        tmp_table = cursor.fetchall()
        p = markdown_table_header % (table[0], remove_newline(table[1]))
        for col in tmp_table:
            p += (remove_newline(markdown_table_row % col) + "\n")
        print(p)
        f.writelines(p)

    f.close()

```
