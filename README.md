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
name = "Turbo OS Startup Kit"
version = "0.1.0"
description = "Integrate Turbo OS Fast!"
authors = ["Stophy"]

[canvas]
width = 256
height = 144

[turbo-os]
api-url = "https://os.turbo.computer"
```

At the same time we can add a `const` to the top of our `lib.rs` right above `turbo::init` like this

```rust
//lib.rs

mod backend;
use backend::*;


pub const PROGRAM_ID: &str = "multiplayer-game"; //<--Add this!

turbo::init! {
    struct GameState {
        //stuff goes here


    } = {
        Self {
            //stuff goes here
        }
    }
}

turbo::go! {
    let mut state = GameState::load();

    state.save();
}
```
>💡 **Tip** 💡 Try not to overlap names, if that name exists it'll error out!

Make sure you come up with a different PROGRAM_ID than `mulitplayer-game`. This is also how it'll show up in the TurboOS Terminal!

## User ID and running TurboOS

At this point we'll need our `user_id` from the dashboard. Head over to your dashboard at [os.turbo.computer](https://os.turbo.computer/dashboard) It should look something like this

![Turbo Dashboard](https://github.com/user-attachments/assets/b779e041-c7ad-44df-ae99-1aa0d85d6528)

From here let's copy and paste the 2nd line in our dashboard

![Turbo Dashboard steps1](https://github.com/user-attachments/assets/d6289a6e-1064-4ddb-b0ab-b0ed82380b92)


and make some changes so it looks like this in our terminal

```
turbo run -w --user c5414b77-a2bb-461a-8ce5-cde71fccde0d --program-id multiplayer-game
```
>💡 **Tip** 💡 The `user_id` is the `c5414b77-a2bb-461a-8ce5-cde71fccde0d` but this will be specific to you on your dashboard at [os.turbo.computer](https://os.turbo.computer/dashboard) if you try to enter the same ID it won't work!

Then in the terminal it'll prompt you by asking you for the `one-time code`.

Which you'll get in your dashboard at [os.turbo.computer](https://os.turbo.computer/dashboard)!

![Turbo Dashboard steps2](https://github.com/user-attachments/assets/0a374bf0-1cde-48d8-9e13-a4554f4d3099)

If you don't get any errors in the terminal you should see your project open and run just like a normal turbo project!

## Backend and Frontend

Now that we have our project and were connected to TurboOS lets test it! I recommend making a `backend.rs` and we'll use that as the place to put our TurboOS specific functions.

To make a `backend.rs` just right click the src folder in Visual Studio Code and select `new file` and name it `backend.rs`

Now we'll add a test function to `backend.rs` which will look like this

```rust
//backend.rs

use crate::*;

#[unsafe(export_name = "turbo/test_counter")]
unsafe extern "C" fn on_increment_counter() -> usize {
    let userid = os::server::get_user_id();
    let file_path = format!("users/{}", userid);
    //read the current number from the file, or set it to 0 if it doesn't exist
    let mut counter = os::server::read_or!(u32, &file_path, 0);

    counter += 1;
		//write the updated file to the server
    let Ok(_) = os::server::write!(&file_path, counter) else {
        return os::server::CANCEL;
    };

    os::server::log!("length:{}", file_path.len());
    return os::server::COMMIT;
}
```
and on the frontend we'll need to call this function like this

```rust
    if gamepad(0).a.just_pressed() {
        os::client::exec(PROGRAM_ID, "test_counter", &[]);
        log!("a pressed!")
    }
```

which we can put inside our go loop like this

```rust
turbo::go! {
    let mut state = GameState::load();
    if gamepad(0).a.just_pressed() {
        os::client::exec(PROGRAM_ID, "test_counter", &[]);
        log!("a pressed!")
    }
    state.save();
}
```
Go ahead and run it and press the `z` key or `a` key on a controller and check your dashboard!

## Reading it on the Dashboard

If everything was done correctly, our turboOS dashboard should have something like this show up!

<img width="1127" alt="Screenshot 2025-05-26 at 7 25 51 PM" src="https://github.com/user-attachments/assets/d71246cc-8c3d-402a-b4a2-a6ae68766cca" />

and inside of it you'll see the functions being called and the ID of the player that caused that function to get called! A successful attempt will be green and a failed attempt will be red!

<img width="1144" alt="Screenshot 2025-05-26 at 7 28 34 PM" src="https://github.com/user-attachments/assets/464d906f-8645-479d-8715-539b9a9adc6a" />

Now you're setup with Turbo OS and can add in specific functions like multiplayer, leaderboards, or achievements! Whatever you decide to do I'll have a part two guide going over what some functions for those might look like! Thanks for reading!

