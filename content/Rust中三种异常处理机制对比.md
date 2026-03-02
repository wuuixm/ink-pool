---

---
#### 分析 `原生异常处理`，`？语法糖`，`anyhow库` 这三种异常处理机制的不同
---

##### **tags:** #笔记 
##### **2026-03-02**

---
# 原生错误处理
#### 优点：
- 逻辑展开清晰，可读性高
- 所有错误路径完全显式
- 不依赖额外机制
#### 缺点：
- 重复代码多，冗长
- 耦合过多，不方便后续添加新的错误类型
- 代码量过大的时候可读性反而下降
#### 使用场景：
- 精细控制错误分支
- 教学
- 对于一些代码量不大，且无需经常修改的库函数场景
#### 举例：
```
use std::fs::File;
use std::io::{self, Read};

fn read_file() -> Result<String, io::Error> {
    let mut file = match File::open("a.txt") {
        Ok(f) => f,
        Err(e) => return Err(e),
    };

    let mut content = String::new();

    match file.read_to_string(&mut content) {
        Ok(_) => Ok(content),
        Err(e) => Err(e),
    }
}
```

## 使用？语法糖
#### 优点：
-  简洁
- 错误信息完整，无丢失
- 后续添加新错误类型方便，耦合度低
- 适合库设计
#### 缺点：
- 需要自定义错误类型
- 需要实现每个错误类型向统一错误类型的转换
#### 使用场景：
- 库开发
- 核心业务逻辑
#### 举例：
- 涉及一种错误
```
use std::fs::File;
use std::io::{self, Read};

fn read_file() -> Result<String, io::Error> {
    let mut file = File::open("a.txt")?;
    let mut content = String::new();
    file.read_to_string(&mut content)?;
    Ok(content)
}
```
- 涉及多种错误
```
#[derive(Debug)]
enum LoadError {
    Io(std::io::Error),
    Parse(std::num::ParseIntError),
}

impl From<std::io::Error> for LoadError {
    fn from(e: std::io::Error) -> Self {
        LoadError::Io(e)
    }
}

impl From<std::num::ParseIntError> for LoadError {
    fn from(e: std::num::ParseIntError) -> Self {
        LoadError::Parse(e)
    }
}

fn load_number() -> Result<i32, LoadError> {
    let content = std::fs::read_to_string("a.txt")?;
    let number = content.trim().parse::<i32>()?;
    Ok(number)
}
```

## 使用 anyhow 动态错误
#### 优点：
- 及其简洁
- 无需自定义错误类型
- 自动保留错误链
#### 缺点：
- 无法从函数签名看出错误类型
- 调用者无法知晓所有错误类型，无法穷尽匹配
- 使用了动态类型，类型信息被擦除
#### 使用场景：
- CLI 工具
- 个人应用开发
- 原型开发
- 不需要区分错误类型的场景
#### 举例：
```
use anyhow::{Result, Context};
use std::fs;

fn load_number() -> Result<i32> {
    let content = fs::read_to_string("a.txt")
        .context("读取文件失败")?;

    let number = content
        .trim()
        .parse::<i32>()
        .context("解析数字失败")?;

    Ok(number)
}
```
