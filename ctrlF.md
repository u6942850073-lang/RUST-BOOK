# STACK PUSH POP || LIKE C++ VECTOR

```rs
fn main() {
    let mut stack: Vec<char> = Vec::new();

    stack.push('a');
    stack.push('b');
    stack.push('c');

    println!("{:?}", stack); // ['a', 'b', 'c']

    if let Some(top) = stack.pop() {
        println!("Popped: {}", top); // c
    }

    println!("{:?}", stack); // ['a', 'b']
}
```
