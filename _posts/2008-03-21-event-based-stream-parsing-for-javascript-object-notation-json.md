---
layout: post
title: Event Based Stream Parsing for JavaScript
    Object Notation (JSON)
date: 2008-03-21
comments: false
---

{{ page.title }}
================

### Overview


I have been playing around with the idea of a stream based, sax like,
parsing API for JSON. In my mind it has a few very direct benefits. I
suspect that it would simplify implementing a parser for an already
simple syntax. It would also allow for parsing arbitrarily large
documents. In my case I need to return information to the user as
quickly as possible, I absolutely cannot wait for the entire document
to be parsed. This would solve that problem. All that said I seriously
doubt that I am the first one to think of this, though I can't find
any references to anything similar out there. If any of you have any
pointers let me know in the comments and I will reference them. I plan
to implement this in erlang and at least one other language. I will
post links as soon as that is done. Without further ado.

### Abstract

The JavaScript Object Notation (JSON) is defined in RFC 4627. This
document describes method of parsing JSON using an event driven
api. It is expect that details of implementation will change from
language to language. However, this document should present a uniform
approach to stream parsing of JSON

### Introduction

JavaScript Object Notation (JSON) is describe as a text format for
serialization of structured data. More commonly it is used as a
transport protocol for various network based applications. This
document describes an event based parsing mechanism for JSON data that
can be implemented in a simple and strait forward manner. Reading and
understanding [RFC 4627](http://www.ietf.org/rfc/rfc4627.txt) is a
prerequisite to reading and understanding this document.

### Description

The stream oriented parser produces a series of events that describe
the structure currently being parsed. These events are then consumed
by the calling application in some manner. The actual mechanism of
consumption will vary from language type to language type though a few
different types of APIs are discussed in the appendix of this
document.

JSON is composed of two types of data structures primitive and complex
types. The events for the two types of structures remain the same,
changing only in the description of the structure being described.

The events around primitive types are relatively simple and should be
implemented as simply as possible. Individual API definers will choose
how they want to implement it using the examples in the appendix as a
guide.

Primitive types in JSON are strings, numbers, booleans and null. These
are described within the event itself

    string = STRING_DATA(value)
    number = NUMBER_DATA(value)
    boolean = BOOLEAN_DATA(value)
    null = NULL_DATA(value)

Complex types in JSON are objects and arrays. These are represented by
a series of events that describe the object. Because they are complex
types their representation is much more complex then that of primitive
types. However, it allows the object to be consumed as it occurred in
the stream.



     object = OBJECT_BEGIN
        KEY(string_value)
        VALUE_BEGIN
            ... # recursive type description
        VALUE_END
             ... # arbitrary number of additional key/value pairs
      OBJECT_END

      array = ARRAY_BEGIN
         VALUE_BEGIN
            ... # recursive type description
          VALUE_END
      ARRAY_END


### Callback API Description for Erlang

This would be implemented as a behavior that defines these
callbacks. Client code that wishes to receive these callbacks would
implement these methods. This should allow a high degree of
flexibility for the client.


    %% should guard data will be one of the primitive types
    %% null will be represented by the atom null
    data(Value, State) -> State2
    object_begin(State) -> State2
    object_end(State) -> State2
    key(Value, State) -> State2
    value_begin(State) -> State2
    value_end(State) -> State2

    array_begin(State) -> State2
    array_end(State) -> State2
