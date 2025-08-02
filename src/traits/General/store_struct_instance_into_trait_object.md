# [Store a struct instance into a trait object (Trait coercions)](https://doc.rust-lang.org/reference/type-coercions.html)

- &dyn Animal - Using references / Borrowed trait objects
```rust
trait Animal {
    fn describe(&self) -> String;
}

struct Dog;
struct Cat;

impl Animal for Dog {
    fn describe(&self) -> String{
        "I'm a dog".to_string()
    }
}

impl Animal for Cat {
    fn describe(&self) -> String{
        "I'm a cat".to_string()
    }
}

fn main() {
    println!("Using direct struct instantiation");
    // Dog{} - struct instantiation, here a temporary instance, & will take a reference 
    // &dyn Animal - (trait object) reference to a type which implements Animal trait
    // it enables runtime polymorphism / dynamic dispatch
    // `&dyn Animal = &Dog{}` is coercion from thin pointer to fat pointer with vtable
    let animal: &dyn Animal = &Dog{};
    println!("{}", animal.describe());
    let animal: &dyn Animal = &Cat{};
    println!("{}\n", animal.describe());
    
    println!("Using separate variables");
    let dog = Dog{};
    let cat = Cat{};
    let animal: &dyn Animal = &dog;
    // method calls are resolved via vtable
    println!("{}", animal.describe());
    let animal: &dyn Animal = &cat;
    println!("{}", animal.describe());
}
```

- Box<&dyn Animal> - Boxed References to Trait Objects
```rust
trait Animal {
    fn describe(&self) -> String;
}

struct Dog;
struct Cat;

impl Animal for Dog {
    fn describe(&self) -> String{
        "I'm a dog".to_string()
    }
}

impl Animal for Cat {
    fn describe(&self) -> String{
        "I'm a cat".to_string()
    }
}

fn main() {
    println!("Using direct struct instantiation");
    // Dog{} - struct instantiation, here a temporary instance
    // Box::new(Dog{}) - Will move the temporary instance on heap with its own address 
    // Box<dyn Anima> = Box::new(Dog{} - coercion from Box<Dog> (thin pointer) to Box<dyn Animal> (fat pointer with data pointer + vtable pointer)
    let animal: Box<dyn Animal> = Box::new(Dog{});
    println!("{}", animal.describe());
    
    // can also do via referenced trait objects
    // Box::new(&Cat{}) - boxes a reference to the temporary Cat instance (not the Cat itself)
    let animal: Box<&dyn Animal> = Box::new(&Cat{});
    println!("{}\n", animal.describe());

    println!("Using separate variables");
    let dog = Dog{};
    let cat = Cat{};
    let animal: Box<&dyn Animal> = Box::new(&dog);
    // method calls are resolved via vtable
    println!("{}", animal.describe());
    let animal: Box<&dyn Animal> = Box::new(&cat);
    println!("{}", animal.describe());
}
```