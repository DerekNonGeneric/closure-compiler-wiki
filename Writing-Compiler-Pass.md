# Introduction

This is a quick tutorial for anyone who is interested in developing a pass for the Closure Compiler. It can be an optimization or other Javascript analysis for educational purpose or just self amusement.

# Scope

Upon finishing this tutorial, you will be able write compiler passes to do code transformation and make use of some of the provided tools in the compiler to perform analysis on Javascript code.

# Prerequisites

Get the code by cloning the repository:

    git clone https://github.com/google/closure-compiler.git

Install Maven if it's not installed already, then make sure you can build and test the compiler:

    mvn install

# Adding Your Pass

Fire up your favorite editor and type (paste) the following code in src/com/google/javascript/jscomp/HelloWorld.java

```java
package com.google.javascript.jscomp;

import com.google.javascript.rhino.Node;

class HelloWorld implements CompilerPass {

  final AbstractCompiler compiler;
  
  public HelloWorld(AbstractCompiler compiler) {
    this.compiler = compiler;
  }

  @Override
  public void process(Node externs, Node root) {
    // We are going to add stuff here. 
  }
}
```

Any compiler passes should implement [`CompilerPass`](https://github.com/google/closure-compiler/blob/master/src/com/google/javascript/jscomp/CompilerPass.java) providing the `process()` method. The compiler will execute this method when the pass is scheduled to run. The Abstract Syntax Tree (AST) will be passed into our new pass. The externs tree contains all the externs definitions provided by the user with `--externs`. The root tree contains all the source code for the compilation.

# Traversing the AST

We can traverse the tree with our own recursion with the Node's `getFirstChild()`, `getLastChild()` and `getNext()` functions. However, the compiler has several traversal tools that simplify this process. We will use [`NodeTraversal`](https://github.com/google/closure-compiler/blob/master/src/com/google/javascript/jscomp/NodeTraversal.java).

```java
package com.google.javascript.jscomp;

import com.google.javascript.jscomp.NodeTraversal.AbstractPostOrderCallback;
import com.google.javascript.rhino.Node;

class HelloWorld implements CompilerPass {

  final AbstractCompiler compiler;
  
  public HelloWorld(AbstractCompiler compiler) {
    this.compiler = compiler;
  }
  
  @Override
  public void process(Node externs, Node root) {
    NodeTraversal.traverse(compiler, root, new Traversal());
  }
  
  private class Traversal extends AbstractPostOrderCallback {
    @Override
    public void visit(NodeTraversal t, Node n, Node parent) {
      // Do work here.
    }
  }
}
```

The NodeTraversal traverses the AST in a left-to-right post-order manner. Within the visit method we can specify what to do with each node as we visit it.

# Recognizing Code Patterns

We are going to ignore any node that is not a variable declaration.

```java
package com.google.javascript.jscomp;

import com.google.javascript.jscomp.NodeTraversal.AbstractPostOrderCallback;
import com.google.javascript.rhino.Node;

class HelloWorld implements CompilerPass {

  final AbstractCompiler compiler;
  
  public HelloWorld(AbstractCompiler compiler) {
    this.compiler = compiler;
  }
  
  @Override
  public void process(Node externs, Node root) {
    NodeTraversal.traverse(compiler, root, new Traversal());
  }
  
  private class Traversal extends AbstractPostOrderCallback {
    @Override
    public void visit(NodeTraversal t, Node n, Node parent) {
      if (!n.isVar()) {
        return;
      }
    }
  }
}
```

# Code Transformations

Now that we found our node we would like to insert our "Hello World"

```java
package com.google.javascript.jscomp;

import com.google.javascript.jscomp.NodeTraversal.AbstractPostOrderCallback;
import com.google.javascript.rhino.IR;
import com.google.javascript.rhino.Node;

class HelloWorld implements CompilerPass {

  final AbstractCompiler compiler;
  
  public HelloWorld(AbstractCompiler compiler) {
    this.compiler = compiler;
  }
  
  @Override
  public void process(Node externs, Node root) {
    NodeTraversal.traverse(compiler, root, new Traversal());
  }
  
  private class Traversal extends AbstractPostOrderCallback {
    @Override
    public void visit(NodeTraversal t, Node n, Node parent) {
      if (!n.isVar()) {
        return;
      }

      Node printHelloWorld = IR.call(
          IR.name("print"),
          IR.string("Hello World!"));
      
      Node statement = IR.exprResult(printHelloWorld);
      
      parent.addChildAfter(statement, n);
    }
  }
}
```

We construct a subtree for the code that calls print() with the argument "Hello World!". A call node's first child is the function being called (in this case, `print`) and the rest of the children (if any) are the arguments to the function. On top of that we wrap it in an "expr result" node to make the AST valid, and finally insert it after the variable declaration. The AST can be built up using `new Node()`, `Node#addChildToBack`, etc. but it is often easier to use the [`IR`](https://github.com/google/closure-compiler/blob/master/src/com/google/javascript/rhino/IR.java) methods as the code is easier to read and many of them do some extra checking to catch incorrect usage. To see what the AST for a given snippet of JavaScript looks like the [Closure Compiler Debugger](https://closure-compiler-debugger.appspot.com/) is often helpful.

# Executing the Pass

Let's say we want to execute our pass just before some optimization passes. To do so, we insert the following code in java/com/google/javascript/jscomp/DefaultPassConfig.java.

```java
    // Collapsing properties can undo constant inlining, so we do this before
    // the main optimization loop.
    if (options.getPropertyCollapseLevel() != PropertyCollapseLevel.NONE) {
      passes.add(collapseProperties);
    }
   
    ///////////////////  NEW CODE STARTS  //////////////////////////
    passes.add(
        PassFactory.builder()
            .setName("helloWorld")
            .setInternalFactory(HelloWorld::new)
            .setFeatureSet(FeatureSet.ES2019_MODULES)
            .build());
    ///////////////////  NEW CODE ENDS  ////////////////////////////
    
    // ReplaceStrings runs after CollapseProperties in order to simplify
    // pulling in values of constants defined in enums structures.
    if (!options.replaceStringsFunctionDescriptions.isEmpty()) {
      passes.add(replaceStrings);
    }
```

If your pass is a check and not an optimization, you can paste this code snippet in the getChecks() method in DefaultPassConfig.java

# Building

Adding this new compiler pass breaks some tests. So build the compiler again but skip running tests this time:

    mvn install -DskipTests

# Running

Let's load up our editor and write a quick some_script.js Javascript to see if it works.

```js
var print;
var x;
```

Run the compiler:

    java -jar target/closure-compiler-1.0-SNAPSHOT.jar --js=/tmp/some_script.js

and you should see

```js
var print;print("Hello World!");var x;print("Hello World!");
```

in the output. Your new pass has added a `print` call after each var statement.

# Advanced features

If you want your pass to do some more complicated analysis, it might need access to the control-flow-graph of a particular function. [TODO: Add information on how to get and use the CFG] Or, you might want to write a pass that takes advantage of type information. [TODO: Add a page showing how to get the types of Nodes and do something interesting with them]

# Example passes

This wiki page showed you a relatively simple compiler pass. Here are some examples of passes that are a bit more complex but we hope will be relatively easy to follow even if you're new to the Closure Compiler code base. Reading the unit tests for a pass (or writing your own, to see what the pass does with various snippets of JS) is a good way to understand what a pass is doing, so check out [the unit tests](https://github.com/google/closure-compiler/tree/master/test/com/google/javascript/jscomp) as well.

 * [CheckPrototypeProperties](https://github.com/google/closure-compiler/blob/master/src/com/google/javascript/jscomp/lint/CheckPrototypeProperties.java)
 * [PeepholeFoldConstants](https://github.com/google/closure-compiler/blob/master/src/com/google/javascript/jscomp/PeepholeFoldConstants.java)
 * [CheckSuspiciousCode](https://github.com/google/closure-compiler/blob/master/src/com/google/javascript/jscomp/CheckSuspiciousCode.java)
 * [Es6RewriteDestructuring](https://github.com/google/closure-compiler/blob/master/src/com/google/javascript/jscomp/Es6RewriteDestructuring.java)