# Short term
- [ ] Make simple components render out static GTK views
```rust
fn FooComponent() -> reoxidized::Component {
  gtk!{
    <window>
      <button>
        Click me!
      </button>
    </window>
  }
}

fn main() {
  reoxidizedGtk::render(FooComponent); // Blocks
  // Will be changed to the following when component support is created
  // reoxidizedGtk::render(gtk! { <FooComponent /> });
}
```

- [ ] Support dynamic elements in gtk macro
```rust
fn FooComponent() -> reoxidized::Component {
  let value = "Hello";

  gtk!{
    <window>
      <button>
        {hello}
      </button>
    </window>
  }
}
```

- [ ] Support rendering a tree of components
```rust
fn Sidebar() -> reoxidized::Component {
  gtk!{
    <label>
      Sidebar
    </label>
  }
}

fn Main() -> reoxidized::Component {
  gtk!{
    <textView>
      Main content
    </textView>
  }
}

fn FooComponent() -> reoxidized::Component {
  gtk!{
    <window>
      <Sidebar />
      <Main />
    </window>
  }
}
```

- [ ] Support rendering `None` to render nothing
```rust
#[Component]
fn VoidComponent() -> reoxidized::Component {
  None
}
```

- [ ] Support passing "props" down to components
```rust
#[derive(Props)]
struct OtherComponentProps {
  hide: bool,
  title: String,
  content: String
}

fn OtherComponent(props: OtherComponentProps) -> reoxidized::Component {
  if props.hide { return None };

  gtk! {
    <label>
      {props.title}
      {props.content}
    </label>
  }
}

fn FooComponent() -> reoxidized::Component {
  let article = "The quick brown fox";

  gtk!{
    <window>
      <OtherComponent hide title="My awesome app" content={article} />
    </window>
  }
}
```

- [ ] Support components holding "state" triggering re-renders
```rust
#[Component]
fn FooComponent() -> reoxidized::Component {
  let (value, set_value) = use_state!("Click me!");

  gtk!{
    <window>
      <button clicked={|| { set_value("Clicked!")}}>
        {value}
      </button>
    </window>
  }
}
```

- [ ] Allow callbacks that are triggered when the provided arguments change
```rust
#[Component]
fn FooComponent() -> reoxidized::Component {
  let (value, set_value) = use_state!("This is the initial value");
  use_effect!(|| {
    set_value("This value is set after first render");
  }, vec![]);

  gtk!{
    <window>
      <label>
        {value}
      </label>
    </window>
  }
}
```

# Medium term
- [ ] Fragment support
```rust
fn FooComponent() -> reoxidized::Component {
  gtk!{
    <>
      <label>First label</label>
      <label>Second label</label>
    </>
  }
}
```

- [ ] Support "custom hooks"
```rust
fn use_input -> (i32, Closure) {
  let (value, set_value) = use_state!();
  let handler = |new_value: i32| {
    if new_value === "Hakuna" {
      set_value("Matata");
    } else {
      set_value(new_value);
    }
  };

  (value, handler)
}
fn FooComponent() -> reoxidized::Component {
  let (value, on_click) = use_input();
  gtk!{
    <window>
      <button clicked={|| { on_click("Hakuna") }}>
        {value}
      </button>
    </window>
  }
}
```

- [ ] Support context provider/consumer pairs
```rust
let some_context = reoxidized::Context::new();

fn OtherComponent() -> reoxidized::Component {
  let context_value = use_context!(some_context);

  gtk! {
    <label>
      {context_value}
    </label>
  }
}

fn FooComponent() -> reoxidized::Component {
  let some_value = 123;

  gtk!{
    <some_context.Provider value={123}>
      <window>
        <OtherComponent />
      </window>
    </some_context.Provider>
  }
}
```

- [ ] When everything "works", set up an RFC process

# Long term
- [ ] Provide devtools
- [ ] Provide test utils like "test rendering"
- [ ] Support QT rendering
- [ ] Support Win (UWP?) rendering
- [ ] Support Mac (Quartz?) rendering

