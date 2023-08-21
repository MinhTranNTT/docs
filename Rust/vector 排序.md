### 整数 Vector 排序

[![std-badge](https://badge-cache.kominick.com/badge/std-1.29.1-blue.svg)](https://doc.rust-lang.org/std) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

通过 [`vec::sort`](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.sort) 对一个整数 Vector 进行排序。另一种方法是使用 [`vec::sort_unstable`](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.sort_unstable)，后者运行速度更快一些，但不保持相等元素的顺序。

```rust
fn main() {
    let mut vec = vec![1, 5, 10, 2, 15];
    vec.sort();
    // vec.sort_unstable();
    assert_eq!(vec, vec![1, 2, 5, 10, 15]);
}

```



### 浮点数 Vector 排序

[![std-badge](https://badge-cache.kominick.com/badge/std-1.29.1-blue.svg)](https://doc.rust-lang.org/std) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

f32 或 f64 的 vector，可以使用 [`vec::sort_by`](https://doc.rust-lang.org/std/primitive.slice.html#method.sort_by) 和 [`PartialOrd::partial_cmp`](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html#tymethod.partial_cmp) 对其进行排序。

```rust
fn main() {
    let mut vec = vec![1.1, 1.15, 5.5, 1.123, 2.0];
    vec.sort_by(|a, b| a.partial_cmp(b).unwrap());
    assert_eq!(vec, vec![1.1, 1.123, 1.15, 2.0, 5.5]);
}

```



### 结构体 Vector 排序

[![std-badge](https://badge-cache.kominick.com/badge/std-1.29.1-blue.svg)](https://doc.rust-lang.org/std) [![cat-science-badge](https://badge-cache.kominick.com/badge/science--x.svg?style=social)](https://crates.io/categories/science)

依据自然顺序（按名称和年龄），对具有 `name` 和 `age` 属性的 `Person` 结构体 `Vector` 排序。为了使 `Person` 可排序，你需要四个 `trait`：[`Eq`](https://doc.rust-lang.org/std/cmp/trait.Eq.html)、[`PartialEq`](https://doc.rust-lang.org/std/cmp/trait.PartialEq.html)、[`Ord`](https://doc.rust-lang.org/std/cmp/trait.Ord.html)，以及 [`PartialOrd`](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html)。这些 `trait` 可以被简单地派生。你也可以使用 [`vec::sort_by`](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.sort_by) 方法自定义比较函数，仅按照 `age` 属性排序。

```rust
#[derive(PartialEq, Eq, PartialOrd, Ord, Debug)]
struct Person {
    name: String,
    age: u32,
}

impl Person {
    fn new(name: String, age: u32) -> Self {
        Person { name, age }
    }
}

fn main() {
    let mut people = vec![
        Person::new("Zoe".to_string(), 25),
        Person::new("Al".to_string(), 60),
        Person::new("John".to_string(), 1),
    ];

    people.sort();

    assert_eq!(
        people,
        vec![
            Person::new("Al".to_string(), 60),
            Person::new("John".to_string(), 1),
            Person::new("Zoe".to_string(), 25),
        ]
    );

    people.sort_by(|a, b| b.age.cmp(&a.age));

    assert_eq!(
        people,
        vec![
            Person::new("Al".to_string(), 60),
            Person::new("Zoe".to_string(), 25),
            Person::new("John".to_string(), 1),
        ]
    );
}

```

