---

---
#### 记录Rust中零散的知识点，不一定包含逻辑关联
---

##### **tags:** #笔记 
##### **2025-08-15**

---
## 切片与借用
- [[值与指针#指针传递与借用|借用类似于指针]]
- 切片就是一类特殊的借用，是对**连续内存数据**的专属借用名称
```
fn main(){
	let data =  42;
	let p = &data;// p称为借用

	let str = "String".to_string;
	let pstr = &str;// pstr称为切片
}
```
- 切片包含两部分信息：一个指向数据的指针和数据的长度

## 类
- Rust 中无类的概念，但是可以使用多种机制的组合构建类
- 使用结构体 `struct` 或是枚举 `enum` 作为类的属性
- 使用拓展方法 `impl` 来定义类的构造方法和行为
- 特质 `trait` 相当于 OOP 中的接口+抽象类
- 多态可通过 Rust 中的泛型实现
```
// trait代替接口
trait Speak {
    fn speak(&self) -> String;
}

// 结构体代替类属性
struct Animal {
    name: String, 
}

impl Animal {
    // 构造函数（关联函数）
    fn new(name: &str) -> Self {
        Self {
            name: name.to_string(),
        }
    }

    // 类方法(行为)
    fn get_name(&self) -> &str {
        &self.name
    }
}

// 组合代替继承
struct Dog {
    animal: Animal, 
}

impl Dog {
    fn new(name: &str) -> Self {
        Self {
            animal: Animal::new(name),
        }
    }
}

impl Speak for Dog {
    fn speak(&self) -> String {
        format!("{} says: Woof!", self.animal.get_name())
    }
}

struct Cat {
    animal: Animal,
}

impl Cat {
    fn new(name: &str) -> Self {
        Self {
            animal: Animal::new(name),
        }
    }
}

impl Speak for Cat {
    fn speak(&self) -> String {
        format!("{} says: Meow!", self.animal.get_name())
    }
}

// 多态函数
fn make_speak(pet: &dyn Speak) {
    println!("{}", pet.speak());
}

fn main() {
    let dog = Dog::new("Buddy");
    let cat = Cat::new("Kitty");

    // 编译期看作不同类型，运行时通过 trait 对象实现多态
    make_speak(&dog);
    make_speak(&cat);
}
```

## 结构体与枚举的区别
- 结构体是「与」关系的数据，代表着这个结构体中的数据是该对象都有的，比如一个人既有种族，又有年龄，性别
- 枚举中是「或」关系的数据，代表着该对象所拥有的只是枚举中的某一个，如枚举中如果包含许多不同的年龄，那么在当前时空任一时间点，对象只会拥有唯一的一个年龄，即只会拥有唯一的枚举数据
- 通常可以使用枚举中的数据作为结构体的数据的右值
  此处为了输出简便，使用了带属性的[[Rust中的枚举|枚举]]
```
enum Color {
	Red(String),
	Blue(String),
	Yellow(String),
}

struct Pen {
	size: i32,
	color: Color,
}

fn main(){
	let new_pen = Pen{
		size: 42,
		color: Color::Yellow,
	};
	println!("the pen is {}",new_pen.color);
}
```

