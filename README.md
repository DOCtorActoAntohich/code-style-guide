# C++ Code style guide

This code style guide draft is made by DOCtorActoAntohich for personal use.

This document assumes you know the basics of programming and are familiar with basic concepts of C++.

This document may and should be expanded to cover more cases.

Possible changes:
* Snake case for type names instead of Pascal case.
* Alignment rules when many function parameters go beyond line length limit.
* Alignment rules for operators and long lines.

Important note: think about optimization in two cases only: when you see that you use the language wrong (e.g. forgot to pass by reference), or when the code executes really slow.

## Table of contents

- C++ Code style guide
  - [Glossary](#glossary)
    - Naming cases
    - C++ related
    - Principles
  - [Formatting](#formatting)
    - Maximum line length
    - What to use
    - Indentation size
    - Empty lines
    - Alignments
  - [Variables](#variables)
    - General rules
    - Global variables
    - Local variables
    - Constants
    - Arrays
    - Boolean variables
  - [Operators](#operators)
    - Code style
    - Function-like operators `sizeof`, `typeof`/`typeid`
    - References and pointers
    - `goto`
    - Operator overloading
  - [If-else statements](#if-else-statements)
    - Code style
    - Condition
    - Example
  - [Loops](#loops)
    - `for` loops
    - `while` loops
    - `do-while` loops
  - [Namespaces](#namespaces)
    - Names and structure
    - Code style
    - Global namespace
    - Anonymous namespaces
    - Example
  - [Classes](#classes)
    - Structures
    - Naming
    - Code style
    - Example
  - [Types](#types)
    - Recommendations
    - Type aliases
  - [`using`](#using)
  - [Functions and methods](#functions-and-methods)
    - Naming
    - Code style
    - Many parameters
    - Lambdas
    - Example
  - [Templates](#templates)
    - `template`
    - Variadic templates
  - [Enums](#enums)
    - Naming
    - Enum constants
    - Code style
    - Example
  - [Pointers](#pointers)
    - Main rules
    - When you can use pointers
    - How to use pointers
  - [Exceptions](#exceptions)
    - Recommendations
  - [Preprocessor](#preprocessor)
    - `#define`
    - `#include`
    - Include guards vs `#pragma once`
  - [SFINAE](#sfinae)
  - [Project structure, files, and directories](#project-structure-files-and-directories)
    - Project structure template
    - Header files
    - Source files
    - Other
    - If the project is intended for use in other projects
    - If the project is not intended for use in other projects (i.e. not a library)
  - [Relative path vs absolute path; Include path](#relative-path-vs-absolute-path-include-path)

## Glossary

### Naming cases
* Pascal case: `LongNameInPascalCase`
* Snake case: `long_name_in_snake_case`
* Constant case: `LONG_NAME_IN_CONSTANT_CASE`

### C++ related
* Header files - files that contain declarations of objects (i.e. what it has).
* Source, or Implementation files - files that contain definitions of objects (i.e. how it works).

### Principles
* [RAII](https://en.cppreference.com/w/cpp/language/raii) - Resource Acquisition Is Initialization - a technique that binds an object's lifetime to some resource that must be acquired before use and freed after that.
* [SFINAE](https://en.cppreference.com/w/cpp/language/sfinae) - Substifution Failure Is Not An Error - principle that allows to abuse templates' direct substitution for compile-time checks.


## Formatting

It's recommended to display all whitespace characters to have a better control over code style.

### Maximum line length
- [ ] 80
- [ ] 100
- [X] 120 - avoid coding in terminal, and use modern tools instead.

### What to use
- [ ] Tabs
- [X] Spaces - enable a feature to replace tabs with spaces.

### Indentation size
- [ ] 2
- [X] 4 - balance is really important.
- [ ] 8

### Empty lines
- `1` in the following cases:
    - Between related code blocks (in one method, or in class declarations).
    - Between two highly related methods in implementation files.
    - Between different groups on `#include` directives.
- `2` in the following cases:
    - Between comments in the beginning of the file and `#include` directives.
    - Between `#include` directives and the actual code.
    - Between two classes in header files (this should be avoided though).
- `3` between methods implementations.

### Alignments
- For multiple related definitions, you may align them by the rightmost name.
- For multiple assignments, align them by the rightmost `=` operator, which may be tabbed too.

## Variables

### General rules
* Never declare and initialize more than one variable per line.
* Do not use any abbreviations, unless they are well-known: `http`, `dns`, `ftp`, etc.
* Provide full descriptive names for all the variables.
* Use short names like `i` and `k` only for indexes in loops.
* If you need to indicate that variable stores amount of something, you may add `n_` prefix, which means "number of": `n_players`, `n_apples`.

### Global variables
* Never use global variables.
* You may use global `constexpr` objects.

### Local variables
* Names of all local variables should be written in Snake case: `player_score`, `n_games_played`.
* Do not reuse one variable for multiple purposes.
* Never declare all local variables in the beginning of the method or any other block. Declare and initialize them right before the first time you use it.

### Constants
* Names for constants should be written in Constant case.
* Mark constants as `constexpr` when it applies.
* Mark input parameters for methods or functions as `const` to avoid accidental changes.

### Arrays
* Names for arrays and other containers that contain multiple objects must be written in plural: `requests`, `connected_players`, etc.
* If you need to define array contents in code, always put spaces around curly brackets, and break long lines by comma.
* Try to replace raw C-style arrays with `std::vector` or `std::array`.

Arrays (including the ones from STL) should be declared as follows:

```
int32_t array[] = {
    1,  2,  3,  4,  5,
    6,  7,  8,  9,  10,
    11, 12, 13, 14, 15
};
```

### Boolean variables
* Names of `bool` variables should resemble a question or a name of predicate, while the value of the variable should be an answer or a result of the predicate: `is_available`, `can_be_opened`, etc.
* If the variable acts as a boolean flag to, for example, mark the result of execution of some loop, then never call it `flag` but give it more descriptive name like `object_found`.

## Operators

### Code style
* Always put spaces around binary operators.
* Even though `,` comma is technically a binary operator, you should always put a space only to the right of it, and never to the left.
* Never put space between a unary operator and operand.
* If a line with several operators is too long, break the line after the binary operator so it becomes obvious to the reader that the line is not completed.
* It is not forbidden to use alternative bitwise and logical operators, but stay coherent with the rest of the code base.

### Function-like operators `sizeof`, `typeof`/`typeid`
* Never put spaces around brackets in function-like operators: `sizeof(something)`
* Avoid using `sizeof` in high-level code.
* For these operators, do not use type names, but use specific variables instead.

### References and pointers
* References and pointers are associated with the type, not with the name of the variable, so align `*` and `&` signs to the type name: `int32_t* some_pointer`, `const string& last_name`

### `goto`
* Avoid using `goto`, whatever it takes.

The only possible usage for `goto` is to have a centralized return point of C-style functions when, in case of errors, the data must be cleaned up. If you, for some reason, face such scenario, make wrappers around objects you have to clean up manually so that they follow RAII principle, and throw exceptions.

### Operator overloading
* You are allowed and encouraged to overload operators.
* However, it should be obvious what operators do, and should follow the common sense. If not then don't overload them.

## If-else statements

### Code style
* Always put curly brackets to define it-else blocks.
* The first bracket must be on the same line where condition ends, the second bracket must be on the new line.
* `else if` and `else` blocks must always start on new lines. Never put them right after closing bracket of previous block.
* Always indent the contents of `if-else` blocks.

### Condition
* If the condition line is too long, split is exactly the same as operators require.
* Is is recommended to put brackets to make it easier to understand the order of operations.
* When the condition is too hard (e.g. consists of 3 or more sub-checks), consider wrapping it into some `bool` variables right before the `if` statement

### Example

```cpp
// Warning: not pure C++ ahead, but just an illustrative example.
// This is fine, but may be quite hard to understand on the first glance:
if (str.has_digits_only() or (str.starts_with('-') and
    str.slice(str.begin() + 1, str.end()).has_digits_only())) {
    return convert_to_int(str);
}
else {
    throw NotANumberException();
}



// This block has more lines, but it's easier to debug,
// And may help to improvereadability
// (If not in this then in other cases).
bool is_positive_number = str.has_digits_only();
bool is_negative_number = str.starts_with('-') and
    str.slice(str.begin() + 1, str.end()).has_digits_only();
if (is_positive_number or is_negative_number) {
    return convert_to_int(str);
}
else {
    throw NotANumberException();
}
```

## Loops

### `for` loops
* Indexes in `for` loops must be declared in loop's brackets (in init statement), and not before the loop.
* When possible, avoid using indexes, and use iterators or "for-each style" loops with reference types.
* If either of 3 statements in `for` loop is missing, consider transforming it into `while` loop.
* Replace basic printing or search loops with `std::for_each` and `std::find`.
* Always put a space between `for` and brackets.
* Always put curly brackets to define the body of a loop.
* The opening bracket must be on the same line as `for` statement, the closing bracket must be on a new line.
* Always indent the contents of `for` loops.

```cpp
// Pretty good when you have to do something
// That depends on previous/next element.
for (int32_t i = 0; i < rectangles.size(); ++i) {
    draw_rectangle(rectangles[i]);
}

// Less error-prone and better because we killed indexes:
for (const auto& rectangle : rectangles) {
    draw_rectangle(rectangle);
}

// The best choice for such a simple case.
// Not always can be done though, but
// If you are a lambda's master, you might want to use that often:
std::for_each(rectangles.begin(), rectangles.end(), draw_rectangle);
```

### `while` loops
* For endless loops, use `while (true)` only, never use `while (1)` or `for (;;)`.
* If you want to exit the endless loop, try to make it gently by using `while (should_continue)` and `should_continue = false` lines. If for some reason that makes code harder to read, don't hesitate to use `break`.
* If `while` loop behaves like a `for` loop, convert it to `for` loop.
* Try to simplify the condition of `while` loop. For that, you may consider writing a small lambda function.
* Always put space between `while` and brackets (shown above).
* Always put curly brackets to define the body of `while` loops.
* The opening bracket must be on the same line as `while` statement, the closing bracket must be on a new line.
* Always indent the contents of `while` loop.

```cpp
while (!file.eof()) {
    int32_t n;
    file >> n;
    numbers.push_back(n);
}
```

### `do-while` loops
* They may usually be converted to `while` loops so you should try to do that when possible.
* Always put curly brackets to define the body of `do-while` loop.
* The opening bracket must be on the same line as `do` keyword, the closing bracket must start the new line.
* The `while` keyword must be on the same line as the closing curly bracket.
* Always put a space between `while` keyword and bracket with loop condition.

```cpp
do {
    should_continue = keyboard.get_signal();
} while (should_continue);
```

## Namespaces

### Names and structure
* Names for namespaces must be written in Snake case.
* The namespaces structure should coincide with your project's directory structure. Thus, name every directory exactly the same as the namespace.

### Code style
* Both curly brackets defining the body of namespace must lie on new lines.
* The body of the namespace must not be indented.
* It is allowed and recommended to compact nested namespaces into single line.

### Global namespace
* Never declare anything related to your project in global namespace.
* Instead, define a custom namespace for your project, and put everything there.
* This prevents global namespace from pollution.

### Anonymous namespaces
* If you don't want to pollute your project's namespace, and you have some local objects (classes or functions) that will be used only once to implement a single class, put these objects into an anonymous namespace.
* This anonymous namespace must be in the same implementation file where you need it.

### Example

```cpp
namespace my_project::utility
{
class CoolClass {
    // Contents
};



CoolClass create_object()
{
    return CoolClass();
}
}
```

## Classes

### Structures
* Avoid using structures for public API.
* Always use `class` instead of `struct`.
* You still may use `struct` only when you think combining some data into a single structure will help to simplify implementation of a class. This structure must have no methods, and should never be exposed to public API.

### Naming
* Names of classes should be written in Pascal case.
* Classes or other objects should represent an entity and thus have pure nouns in their name: `HTTPRequest`, `Level`, `Price`.
* Avoid verbal nouns when possible: `Parser`, `Helper`, etc.
* Names of private members (fields or methods) must start with prefix `m_` and follow variable naming rules: `m_position`, `m_get_some_local_data()`

### Code style
* A list of classes you inherit from must lie on the same line as `class` keyword. Try to avoid multiple inheritance when possible, and take caution measures when you cannot avoid it at all.
* The first curly bracket defining the body of a class must be on the same line as `class` keyword. Closing bracket must be on a new line.
* Try to avoid writing `public`, `protected`, or `private` keywords more than once and therefore mixing them in different orders.
* Never indent these 3 keywords themselves - they should be aligned to the `class` keyword.
* Always indent blocks they define, though.
* Define class fields and methods in exactly the same order: public fields/methods should go first, then protected, then private.
* Try to avoid using public fields for classes, and restrict access to fields you need to make public via getters and setters.

### Example

```cpp
class Level : public DataStorage {
public:
    using VectorF = std::vector<float_t>;
    Level();

    int get_blocks_amount() const;

protected:
    bool m_is_open() const noexcept;

private:
    size_t m_size;
};
```


## Types

### Recommendations
* Avoid using raw `int`, `unsigned int` and other types like them. Prefer using `int32_t`, `uint32_t` and other types defined in `<cstdint>`. Likely you won't have to `#include <cstdint>` if you have other Standard header included.
* Avoid using `char` for storing bytes, use `int8_t`, `uint8_t` or `std::byte`
* Use `char` only to interact with single symbols.
* Never use `char*` and `const char*` for strings, only use `std::string`. If C-style function requires C-style strings, make a wrapper, pass `std::string` and use `.c_str()`;

### Type aliases
* Always use `using` to make a type alias because it is more readable and flexible than `typedef`.
* Names of type aliases must follow the same rules as for classes.
* `using` declarations are not required to have `_t` suffix, and usually it is not advised.

## `using`
* Write `using` declaration only to make type aliases inside of class, but not in a global namespace or your project's namespace.
* Never write `using namespace X;` when you want to import a namespace unrelated to your project.
* Specifically, **NEVER** write `using namespace std;` **AT ALL**.
* In implementation files it is allowed to put `using namespace X;` line before you start implementing *your* class (where `X` is a namespace where *your* class is declared). It will help to avoid unnecessary verbosity.
* In methods, you may write `using <namespace::class_name>` to simplify some names. Still, don't do that to `std::something`.

## Functions and methods

### Naming
* Names of methods should be written in Snake case.
* Methods should induce action and thus contain verbs in names: `get_last_name`, `save_file`.
* If the method returns boolean value, the name should be a boolean predicate: `is_open`, `has_flag`, `can_save`, etc.

### Code style
* Never insert spaces around brackets.
* Each curly bracket that defines method's body must be on new line.
* Mark methods with `const` and `noexcept` where it applies.

### Many parameters
* Methods should not have more than 5 parameters.
* If you have 4-5 parameters, that's still acceptable, though you might want to find ways to simplify the signature.
* Anything that has more than 5 parameters must be refactored.
* When the line is too long, break it by comma, and indent parameters on new line(s) once.

### Lambdas
* It's recommended to use lambdas over callable structures.
* Same code style rules apply to lambdas.
* If you need a local lambda that will improve code readability and reduce code duplication, you may capture anything you want into it.
* If you have to return a lambda function somewhere, try to avoid capturing local and any other variables.

### Example

```cpp
FileDescriptor open_file(const std::filesystem::path& path)
{
    return FileDescriptor(path);
}


auto file = open_file("resources/config.jpg");
```

## Templates

### `template`
* Function signature or class description must start on a separate line from `template` keyword and list of template types.
* Always put a space between `template` and angle brackets.
* Avoid long lists of template types, and break them by commas.
* Prefer using `class` over `typename`.
* Names for template types should follow the same rules as for classes.

### Variadic templates
* The name for a variadic template type must be `Args`
* The name for function parameters of this type must be `args`.
* Put a space after, and never put a space before ellipsis in definitions, but not calls: `template <class... Args>`, `void f(Args... args)`, but `f(args...);`

## Enums

Always prefer using `enum class` over `enum` because `enum class` does not break scopes.

### Naming
* Names of enums must be written in Pascal case.
* Even though enums unite multiple constants, their names should **NOT** be in plural.

### Enum constants
* Enum constants should be named in Pascal case.
* The first constant(s) should be "default" or "error" value.
* Try to provide explicit values for constants when possible and/or needed, otherwise explicitly state only the first value.

### Code style
* Code style for enums is the same as code style for classes.

### Example

```cpp
enum class Direction {
    None = 0,

    Left,
    Right,
    Up,
    Down,
};



Direction d = Direction::Left;
```

## Pointers

### Main rules
* Try to never expose raw pointers to public API as they are error prone.
* In fact, try to avoid using raw pointers in the favor of smart pointers.
* Passing by pointer is not required anymore because you can and should (read as "must") pass by reference.
* Instead of dynamic arrays, use `std::vector` with `.reserve()` and `.emplace_back()`, or `std::array`.

### When you can use pointers
* To implement low-level stuff.
* To make wrappers for low-level stuff or C-style functions.
* To implement something for high-level API, though pointers should be completely hidden from

### How to use pointers
* Never use `NULL` to mark null pointers, use `nullptr` for that purpose.
* Never use `malloc` and `free`, use `new` and `delete`.
* Try to use constant pointers: `type* const name = nullptr;`. Don't confuse them with pointers to constants. This should save you from accidental pointer arithmetic.

## Exceptions
* You are allowed and encouraged to use exceptions when it's applicable, but still you have to be cautious.

### Recommendations
* Mark methods as `noexcept` when they don't throw exceptions.
* Don't throw primitive types (such as `int` or `const char*`), only use exception types.
* You may define your own exception types when you feel like standard library (or other library) does not provide you with enough tools.
* Always handle exceptions you throw.

## Preprocessor

### `#define`
* Never declare constants with `#define`, use `constexpr` instead.
* Try to avoid using macros in the favor of explicit inline methods and/or passing by reference.
* If you absolutely cannot avoid using macros and the destiny of the world depends on it, always take caution measures (at least extra brackets) and check what you have written thrice.
* Still, avoid writing macros that depend on code in local scopes.

### `#include`
* You may create small proxy files that include multiple files to include only one file instead of, say, 10, but make sure that proxy file includes only a group of related files.
* Use angle brackets for standard library and any other external *library* headers.
* Use quote signs for files from your project(s).
* You should include other files strictly in the following order:
    * The file you implement (only in source files).
    * Standard Library.
    * Any other external library.
    * Your other files.
* Leave an empty line between different groups of `#include` directives.

### Include guards vs `#pragma once`
* Use `#pragma once` instead of include guards.
* If you disagree with that, you may use include guards, but the name of constant you `#define` must consist of the path to the file, file name and the `INCLUDED` suffix, all written in Constant case: `PROJECT_NAMESPACE_SUBNAMESPACE_FILE_H_INCLUDED`. This should discourage you from using include guards, and also save from name collisions in extreme cases.

## SFINAE

If you need to do compile-time checks using SFINAE principles, for public API, follow the same style as per `std`.

## Project structure, files, and directories

### Project structure template
- `bin` or `out` - where build results go (must be in `.gitignore`)
- `lib` - where all the libraries required for this project lie (either `git submodule`s, or downloaded libraries with pre-built files).
- `include` - a folder with header files of this project.
- `src` - a folder with source files of this project.
- `res` - a folder with resources like textures, etc. Must be copied to where binaries lie after build is successfully finished.

### Header files
* Extension: `.h`
* Every header file should start with `#pragma once`
* One header file should describe only one object.
* Header files should have the same name as the object inside, but written in snake case.

### Source files
* Extension: `.cpp`
* The name of source file must be exactly the same as name of header file it implements.
* Every source file that implements header file must always include its header file in the first line, before any other `#include`s.
* `main.cpp` should contain `main` function (or other entry point). Parsing parameters with `argc` and `argv` is not mandatory if you don't need it.

### Other
* The name of directories and other files should be written in snake case.

### If the project is intended for use in other projects
* It is recommended to split source code into `src` and `include` folders for source and header files respectively.
* `include` directory should be added to additional include paths for the project.

### If the project is not intended for use in other projects (i.e. not a library)
* It is not necessary to split source code into 2 folders as in previous case.
* If not split, `src` folder should be added to include path.

## Relative path vs absolute path; Include path
* You are allowed to use relative paths only when you implement some header. In this case you can simply write `#include "header.h"`.
* In any other case you must use absolute paths for inclusion

For absolute paths, you need to add `include` or `src` directory to additional include path, and then you should write as follows:

```cpp
// Not like this:
#include "../../../../utils/log.h"

// But like this:
#include "utils/log.h"
```
