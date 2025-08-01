# Factory Pattern

```rust
struct AnimalFactory;

impl AnimalFactory {
    fn create_animal(animal_type: AnimalType) -> Box<dyn Animal> {
        match animal_type {
            AnimalType::Dog => Box::new(Dog),
            AnimalType::Cat => Box::new(Cat)
        }
    }
}

trait Animal {
    fn describe(&self) -> String;
}

enum AnimalType { Dog, Cat }

struct Dog;
impl Animal for Dog {
    fn describe(&self) -> String {
        "I'm a Dog".to_string()
    }
}

struct Cat;
impl Animal for Cat {
    fn describe(&self) -> String {
        "I'm a Cat".to_string()
    }
}

fn main() {
    let animal = AnimalFactory::create_animal(AnimalType::Dog);
    println!("{}", animal.describe());

    let animal = AnimalFactory::create_animal(AnimalType::Cat);
    println!("{}", animal.describe());
}
```