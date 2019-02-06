## 说明

该 Phar 打包示例代码 Fork 自 [SegmentFault/phar-sample](https://github.com/SegmentFault/phar-sample), 打包工具应用 [humbug/box](https://github.com/humbug/box)

## 打包

### 安装

```bash
composer global require humbug/box
```

### 配置

> 创建 box.json, 配置项如下

```js
{
    "main": "portal/index.php",
    "output": "Sample.phar",
    "alias": "deploy",
    "directories": [
        "app",
        "lib"
    ],
    "finder": [],
    "compression": "NONE",
    "compactors": [
        "KevinGH\\Box\\Compactor\\Json",
        "KevinGH\\Box\\Compactor\\Php"
    ]
}
```
- `main` 用于设定应用的入口文件，也就是打包 PHAR 后，直接运行该 PHAR 包所执行的代码，你可以在某种意义上理解为 index.php。
- `output` 用于设定 PHAR 的输出文件，可以包含目录，相对路径或绝对路径。
- `directories` 用于指定打包的 PHP 源码目录。
- `finder` 配置相对比较复杂，底层是使用 `Symfony/Finder` 实现，与 `PHP-CS-Fixer` 的 Finder 规则类似。在以上例子中，包含两个 Finder；
    - 第一个定义在 vendor 文件夹内，排除指定名称的文件和目录；
    - 第二个表示包含应用根目录的 composer.json
- `compression` 用于设定 PHAR 文件打包时使用的压缩算法。可选值有：GZ（最常用） / BZ2 / NONE（默认）。但有一点需要注意：使用 GZ 要求运行 PHAR 的 PHP 环境已启用 Gzip 扩展，否则会造成报错
- `compactors` 用于设定压缩器，但此处的压缩器不同于上文所介绍的 compression；一个压缩器类实例可压缩特定文件类型，降低文件大小，例如以下 Box 自带的压缩器：
    - KevinGH\Box\Compactor\Json：压缩 JSON 文件，去除空格和缩进等。
    - KevinGH\Box\Compactor\Php：压缩 PHP 文件，去除注释和 PHPDoc 等。
    - KevinGH\Box\Compactor\PhpScoper：使用 humbug/php-scoper 隔离代码。
- `git` 用于设定一个「占位符」，打包时将会扫描文件内是否含有此处定义的占位符，若存在将会替换为使用 Git 最新 Tag 和 Commit 生成的版本号（例如 2.0.0 或 2.0.0@e558e33）。你可以参考 这里 的代码来更加深入地理解该用法。

## 打包

```bash
box compile
```

```bash
./Sample.phar
# Hello World!
```
