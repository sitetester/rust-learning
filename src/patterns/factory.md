# Factory Pattern

The Factory pattern provides an interface for creating objects without specifying their exact class. In Rust, this is commonly implemented using enums and traits.

## Basic Factory Example

```rust
trait Animal {
    fn make_sound(&self);
}

struct Dog;
struct Cat;

impl Animal for Dog {
    fn make_sound(&self) {
        println!("Woof!");
    }
}

impl Animal for Cat {
    fn make_sound(&self) {
        println!("Meow!");
    }
}

enum AnimalType {
    Dog,
    Cat,
}

struct AnimalFactory;

impl AnimalFactory {
    fn create_animal(animal_type: AnimalType) -> Box<dyn Animal> {
        match animal_type {
            AnimalType::Dog => Box::new(Dog),
            AnimalType::Cat => Box::new(Cat),
        }
    }
}

fn main() {
    let dog = AnimalFactory::create_animal(AnimalType::Dog);
    let cat = AnimalFactory::create_animal(AnimalType::Cat);
    
    dog.make_sound(); // Woof!
    cat.make_sound(); // Meow!
}
```
