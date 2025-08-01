# [Drop trait](https://doc.rust-lang.org/std/ops/trait.Drop.html)

It's like a destructure in Java. Used for cleanup.

- **Auto called at end of scope (& in reverse order)**
```rust
struct User {name: String }

impl Drop for User {
    fn drop(&mut self) {
        println!("Drop called for {}", self.name);
    }
}

fn take_ownership(user: User) {
  println!("Inside take_ownership");
  // here, `take_ownership` will own the `user` instance
} // & it will be auto freed at end of it's scope


fn main() {
    let user1 = User { name: "Alex".to_string() };
    let user2 = User { name: "Jones".to_string() };

    let user3 = User { name: "Martin".to_string() };
    take_ownership(user3);
}
```

- **On variable reassignment**  
Doesn't matter even if User struct contains only single non-owned type. Drop will be called, since User **is** a `struct`
  ```rust
        struct UserWithAgeOnly {
          age: u32,
      }
        
      impl Drop for UserWithAgeOnly {
          fn drop(&mut self) {
              println!("Drop called for age: {}", self.age);
          }
      }
        
      fn main() {
          let mut user = UserWithAgeOnly { age: 30 };
          // `Drop` called for "age:30"
          user = UserWithAgeOnly { age: 40 };
      } // `Drop` auto called for "age:40" at end of main()
  ```      

- **For methods like .pop(), .remove(), clear(), ...** 
```rust
struct User {
    name: String,
}

impl Drop for User {
    fn drop(&mut self) {
        println!("Drop called for {}", self.name);
    }
}

fn get_users() -> Vec<User> {
    vec![
        User {
            name: "Alex".to_string(),
        },
        User {
            name: "Jones".to_string(),
        },
        User {
            name: "Martin".to_string(),
        },
    ]
}

fn main() {
    let mut users = get_users();
    users.clear();
    println!();
    
    let mut users = get_users();
    users.remove(2);
    println!("Inside main()");
}
```
**- & on explicit `std::mem::drop()` call**
```rust
struct User {
    name: String,
}

impl Drop for User {
    fn drop(&mut self) {
        println!("Drop called for {}", self.name);
    }
}

fn main() {
    let user = User {
        name: "Alex".to_string(),
    };
    
    std::mem::drop(user);
    println!("Inside main()");
}
```

### How to prevent aut calling?
Ether use [std::mem::forget](https://doc.rust-lang.org/std/mem/fn.forget.html) or [ManuallyDrop](https://doc.rust-lang.org/std/mem/struct.ManuallyDrop.html)