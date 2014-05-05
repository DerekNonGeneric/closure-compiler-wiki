# @idGenerator, @consistentIdGenerator, and @stableIdGenerator documentation

## Introduction

ID generator annotations indicate that a function of type `{function(string):string}` will be used to produce short, unique string IDs. When compiled, invocations of such functions are directly replaced with the calculated IDs.  A common use case for this feature is creating obfuscated HTML IDs.

An example:

    /**
     * Gets an ID for the passed name.
     * @idGenerator
     * @param {string} myLongName The name to get an ID for.
     * @return {string} A short, unique ID that is consistent per input name.
     */
    foo.getId = function(myLongName) {
      return myLongName;
    };
    
    foo.myElement.id = foo.getId('my-super-duper-element');
    foo.myOtherElement.id = foo.getId('my-other-element');
    foo.mainId = foo.getId('my-super-duper-element');

When uncompiled, the bare long name is used as the ID.  When compiled though, the compiler replaces `getId` invocations with a string literal.  For example:

    foo.myElement.id = 'x0';
    foo.myOtherElement.id = 'x1';
    foo.mainId = 'x0';

## ID Generator Strategies

Three ID generation strategies are supported.

`@idGenerator` creates short names using a simple counter, as shown in the example above.  Since compiled IDs are dependent on invocation order, all `@idGenerator` invocations must be in the global scope and unconditional.  While `@idGenerator` is guaranteed to be an injective function, the exact IDs produced should be considered opaque and non-stable between separate compilations.

`@consistentIdGenerator` attempts to retrieve IDs from a mapping supplied to the compiler before falling back to the `@idGenerator` strategy.  

`@stableIdGenerator` constructs IDs using a deterministic hashing strategy, at the cost of slightly longer IDs than `@consistentIdGenerator` and `@idGenerator`.  Unlike `@consistentIdGenerator` and `@idGenerator`, its IDs are guaranteed to be stable across separate compilations.  Invocations may also be made in any context.

Only one of the ID generator annotations may be added to a particular function.

## Compiler Options

ID generator replacement is on by default, but may be toggled with the `CompilerOptions#setReplaceIdGenerators` method.

For debugging support, `CompilerOptions#setGeneratePseudoNames` may be set to make the transformed IDs human readable.

`CompilerOptions#setIdGeneratorsMap` may be set to supply a serialize map for use with `@consistentIdGenerator`.