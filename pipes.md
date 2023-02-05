# RFC 001: Pipes in jam Minecraft Server Software

**Authors**: [pcranaway](https://github.com/pcranaway)

**Status**: Proposed

## Introduction
This RFC outlines a proposal for the introduction of Pipes in jam. Pipes are
designed to provide a more efficient and flexible approach to processing
packets on the plugin-level.

## Pipe
A Pipe is a function that operates on a *Context* object, which contains two
primary fields: a *Packet*, which is an immutable representation of the data sent
from a client to the server, and a *State* object, which is used to store and
maintain the state of each packet between pipes.

## Pipeline
Pipes are executed in a specific sequence, determined by the Pipeline. The
order of Pipes in the Pipeline is configurable in the server's configuration.

On an average server, there may be dozens or hundreds of pipes. It is not
considered wrong nor right for a plugin to have many pipes, instead of one pipe
that deals with anything, as this is specific to the use-case.

## Example Pipeline
For example, a Pipeline could consist of the following Pipes:
- `profile_loader`: loads the profile information of a player from a database
- `welcome_message`: sends a welcome message to the player, including data from their profile

In this case, when a packet is first parsed, `profile_loader` is ran with a
context containing the packet, and an empty state. After `profile_loader` is
done, it returns back the context, where the state is modified to contain the
profile of the player. After this, `welcome_message` is ran with the context
returned by `profile_loader` and it operates on the context.
