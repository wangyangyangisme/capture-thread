# Capture Thread Library

Framework for loggers, tracers, and mockers in multithreaded C++ programs.

## Motivation

This library solves the following problem: *Information needs to be passed
between different levels of implementation, but that information isn't an
integral part of the system's design.*

For example:

1.  Loggers that collect information about the inner workings of the system,
    e.g., performance and call information.
2.  Tracers that provide the inner workings of the system with context regarding
    the execution path taken to get to a certain call.
3.  Mockers that need to replace a real object with a mock object without being
    able to do so via dependency injection.

Instrumenting C++ code can be difficult and ugly, and once finished it can also
cause the original code to be difficult to maintain. Explicitly passing objects 
down from the top causes code to become unmaintainable, and using global static
data can cause serious complications in multithreaded environments when you only 
want to log, trace, or mock a certain call and not others.

This library solves those problems by tracking information per thread, but also
with the possibility of automatically passing information between threads.

## Using

This library contains a very simple framework for creating a logger, tracer, or
mocker that operates in the current thread. The library also contains simple
logic for sharing this context across threads.

The following steps are involved in instrumenting a project:

1.  Write a class to contain the information that will be shared. See
    [`simple.cc`](example/simple.cc) and [`threaded.cc`](example/threaded.cc)
    for the two primary ways of doing this.
2.  Update your implementation code to use the static API of your class to send
    or receive information.
3.  Where appropriate, instantiate your class to make logging, tracing, or
    mocking (whichever you implemented) available in that scope.
4.  If you need to work with multiple threads, e.g., tracing a call from one
    thread to another, wrap thread callbacks in `ThreadCrosser::WrapCall`.

See the [`example`](example) directory for examples of several types of
implementation. Unit tests in [`test`](test) also demonstrate a variety of
features and behavior.
