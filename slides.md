---
# try also 'default' to start simple
theme: excali-slide
# some information about your slides (markdown enabled)
title: Intro to Rust
info: |
  ## An introduction to Rust development
# apply UnoCSS classes to the current slide
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 35min
---

# Intro to Rust

A crash course to using the Rust language, and why it is so awesome.

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# So what is Rust?

Rust is a memory-safe, typed, system level language that was developed to create stable and fast software.

- 📝 **Verbose** - Rust has a strong focus on verbosity, especially with error handling.
- 🏃‍♂️ **Fast** - Rust is a system level language that compiles to binary.
- 🧑‍💻 **Readable** - Rust has a very readable syntax that is quick to pick up.
- 📦 **Supported** - Rust has an amazing eco-system with tooling for lots of applications.
- ⛑️ **Memory-safe** - Rust uses a unique ownership model to manage memory in a much safer way.
- 📜 **Well-documented** - Rust has first-class documentation with detailed error handling.
  <br>
  <br>

Find the official docs [on the Rust site](https://doc.rust-lang.org/stable/book/)

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---

# Getting started

Getting up and running in the Rust ecosystem is super quick. The easiest way to get going is to install the language using homebrew:

```bash
brew install rust
```

Alternatievly you can install the language using cUrl:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

You can get going on your first project using:

```bash
cargo new project-name
```

This will give you the barebones structure of a Rust application in a folder of the same name as your project:

```bash
cd ./project-name
```

---

# The Project Structure

Rust compiles down to a binary, and so it uses a main function model. A new project would look like this:

```bash
.
├── Cargo.toml
└── src
    └── main.rs
```

When compiling code, Rust looks for `main.rs` and then compiles our code down from there. Opening `main.rs` you will see a very basic application:

```rust
fn main() {
    println!("Hello, world!");
}
```

Rust will begin execution of your compiled code from the main function, so you will start your development within that block.

---

# The Syntax

Rust has a very familar syntax to those who know C-syntax languages such as C++, C#, or even TypeScript. Some of the basics are:

```rust [main.rs] {all|1|3-7|6|9-13|all} twoslash
enum Role { Admin, Viewer }

struct User {
    name: String,
    age: i16,
    role: Role,
}

fn main() {
    let user_1 = User { name: String::from("John Doe"), age: 23, role: Role::Admin };

    println!("Hello {}", user_1.name);
}

```

<arrow v-click="[1, 2]" x1="450" y1="220" x2="265" y2="220" color="#953" width="2" arrowSize="1" />
<arrow v-click="[2, 3]" x1="450" y1="255" x2="195" y2="255" color="#953" width="2" arrowSize="1" />
<arrow v-click="[3, 4]" x1="450" y1="310" x2="195" y2="310" color="#953" width="2" arrowSize="1" />

<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---

# Using structs

Rust's concept of structs is equivelent to classes in other lanaguages. It is basically defining a structure for a set of data. On the previous slide you would have seen the `User` struct. This defined a very basic user object.

```rust
struct User {
    name: String,
    age: i16,
    role: Role,
}
```

Rust also alows us to define functions on structs using the `impl` keyword.

```rust
struct User {...}

impl User {
    // This creates a new User object much like the new keyword in other languages
    fn new(name: &str, age: i16, role: Role) -> Self {
        Self { name: String::from(name), age, role }
    }
}
```

---

# Understanding enums

The concepts of enums in Rust is a little different to other languages. Enums at their base level work the same way as you would expect:

```rust
enum Role {
    Admin,
    Viewer
}
```

Enums also allow you to assign data a data type (not a value) to a field. Let's say we wanted to have a specific permission string on the viewer type. This would look like this.

```rust
enum Role {
    Admin,
    Viewer(String)
}
```

This tells us that the enum value of `Viewer` is a bucket that holds a value of type `String`. An example of this is on the next page.


<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---

## (cont.) Understanding enums

For our user, we could constuct the object like this:

```rust
let user_1 = User { name: String::from("John Doe"), age: 23, role: Role::Viewer(String::from("read:orders")) };
```

Enums are one of the fundamental aspects of Rust and are in large part the reason for its safety and verbosity. Rust provides a really helpful tool for working with enums called `match`.

```rust
let role = Role::Viewer(String::from("read:orders");

match role {
    Role::Viewer(value) => println!("Role is Viewer with the {} permission", value),
    Role::Admin => println!("Role is Admin")
}
```

You can think of this much like a switch statement with one important difference. Rust forces you to handle all conditions (or explicitly not handle cases). What makes Rust so safe to work with is that Rust takes advantage of this under the hood. All of Rust's error handling works with enums and so you are forced to have to handle any possible error.

---

## (cont.) Understanding enums: no implicit errors

Below is an example comparing Rust to TypeScript (and many other lanaguages). One superpower of Rust is that is tries to mitigate how much human error can creep into software.

```ts
const result = await fetch('https://example.com').then(result => result.json());

console.log(result); // What is value of result here?
```

In the above case, result could be of the data type we expect but there could also have been an error. This poses a problem because if you haven't handled that case, then you have introduced a possible bug into your application. On the flipside, with Rust, you don't get implicit errors. A similar function would look like this:

```rust
// NOTE: fetch isn't a real function in Rust. This is just to demo
let result: Result<DataType, ErrorType> = fetch("https://example.com").await;

match result {
    Ok(data) => println!("Data fetched: {:?}", data),
    Err(e) => println!("An error occurred: {:?}", e),
}
```

Rust forces you to have to cater for both cases.

---

## (cont.) Understanding enums: null vs. Option

One thing that might catch you out is that Rust does not have a concept of null. This was intentionally done so that errors are explicit rather than implicit. Consider this example in typescript.

```ts
// getData(): UserData | null;
const result = getData(); 

console.log(result.name); // What is value of result here?
```

In the above case, result could be of the data type we expect but it could also be `null`. In this model we are assuming a positive result and have to remember to check for problems. In Rust, we assume that there is always a case where something could go wrong and we are forced to handle it.

```rust
// get_data(): Option<UserData>;
let result: Option<UserData> = get_data("https://example.com");

match result {
    Some(data) => println!("Data fetched for user: {}", data.name),
    None => println!("No data found"),
}
```

`Option` is just a special enum that is built into Rust and is equivelent to `null`.

---

# Safer doesn't mean "no bugs"

Often times Rust can be hailed as a bug-free lanaguage. While this has some merit, it is not true. Rust does a lot to help devlopers avoid bugs. However, you can still mess things up. One example is the Cloudflare incident that happen recently. The `Option` enum has a help method called `unwrap` that allows you to ignore a bad value. For example:

```rust
// get_data(): Option<UserData>;
let result: UserData = get_data("https://example.com").unwrap();

// NOTE: result could be empty (None) here, but we are ignoring that.
println!("Data fetched for user: {}", result.name);
```

This is exactly what Cloudflare had done when we had the outage the other day. They had unwrapped an error which cause the entire program to crash. Rust does not recommend using `unwrap` in production code so be careful if you are using it. Prefer using something like `unwrap_or` to provide default values.

[Read Cloudflare's article here](https://blog.cloudflare.com/18-november-2025-outage/#memory-preallocation)

---

# Ownership model

Rust has another very unique feature that makes it a lot safer to user than many other system level lanaguages. That is the ownership model. At a very high level, this means that each variable we define owns its value. Rust doesn't have a garbage collector like Java or C# but you also don't have to manually manage your memory like C or C++. This gives us the best of both worlds.

```rust
fn main() {
    let name = String::from("John Doe");
    println!("Name is {}", name); // This works
}
```

How Rust manages memory is that is keeps a value alive until it goes out of scope. For example:

```rust
fn main() {
    let name = String::from("John Doe");
    let my_name = name;
    
    // This now throws an error because the variable `name` no longer has ownership of its value.
    println!("Name is {}", name); 
}
```

---

## (cont.) Ownership model

The advantage here is that we don't need to remember to cleanup memory like in C but we don't have the inefficiency of a garbage collector like in Java. The downside is that you have to be mindful of this ownership model when writing code. 

Now, you might have picked up a concern here. What happens if you need to use a value but you don't want the original owner to lose ownership? Rust further works with the ownership model using a concept called borrowing. Borrowing is basically telling Rust that you are going to use a value, but it still belongs to the original variable it was assigned to. In this case, the variable is only discarded when the original variable goes out of scope. Borrowing is done using the `&` syntax.

```rust
fn main() {
    let name = String::from("John Doe");
    let my_name = &name;
    
    // No error now
    println!("My name is {}", my_name); 
    println!("Name is {}", name); 
}
```

There are some more examples on the next slide.

---

## (cont.) Ownership model


```rust
fn transform_name(name: String) -> String {
    format!("Foo {}", name)
}

fn main() {
    let name = String::from("John Doe");
    let my_name = transform_name(name);
    
    println!("My name is {}", my_name); // No error here
    println!("Name is {}", name); // This line gives an error because we gave ownership to the transform_name function
}
```


```rust
fn transform_name(name: &str) -> String {
    format!("Foo {}", name)
}

fn main() {
    let name = String::from("John Doe");
    let my_name = transform_name(&name);
    
    println!("My name is {}", my_name); // No error here
    println!("Name is {}", name); // Also no error here
}
```

---

# Learning resources

- [The Rust Handbook](https://doc.rust-lang.org/stable/book/title-page.html)
- [Rustlings](https://rustlings.rust-lang.org/)
