# Python Conversation Flow by Syun Lee

This project contains a Python library for building conversational bots using a state machine architecture. The library provides building blocks to define states, transitions, trigger actions on transitions, and persist state for users.

Out of the box integrations are provided for Telegram and command line interfaces. Custom integrations can also be built by dispatching actions to the state machine.

## Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
- [State Machine Concepts](#state-machine-concepts)
- [Telegram Bot Integration](#telegram-bot-integration)
- [Command Line Interface](#command-line-interface)
- [Persistence](#persistence)
- [Extensibility](#extensibility)
- [Examples](#examples)
- [Contributing](#contributing)

## Overview

This library aims to simplify building conversational interfaces by providing a framework to:

- Define a state machine with states and transitions
- Trigger actions when transitioning between states
- Remember state on a per-user basis across conversations
- Integrate with platforms like Telegram or a CLI
- Enable easy debugging via visualizing the state machine

## Getting Started

To install:

```shell
pip install git+https://github.com/pysyun/pysyun_conversation_flow.git --upgrade
```

Import what you need:

```python
from pysyun.conversation.flow.dialog_state_machine import DialogStateMachineBuilder 
from pysyun.conversation.flow.telegram_bot import TelegramBot
```

## State Machine Concepts

The core concept is a state machine defined using a fluent builder API:

```python
machine = DialogStateMachineBuilder()
    .add(State0) 
    .to(State1, "input that triggers transition")
    .go("another input for this state", on_transition=my_action) 
    .build()
``` 

The machine keeps track of state per user. On transitions actions can be performed via passing a callback. At any point the state machine can be visualized by calling `machine.to_graphviz()`.

## Telegram Bot Integration

`TelegramBot` wraps the state machine and handles dispatching updates:

```python
bot = TelegramBot("token", initial_state)
bot.run() 
```

Helper methods are provided for sending messages or menus. Custom logic can also be wired in on state transitions.

## Command Line Interface

A CLI bot is provided that interfaces with the state machine:

```python
bot = ConsoleBot("token", initial_state)
bot.run()
```

## Persistence

Bots can use persistence to pickle state across restarts:

```python
PersistentTelegramBot("token", persistence_file="data.pickle") 
```

## Extensibility

The state machine can be integrated into any conversational application by dispatching user inputs to it. The updates just need to provide user ID and text body.

## Examples

See the [flows](./flows) folder for sample bots showcasing common conversational patterns.

## Contributing

Contributions welcome! Check out the [issues](https://github.com/pysyun/pysyun_conversation_flow/issues).
