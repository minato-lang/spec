# minato

## Introduction
Minato is a general-purpose, procedural programming language.
It is designed for usage in client-side development, targeting both
web and mobile platforms while providing robust error handling.

The driving force behind Minato is to reduce the number of languages
a developer needs to learn to be productive on the client. For example,
a new web developer needs to learn HTML, CSS, and JavaScript at
minimum, whereas backend engineers can get away with just learning
one language, such as Python or Java.

As well, Minato is designed to be robust and safe, with a strong
emphasis on error handling. This is to reduce the number of runtime
errors that occur in client-side applications, which can be difficult
to debug and fix.

### Inspiration

Minato draws inspiration from the following languages and libraries:
- Zig
- Swift
- SwiftUI
- TypeScript
- Scala
- HTML
- CSS
- SASS

A big thank you to the creators of these languages and libraries for pioneering new ideas and concepts that have inspired Minato.

## Language Overview
### Comments

Comments are used to document code and are ignored by the compiler. Comments are denoted by `@@` at the beginning and end of the comment.
Comment syntax:
```
@@ This is a comment @@

@@ This is a multi-line comment
   that spans multiple lines @@
```

### Atoms

An atom is the smallest unit of a Minato program. The following are
atom types:
- `void` - Indicates that a function does not return a value. Variables of type `void` cannot be created
- `null` - A value that represents nothing
- `byte` - An 8-bit signed integer
- `ubyte` - An 8-bit unsigned integer
- `short` - A 16-bit signed integer
- `ushort` - A 16-bit unsigned integer
- `int` - A 32-bit signed integer
- `uint` - A 32-bit unsigned integer
- `long` - A 64-bit signed integer
- `ulong` - A 64-bit unsigned integer
- `float` - A 64-bit floating point number
- `bool` - A boolean value
- `char` - A unicode character
- `datetime` - A 64-bit Y2038-proof UNIX timestamp
- `fn` - A function

### Compounds

A compound is a collection of atoms. The following are compound types:
- `compound` - A collection of atoms
- `string` - A collection of characters
- `array` - A collection of objects
- `map` - A collection of key-value pairs
- `error` - A collection of error information, such as an error code and message

#### Defining a Compound

A compound is defined as follows:
```
compound compoundName {
  memberName: type;
  ...
}
```

### Variables

A variable is a named storage location that can hold a value. The variable syntax is as follows:
```
variableName: type = value;
```

### Functions

A function is a block of code that can be called. The function syntax is as follows:

```
functionName(argument: type, ...): returnType {
  functionBody
}
```

An example of a function in practice:
```
@@ Creates a post with the given title
   - title: The title of the post
@@
createPost(title: string): Post {
  return Post {
    title = title;
  }
}
```

A function may be called or used as follows:
```
post: !Post = createPost("Hello, World!");
```

A function's return type may be prefixed with `!` to indicate that the function may error. As well, a function's return type may be prefixed with `?` to indicate that the function may return `void`.

### Logic Operators

The following are logic operators:
- `==` - Equal to
- `!=` - Not equal to
- `>` - Greater than
- `<` - Less than
- `>=` - Greater than or equal to
- `<=` - Less than or equal to
- `&&` - Logical AND
- `||` - Logical OR
- `!` - Logical NOT

### Control Flow

The following are control flow statements:
- `if` - Executes a block of code if a condition is true
- `else` - Executes a block of code if the `if` condition is false. Maybe used in conjunction with `if` to form `else if`
- `guard` - Exits a function if a condition is false
- `throw` - Throws an error and exits the control flow
- `while` - Executes a block of code while a condition is true
- `for` - Executes a block of code for a specified number of iterations
- `break` - Exits a loop
- `continue` - Skips the current iteration of a loop
- `match` - Executes a block of code based on a value
- `return` - Exits a function and returns a value. Optionally, a function may return nothing 

Each of these is elaborated on in the following sections.

#### If

The `if` statement executes a block of code if a condition is true. The syntax is as follows:
```
if condition {
  codeBlock
}
```

#### Else

The `else` statement executes a block of code if the `if` condition is false. The syntax is as follows:
```
if condition {
  codeBlock
} else {
  codeBlock
}
```

As well, variables can be assigned based on the result of an `if/else` statement. The syntax is as follows:
```
variableName: type = if condition {
  value
} else {
  value
}
```

##### Else If

Else can be used in conjunction with `if` to form `else if`. The syntax is as follows:
```
if condition {
  codeBlock
} else if condition {
  codeBlock
} else {
  codeBlock
}
```

### Guard

The `guard` statement exits a function if a condition is false. The syntax is as follows:
```
guard condition else {
  codeBlock
}
```

Guard is useful for checking preconditions at the beginning of a function. If the condition is false, the function will exit and return nothing. It is recommended to use guard to unwrap `null`ish values. As well, it is recommended to throw an error if a guard condition is false.

### Error Handling

Minato has a strong emphasis on error handling. The following are error handling statements:

- `catch` - Handles an error that is thrown

Errors must be handled in Minato. If an error is thrown and not handled, the program will fail to compile.

The syntax for error handling is as follows:
```
statement catch error {
  codeBlock
}
```

An example of error handling in practice:
```
getPost(id: string): ?Post {
  p: Post = http.get("https://api.example.com/posts/${id}") catch {
    match error {
      HttpError => {
        print("An error occurred while fetching the post");
        return null;
      }
    }
  };
  return p;
}
```

## Appendix

### A Full Example

The following is a full example of a Minato program:
```
@@ Represents a blog post @@
compound Post {
  @@ The unique identifier of the post @@
  id: string;
  
  @@ The title of the post @@
  title: string;
  
  @@ The author of the post @@
  author: string;
  
  @@ The date the post was created @@
  date: datetime;
  
  @@ The filename of the post @@
  filename: string;

  @@ (OPTIONAL) The tags of the post @@
  tags: ?array<string>;
}

compound Blog {
  posts: ?array<Post>;
  owner: string;
}

createPost(title: string, author: string): Post {
  return Post {
    id = uuid();
    title = title;
    author = author;
    date = now();
    filename = "${title.lowercased()}.md";
  }
}

getPost(id: string): ?Post {
  p: Post = http.get("https://api.example.com/posts/${id}") catch {
    match error {
      HttpError => {
        print("An error occurred while fetching the post");
        return null;
      }
    }
  };
  return p;
}

getBlog(username: string): ?Blog {
  postIds: array<string> = http.get("https://api.example.com/blogs/${username}") catch {
    match error {
      HttpError => {
        print("An error occurred while fetching the blog");
        return null;
      }
    }
  };
  guard postIds.len > 0 else {
    return Blog {
      posts = null;
      owner = username;
    };
  }
  posts: array<Post> = postIds.map(getPost).filter((post: ?Post) { post != null });
  return Blog {
    posts = posts;
    owner = username;
  };
}
```

### Reserved Keywords

The following are reserved keywords in Minato:
- `void`
- `null`
- `byte`
- `ubyte`
- `short`
- `ushort`
- `int`
- `uint`
- `long`
- `ulong`
- `float`
- `bool`
- `char`
- `string`
- `array`
- `map`
- `compound`
- `error`
- `if`
- `else`
- `guard`
- `throw`
- `while`
- `for`
- `break`
- `continue`
- `match`
- `return`
- `catch`