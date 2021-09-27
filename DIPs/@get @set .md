# (Your DIP title)

| Field           | Value                                                           |
|-----------------|-----------------------------------------------------------------|
| DIP:            | (number/id -- assigned by DIP Manager)                          |
| Review Count:   | 0 (edited by DIP Manager)                                       |
| Author:         | (your name and contact data)                                    |
| Implementation: | (links to implementation PR if any)                             |
| Status:         | Will be set by the DIP manager (e.g. "Approved" or "Rejected")  |

## Abstract

depercating ```d @property``` and introducing ```d @get @set``` so that properties can be useful again.


## Contents
* [Rationale](#rationale)
* [Prior Work](#prior-work)
* [Description](#description)
* [Breaking Changes and Deprecations](#breaking-changes-and-deprecations)
* [Reference](#reference)
* [Copyright & License](#copyright--license)
* [Reviews](#reviews)

## Rationale

```d @Property``` suffers from the following issues:
* It doesn't support binary/unary operators
* Requires two parentheses to call a delegate function
* Next to useless due to the capabilities of the CTFE(Compile time function engine)
* Verbage that is unneeded when defining the set and get functions.
* Current definition of ```d @Property``` is consider to be uncertain by the spec


## Prior Work
https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties
https://docs.python.org/3/howto/descriptor.html#properties
https://en.wikipedia.org/wiki/Mutator_method

## Description

This DIP proposes that the attribute ```d @Property``` to be depercated and replaced by ```d @get @set``` as accessors (Or mutator methods).
Accessors are functions that handle values that are either being returned or being modified by a new value that is being passed to the function.
Accessors for ```d @get ``` can only return rvalues as the returning/modfiying the values are being handle by the functions, thus cannot be passed by reference to other functions.
Cannot obtain the reference of the Accessor functions directly without using the ```__traits(@set, exp)``` or ```__traits(@get, exp)``` functions.
Accessors for ```d @set``` can only have zero Parameter and must return a non-void type
Accessors for ```d @set``` can only have one non-default parameter and must return a void type
Accessors can not be overloaded by non-Accessors that share the same name
Accessors can not have variadic parameters.
Calling Accessors functions can only have parentheses when calling a delegate function
The return type of the ```d @get ``` accessor must be the same exact type as the paramater of the ```d @set``` accessor if they share the same exact name
Accessors can be marked as ```d @nogc``` ```d pure``` ```d nothrow``` ```d safe``` ```d throw```
```d @get @set``` can be used to extend the C/C++ structs/classes methods to behave as Accessors

```d 

	private char[] name;
	private int age;
	public char[] Name {
        @get { return name; }
        @set { name = _value; } // Introduce _value to prevent accidental breakage
    }
	extern (C++) @set void foo(int value);// Treat the extended C++ linkage functions as Accessors
	extern (C++) @get int foo();
	extern (C++) @get("Number") int getNumber()
	extern (C++) @set("Number") void setNumber(int value)
	```


## Breaking Changes and Deprecations
Functions that are marked with ```d @Property``` are to be depercated as currently it serves no use,


## Reference
https://dlang.org/spec/function#property-functions

## Copyright & License
Copyright (c) 2021 by the D Language Foundation

Licensed under [Creative Commons Zero 1.0](https://creativecommons.org/publicdomain/zero/1.0/legalcode.txt)

## Reviews
The DIP Manager will supplement this section with a summary of each review stage
of the DIP process beyond the Draft Review.