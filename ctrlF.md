# Redcl é legal
```rs
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}

// The value of x in the inner scope is: 12
// The value of x is: 6
```

# String ops I (coisas fáceis)

## push (for a single char) 
```
let mut s = String::from("Hi");
s.push('!');
println!("{}", s); // Hi!
```

## push_str (for string slices) 

```rs
let mut s = String::from("Hello");
s.push_str(" world");
println!("{}", s); // Hello world
```

## Operator '+' (concatenation) 
```rs
let s1 = String::from("Hello ");
let s2 = String::from("world");
let s3 = s1 + &s2;
println!("{}", s3);
```
## format! 
```rs
let s1 = String::from("Hello");
let s2 = String::from("world");
let s3 = format!("{} {}", s1, s2);
println!("{}", s3);
```

## pop (retona Some()) 

```rs
fn main() {
    let mut s = String::from("Olá!");
    
    let last = s.pop();

    println!("{:?}", last); // Some('!')
    println!("{}", s);      // "Olá"
}
``` 

## exemplo  : is_palyndrome

```rs
impl Solution {
    pub fn is_palindrome(s: String) -> bool {
        let mut myS = String::from("");
        for c in s.chars() {
            if (c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z') || (c >= '0' && c <= '9') {
                myS.push(c.to_ascii_lowercase());
            }
        }
        return myS == myS.chars().rev().collect::<String>();
    }
}
```

# traits I : default behaviour, implementacao para structs particulares 

## Primeiro exemplo 

```rs
// Definição do trait com implementação default
trait Falar {
    fn falar(&self) -> String {
        String::from("<este animal não sabe falar>")
    }
}

// Structs
struct Pessoa {
    nome: String,
}

struct Cao {
    nome: String,
}

struct Inseto;

// Implementações do trait

// Pessoa com comportamento customizado
impl Falar for Pessoa {
    fn falar(&self) -> String {
        format!("Olá, eu sou a pessoa {}", self.nome)
    }
}

// Cão com comportamento customizado
impl Falar for Cao {
    fn falar(&self) -> String {
        format!("{} diz: Au au!", self.nome)
    }
}

// Inseto usa o default (não implementa falar)
impl Falar for Inseto {}

fn main() {
    let p = Pessoa { nome: String::from("Ana") };
    let c = Cao { nome: String::from("Rex") };
    let i = Inseto;

    println!("{}", p.falar());
    println!("{}", c.falar());
    println!("{}", i.falar());
}
```

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

# hashMap

## exemplo 1 : Contar elementos de items

```rs
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();

    let items = vec!["a", "b", "a", "c", "a", "b"];

    for item in items {
        *map.entry(item).or_insert(0) += 1;
    }

    println!("{:?}", map);
}
```

## exemplo 2.1 : elemento com mais aparições (count hashMap) 
```rs
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();

    let items = vec!["a", "b", "a", "c", "a", "b"];

    for item in items {
        *map.entry(item).or_insert(0) += 1;
    }

    let max = map.iter().max_by_key(|&(_, count)| count);

    if let Some((key, count)) = max {
        println!("Mais frequente: {} ({})", key, count);
    }
}
```

## exemplo 2.2 : atenção ao output de map.iter()...

```rs
use std::collections::HashMap;

fn main() {
    let nums = vec![1, 2, 1, 3, 1, 2];

    let mut map = HashMap::new();

    // Contar frequências
    for n in nums {
        *map.entry(n).or_insert(0) += 1;
    }

    println!("Mapa: {:?}", map);

    // Encontrar o elemento com mais ocorrências
    let max = map.iter().max_by_key(|&(_, count)| count);

    match max {
        // ⚠️ Aqui está o ponto importante
        Some((&key, &count)) => {
            println!("Mais frequente: {} ({} vezes)", key, count);
        }
        None => {
            println!("Mapa vazio");
        }
    }
}
```

```
map.iter() devolve - (&i32, &i32) 

max_by_key() devolve - Option<(&i32, &i32)>

Ou seja para aceder precisas de: Some((&key, &count))
```

# borrow() 

## ex1 is same tree (is_same_tree) 

```rs
use std::rc::Rc;
use std::cell::RefCell;

impl Solution {
    pub fn is_same_tree(
        p: Option<Rc<RefCell<TreeNode>>>, 
        q: Option<Rc<RefCell<TreeNode>>>
    ) -> bool {
        match (p, q) {
            (None, None) => true,
            (Some(p_node), Some(q_node)) => {
                let p_ref = p_node.borrow();
                let q_ref = q_node.borrow();

                p_ref.val == q_ref.val
                    && Self::is_same_tree(p_ref.left.clone(), q_ref.left.clone())
                    && Self::is_same_tree(p_ref.right.clone(), q_ref.right.clone())
            }
            _ => false,
        }
    }
}
```

## ex2 : borrow() : Aceder a dados dentro de RefCell

```
use std::cell::RefCell;

fn main() {
    let x = RefCell::new(10);

    let v = x.borrow();   // empréstimo imutável
    println!("{}", *v);   // 10
}
```



## ex3: borrow_mut() : Acesso mutável ao valor dentro de RefCell

```rs
use std::cell::RefCell;

fn main() {
    let x = RefCell::new(10);

    {
        let mut v = x.borrow_mut(); // empréstimo mutável
        *v += 5;
    } // o borrow termina aqui

    println!("{}", x.borrow()); // 15
}
```

## ex4: chamadas recursivas tradicionais (função solta) 

```rs
fn factorial(n: i32) -> i32 {
    if n <= 1 { 1 } else { n * factorial(n - 1) }
}
```

## ex5: chamadas recursivas dentro de impl

```rs
impl Solution {
    pub fn is_same_tree(...) -> bool {
        Self::is_same_tree(...) // necessário
    }
}
```
# Rc<T> Reference Counted pointer 

```rs
use std::rc::Rc;

fn main() {
    let a = Rc::new(String::from("ola"));

    let b = Rc::clone(&a);
    let c = Rc::clone(&a);

    println!("{}", a);
    println!("{}", b);
    println!("{}", c);
}

// Semelhante a shared_ptr do c++
// Mas não é thread safety, é mais explicito e seguro
```




