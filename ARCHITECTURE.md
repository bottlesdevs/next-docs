# Architecture
Bottles is a project that aims to run Windows applications on Linux and macOS using Wine.

This document is an overview of the architecture of the project and how the components interact with each other, it aims to lower the entry barrier for new contributors and help them understand the project's structure.

## Components
We decided to split the project into multiple components to make it easier to maintain and develop new features, each component has a specific role and interacts with other components to achieve the project's goal.

- **Client(s)**: Responsible for rendering the user interface and handling user interactions.
- **Server**: Responsible for handling requests from the client and interacting with the database.
- **Core**: Responsible for handling the logic of the application.
- **WineBridge**: Responsible for interacting with the Windows API within the Wine prefix.

# Overview
The following diagram shows the main components of the project and how they interact with each other.

![[resources/Diagram.png]]

Every component in this project is written in Rust, except for the Client, which can be written in any language that has [gRPC support](https://grpc.io/docs/languages/)

## Client
The client is a desktop application. The client communicates with the server using [gRPC](https://grpc.io/) calls.

If the client is written in Rust, the option to call the `core` library directly is available.

## Server
The server is a [gRPC](https://grpc.io/) server that handles requests from the client.

The server talks to WineBridge to interact with the Windows API.

- Uses gRPC for communication between the client and the server.
- Uses Tonic as a gRPC framework.

## Core
The core is a library that contains the logic of the application. The core is used by the server to handle requests from the client.

## WineBridge
WineBridge is a server that runs within the Wine prefix and interacts with the Windows API.
It listens for requests from the server via [Named Pipes](https://learn.microsoft.com/en-us/windows/win32/ipc/named-pipes) and forwards them to the corresponding Windows API calls.

- Uses [windows-rs](https://crates.io/crates/windows) to interact with the Windows API.
