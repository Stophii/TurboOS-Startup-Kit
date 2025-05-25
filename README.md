# TurboOS-Startup-Kit

## Description

Learn how to easily integrate TurboOS to your Turbo Games! After following this walkthrough you'll have a project configured with the backend power of TurboOS!

>💡 **Tip** 💡 You can reach the TurboOS website [here](https://os.turbo.computer/)!


## Getting Set Up

Heading into your `Turbo.toml` file and making sure it has this line of code

```rust
[turbo-os]
api-url = "https://os.turbo.computer"
```

You can add it in like so:

```rust
name = "Dungeon Crashing"
version = "0.1.0"
description = "A procedurely generated dungeon crawler. Reach floor 40 and don't crash out!"
authors = ["Stophy", "Lucascarbone"]

[canvas]
width = 320
height = 160

[turbo-os]
api-url = "https://os.turbo.computer"
```

At the same time we can add a `const` to the top of our `lib.rs` right above `turbo::init` like this

```rust
pub const PROGRAM_ID: &str = "dungeon-crashing";
```
Feel free to name yours whatever, this is how it'll show up in the TurboOS Terminal!

## User ID and running TurboOS

Now that we've got the `turbo.toml` and `PROGRAM_ID` we can run our project like so 

```turbo run -w --user c5414b77-a2bb-461a-8ce5-cde71fccde0d --program-id dungeon-crashing```

>💡 **Tip** 💡 The `user_id` is the `c5414b77-a2bb-461a-8ce5-cde71fccde0d` but will be specific to you on your dashboard at [os.turbo.computer](https://os.turbo.computer/dashboard)!

Make sure to run it with **your** `user_id` and the specified `program-id` in this case it was `dungeon-crashing`!
