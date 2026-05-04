# Some() || Option<T> || Optional value

## Some() hello world
```rs
fn main() {
    let nome: Option<&str> = Some("João");

    match nome {
        Some(valor) => println!("Olá, {}", valor),
        None => println!("Não há nome"),
    }
}
```

# STACK PUSH POP || LIKE C++ VECTOR

## Stack hello world
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
## valid parentheses
```rs
pub fn is_valid(s: String) -> bool {
    let mut stack: Vec<char> = Vec::new();

    for c in s.chars() {
        match c {
            '(' | '[' | '{' => stack.push(c),

            ')' => {
                if stack.pop() != Some('(') {
                    return false;
                }
            }

            ']' => {
                if stack.pop() != Some('[') {
                    return false;
                }
            }

            '}' => {
                if stack.pop() != Some('{') {
                    return false;
                }
            }

            _ => return false,
        }
    }

    return stack.is_empty()
}
```
