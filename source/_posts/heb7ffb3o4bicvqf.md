---
title: php简单读取文档数据的几种方法对比和效率
urlname: heb7ffb3o4bicvqf
date: '2024-04-17 02:18:36 +0000'
tags:
  - 读取文档
  - 效率
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_author: 小灰灰
copyright_url:
cover:
---

> 在处理大数据文件时，效率往往受到文件大小、内存限制、CPU 性能以及具体实现算法的影响。以下是对之前给出的几种方法在处理大数据效率方面的分析：

## 使用 file() 函数

file()函数会一次性将整个文件内容加载到内存中，转化为数组。对于非常大的文件，这可能会导致内存溢出。此外，一次性处理大量数据也增加了 CPU 负担。因此，对于大数据文件，这种方法的效率较低，且可能因内存限制而无法处理。

```php
$filename = 'path/to/your_file.txt';
$fileContent = file($filename, FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES); // 去除换行符并忽略空行

// 使用array_map处理每一行，按逗号分割
$data = array_map(function ($line) {
    return explode(',', $line);
}, $fileContent);

// 合并为一维数组并去重
$flatData = array_unique(array_merge(...$data));

print_r($flatData);
```

## 使用 file_get_contents() 和 preg_split() 函数

file_get_contents()同样会一次性将文件内容读取到内存中，虽然之后使用 preg_split()时不需要额外的文件读取操作，但正则表达式的复杂度可能影响处理速度。同样，内存消耗和一次性处理大量数据的问题依然存在，使得这种方法在处理大数据时效率不高且可能受限于内存。

```php
$filename = 'path/to/your_file.txt';
$fileContent = file_get_contents($filename);

// 使用正则表达式按换行和逗号分割
$data = preg_split("/[\n,]+/", $fileContent);

// 去除重复值
$uniqueData = array_unique($data);

print_r($uniqueData);

```

## 使用 fopen()、fgets() 和 explode() 函数

这种逐行读取文件的方法具有较好的内存管理特性。每次只读取文件的一小部分（一行），处理完成后释放内存，再读取下一行。这种方式对内存的占用相对较小，更适合处理大数据文件。然而，频繁的文件读取操作和字符串处理（如 explode()）可能会增加 CPU 负担。

```php
$filename = 'path/to/your_file.txt';
$file = fopen($filename, 'r');

$data = [];
while (($line = fgets($file)) !== false) {
    $trimmedLine = trim($line);
    if ($trimmedLine === '') {
        continue; // 跳过空行
    }

    $items = explode(',', $trimmedLine);
    if (!empty($items)) {
        $data[] = $items;
    }
}

fclose($file);

// 合并为一维数组
$flatData = array_merge(...$data);

// 去除重复值
$uniqueFlatData = array_unique($flatData);

// 输出处理后的数组
print_r($uniqueFlatData);

```

## 使用 SPL 的 SplFileObject 类

SplFileObject 类提供了类似 fopen()、fgets()的逐行读取机制，但它以面向对象的方式封装了这些操作，提供了更丰富的功能和更易用的接口。在处理大数据文件时，它的内存效率与方法 3 相似，同样具有良好的内存管理特性，避免一次性加载整个文件到内存。

```php
$filename = 'path/to/your_file.txt';
$file = new SplFileObject($filename);

$data = [];
while (!$file->eof()) {
    $line = $file->fgets();
    $trimmedLine = trim($line);
    if ($trimmedLine === '') {
        continue; // 跳过空行
    }
    $items = explode(',', $trimmedLine);
    if (!empty($items)) {
        $data[] = $items;
    }
}

// 合并为一维数组并去重
$flatData = array_unique(array_merge(...$data));

print_r($flatData);

```

## 结论：

对于大数据文件，方法 3**（fopen()、fgets()、explode()）**和方法 4（**SplFileObject 类**）由于采用了逐行读取的方式，能够有效降低内存消耗，避免一次性加载整个文件带来的内存压力，因此在效率上优于方法 1（file()）和方法 2（file_get_contents() + preg_split()）。在这两者之间，方法 4 提供了面向对象的接口和更丰富的功能，但底层处理逻辑与方法 3 相似，因此在处理大数据效率上，二者相差不大，主要取决于具体的实现细节和环境因素（如 CPU 性能、磁盘 I/O 速度等）。

综上所述，推荐在处理大数据文件时使用方法 3（fopen()、fgets()、explode()）或方法 4（SplFileObject 类），它们在内存效率上表现更好，更适合应对大数据场景。在实际应用中，如果对面向对象编程更熟悉或者需要利用 SplFileObject 提供的其他高级功能，可以选择方法 4；否则，方法 3 的简单直接可能更适合大多数场景。
