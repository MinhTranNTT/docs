## 生命周期 - Type

- 除了函数，类型上也需要指定生命周期，如 `struct`、`enum`
- `struct`、`enum` 上有了生命周期，`impl` 方法 也有要对应的声明
    - 示例：[_09_example.rs](./_09_example.rs) 

        ```rust
        #![allow(unused)]

        #[derive(Debug)]
        struct SplitStr<'a> {
            start: &'a str,
            end: &'a str,
        }

        fn split<'a>(text: &'a str, delimiter: &'a str) -> Option<SplitStr<'a>> {
            let (start, end) = text.split_once(delimiter)?; 
            Some(SplitStr { start, end })
        }

        fn main() {
            let mut parts_of_string = None;
            let string;

            {
                string = String::from("First line;Second line");
                parts_of_string = split(&string, ";");
            }

            println!("{:?}", parts_of_string);
        }
        ```
    
    `?` 因为 `split_once` 返回 `Option` `?` 做简化匹配并获取值
    
    `Option` 返回值引用只和 `self` 有关

    这里的 `self`（方法的接收者，引用当前模块） 等同与 `text` 
    
    ```rust
    pub fn split_once<'a, P: Pattern<'a>>(&'a self, delimiter: P) -> Option<(&'a str, &'a str)> {
        let (start, end) = delimiter.into_searcher(self).next_match()?;
        unsafe { Some((self.get_unchecked(..start), self.get_unchecked(end..))) }
    }
    ```

    - 示例：[_10_example.rs](./_10_example.rs)

        ```rust
        
        ```