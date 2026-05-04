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

## Some() e unwrap() 

```rs
fn main() {
    let a: Option<i32> = Some(10);
    let b: Option<i32> = None;

    let x = a.unwrap(); // FUNCIONA
    println!("x = {}", x);

    let y = b.unwrap(); // CRASH
    println!("y = {}", y);
}
```

```
unwrap serve para sacar o valor de some()
mas se deres unwrap de algo com None dá crash 
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

# dbg! com estruturas

## Exemplo de estrutura || Pessoa

```rs
use chrono::NaiveDate;

#[derive(Debug)]
struct Pessoa {
    nome: String,
    data_nascimento: NaiveDate,
    iban: String,
    saldo: f64,
}

fn main() {
    let pessoa = Pessoa {
        nome: String::from("João Silva"),
        data_nascimento: NaiveDate::from_ymd_opt(1995, 5, 20).unwrap(),
        iban: String::from("PT50 0002 0123 1234 5678 9015 4"),
        saldo: 1500.75,
    };

    dbg!("{:?}", pessoa);
}
```

```
== output com println == 
Pessoa { nome: "João Silva", data_nascimento: 1995-05-20, iban: "PT50 0002 0123 1234 5678 9015 4", saldo: 1500.75 }

== output com dbg! == 

[src/main.rs:19:5] pessoa = Pessoa {
    nome: "João Silva",
    data_nascimento: 1995-05-20,
    iban: "PT50 0002 0123 1234 5678 9015 4",
    saldo: 1500.75,
}
```
