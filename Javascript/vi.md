[Source](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0 "Permalink to Master the JavaScript Interview: What is Functional Programming?")

# Bậc thầy trong phỏn vấn về Javascript: Lập trình chức năng là gì?

![][1]

Structure Synth — Orihaus (CC BY 2.0)

> "Bậc thầy trong phỏng vấn về Javascript" là một loạt các bài viết được thiết kế để chuẩn bị cho các ứng viên về các câu hỏi phổ biến mà họ có khả năng gặp phải khi ứng tuyển vào vị trí senior JavaScript. Đây là những câu hỏi tôi thường xuyển sử dụng trong các cuộc phỏng vấn thực sự.

Lập trình hướng chức năng trở thành một chủ đề hot trong thế giới JavaScript. Chỉ vài năm trước, một số lập trình viên Javascript thậm chí không biết lập trình chức năng là gì, nhưng mỗi ứng dụng có codebase lớn tôi đã từng thấy trong 3 năm sử dụng nhiều ý tưởng lập trình chức năng.

**Lập trình hướng chức năng** (thường được viết tắt FP) là quá trình xây dựng phần mềm bằng cách viết **các hàm thuần**, tránh **chia sẻ trạng thái,** **dữ liệu thay đổi, **and **ảnh hưởng phụ**. Lập trình hướng chức năng là  **declarative** hơn là **imperative**, và luồng trạng thái ứng dụng thông qua các hàm thuần. Ngược lại với lập trình hướng đối tượng, nơi trạngt thái ứng dụng thường được chia sẻ và colocated với các phương thức trong các đối tượng.

Lập trình hướng chức năng là một **mô hình lập trình**, có nghĩa là đó là một cách suy nghĩ về kiến trúc phần mềm dựa trên một số nền tảng cơ bản, xác định theo các nguyên tắc (được liệt kê ở trên). Các ví dụ khác về các mô hình lập trình bao gồm lập trình hướng đối tượng và lập trình thủ tục.

Code theo hướng chức năng có xu hướng ngắn gọn hơn, dễ hiểu hơn, và dễ kiểm tra hơn là code theo hướng imperative hoặc hướng đối tượng - nhưng nếu bạn không quen với nó và các pattern phổ biến liên quan tới nó, code hướng chức năng cũng trông có vẻ dày đặc hơn nhiều, và các tài liệu liên quan có thể không hiểu được với người mới.

Nếu bạn bắt đầu If you start googling functional programming terms, you're going to quickly hit a brick wall of academic lingo that can be very intimidating for beginners. To say it has a learning curve is a serious understatement. But if you've been programming in JavaScript for a while, chances are good that you've used a lot of functional programming concepts & utilities in your real software.

> Don't let all the new words scare you away. It's a lot easier than it sounds.

The hardest part is wrapping your head around all the unfamiliar vocabulary. There are a lot of ideas in the innocent looking definition above which all need to be understood before you can begin to grasp the meaning of functional programming:

* Pure functions
* Function composition
* Avoid shared state
* Avoid mutating state
* Avoid side effects

In other words, if you want to know what functional programming means in practice, you have to start with an understanding of those core concepts.

A **pure function** is a function which:
* Given the same inputs, always returns the same output, and
* Has no side-effects

Pure functions have lots of properties that are important in functional programming, including **referential transparency** (you can replace a function call with its resulting value without changing the meaning of the program). Read ["What is a Pure Function?"][2] for more details.

**Function composition **is the process of combining two or more functions in order to produce a new function or perform some computation. For example, the composition `f . g` (the dot means "composed with") is equivalent to `f(g(x))` in JavaScript. Understanding function composition is an important step towards understanding how software is constructed using the functional programming. Read ["What is Function Composition?"][3] for more.

### Shared State

**Shared state** is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes. A shared scope can include global scope or closure scopes. Often, in object oriented programming, objects are shared between scopes by adding properties to other objects.

For example, a computer game might have a master game object, with characters and game items stored as properties owned by that object. Functional programming avoids shared state — instead relying on immutable data structures and pure calculations to derive new data from existing data. For more details on how functional software might handle application state, see ["10 Tips for Better Redux Architecture"][4].

The problem with shared state is that in order to understand the effects of a function, you have to know the entire history of every shared variable that the function uses or affects.

Imagine you have a user object which needs saving. Your `saveUser()` function makes a request to an API on the server. While that's happening, the user changes their profile picture with `updateAvatar()` and triggers another `saveUser()` request. On save, the server sends back a canonical user object that should replace whatever is in memory in order to sync up with changes that happen on the server or in response to other API calls.

Unfortunately, the second response gets received before the first response, so when the first (now outdated) response gets returned, the new profile pic gets wiped out in memory and replaced with the old one. This is an example of a race condition — a very common bug associated with shared state.

Another common problem associated with shared state is that changing the order in which functions are called can cause a cascade of failures because functions which act on shared state are timing dependent:

Timing dependency example

When you avoid shared state, the timing and order of function calls don't change the result of calling the function. With pure functions, given the same input, you'll always get the same output. This makes function calls completely independent of other function calls, which can radically simplify changes and refactoring. A change in one function, or the timing of a function call won't ripple out and break other parts of the program.

In the example above, we use `Object.assign()` and pass in an empty object as the first parameter to copy the properties of `x` instead of mutating it in place. In this case, it would have been equivalent to simply create a new object from scratch, without `Object.assign()`, but this is a common pattern in JavaScript to create copies of existing state instead of using mutations, which we demonstrated in the first example.

If you look closely at the `console.log()` statements in this example, you should notice something I've mentioned already: function composition. Recall from earlier, function composition looks like this: `f(g(x))`. In this case, we replace `f()` and `g()` with `x1()` and `x2()` for the composition: `x1 . x2`.

Of course, if you change the order of the composition, the output will change. Order of operations still matters. `f(g(x))` is not always equal to `g(f(x))`, but what doesn't matter anymore is what happens to variables outside the function — and that's a big deal. With impure functions, it's impossible to fully understand what a function does unless you know the entire history of every variable that the function uses or affects.

Remove function call timing dependency, and you eliminate an entire class of potential bugs.

### Immutability

An **immutable** object is an object that can't be modified after it's created. Conversely, a **mutable** object is any object which can be modified after it's created.

Immutability is a central concept of functional programming because without it, the data flow in your program is lossy. State history is abandoned, and strange bugs can creep into your software. For more on the significance of immutability, see ["The Dao of Immutability."][5]

In JavaScript, it's important not to confuse `const`, with immutability. `const` creates a variable name binding which can't be reassigned after creation. `const` does not create immutable objects. You can't change the object that the binding refers to, but you can still change the properties of the object, which means that bindings created with `const` are mutable, not immutable.

Immutable objects can't be changed at all. You can make a value truly immutable by deep freezing the object. JavaScript has a method that freezes an object one-level deep:

But frozen objects are only superficially immutable. For example, the following object is mutable:

As you can see, the top level primitive properties of a frozen object can't change, but any property which is also an object (including arrays, etc…) can still be mutated — so even frozen objects are not immutable unless you walk the whole object tree and freeze every object property.

In many functional programming languages, there are special immutable data structures called **trie data structures** (pronounced "tree") which are effectively deep frozen — meaning that no property can change, regardless of the level of the property in the object hierarchy.

Tries use **structural sharing** to share reference memory locations for all the parts of the object which are unchanged after an object has been copied by an operator, which uses less memory, and enables significant performance improvements for some kinds of operations.

For example, you can use identity comparisons at the root of an object tree for comparisons. If the identity is the same, you don't have to walk the whole tree checking for differences.

There are several libraries in JavaScript which take advantage of tries, including [Immutable.js][6] and [Mori][7].

I have experimented with both, and tend to use Immutable.js in large projects that require significant amounts of immutable state. For more on that, see ["10 Tips for Better Redux Architecture"][4].

### Side Effects

A side effect is any application state change that is observable outside the called function other than its return value. Side effects include:

* Modifying any external variable or object property (e.g., a global variable, or a variable in the parent function scope chain)
* Logging to the console
* Writing to the screen
* Writing to a file
* Writing to the network
* Triggering any external process
* Calling any other functions with side-effects

Side effects are mostly avoided in functional programming, which makes the effects of a program much easier to understand, and much easier to test.

Haskell and other functional languages frequently isolate and encapsulate side effects from pure functions using [**monads**][8]. The topic of monads is deep enough to write a book on, so we'll save that for later.

What you do need to know right now is that side-effect actions need to be isolated from the rest of your software. If you keep your side effects separate from the rest of your program logic, your software will be much easier to extend, refactor, debug, test, and maintain.

This is the reason that most front-end frameworks encourage users to manage state and component rendering in separate, loosely coupled modules.

### Reusability Through Higher Order Functions

Functional programming tends to reuse a common set of functional utilities to process data. Object oriented programming tends to colocate methods and data in objects. Those colocated methods can only operate on the type of data they were designed to operate on, and often only the data contained in that specific object instance.

In functional programming, any type of data is fair game. The same `map()` utility can map over objects, strings, numbers, or any other data type because it takes a function as an argument which appropriately handles the given data type. FP pulls off its generic utility trickery using **higher order functions**.

JavaScript has **first class functions**, which allows us to treat functions as data — assign them to variables, pass them to other functions, return them from functions, etc…

A **higher order function** is any function which takes a function as an argument, returns a function, or both. Higher order functions are often used to:
* Abstract or isolate actions, effects, or async flow control using callback functions, promises, monads, etc…
* Create utilities which can act on a wide variety of data types
* Partially apply a function to its arguments or create a curried function for the purpose of reuse or function composition
* Take a list of functions and return some composition of those input functions

#### Containers, Functors, Lists, and Streams

A functor is something that can be mapped over. In other words, it's a container which has an interface which can be used to apply a function to the values inside it. When you see the word functor, you should think "mappable".

Earlier we learned that the same `map()` utility can act on a variety of data types. It does that by lifting the mapping operation to work with a functor API. The important flow control operations used by `map()` take advantage of that interface. In the case of `Array.prototype.map()`, the container is an array, but other data structures can be functors, too — as long as they supply the mapping API.

Let's look at how `Array.prototype.map()` allows you to abstract the data type from the mapping utility to make `map()` usable with any data type. We'll create a simple `double()` mapping that simply multiplies any passed in values by 2:

What if we want to operate on targets in a game to double the number of points they award? All we have to do is make a subtle change to the `double()` function that we pass into `map()`, and everything still works:

The concept of using abstractions like functors & higher order functions in order to use generic utility functions to manipulate any number of different data types is important in functional programming. You'll see a similar concept applied in [all sorts of different ways][9].

> "A list expressed over time is a stream."

All you need to understand for now is that arrays and functors are not the only way this concept of containers and values in containers applies. For example, an array is just a list of things. A list expressed over time is a stream — so you can apply the same kinds of utilities to process streams of incoming events — something that you'll see a lot when you start building real software with FP.

### Declarative vs Imperative

Functional programming is a declarative paradigm, meaning that the program logic is expressed without explicitly describing the flow control.

**Imperative** programs spend lines of code describing the specific steps used to achieve the desired results — the **flow control: How** to do things.

**Declarative** programs abstract the flow control process, and instead spend lines of code describing the **data flow: What** to do. The _how_ gets abstracted away.

For example, this **imperative** mapping takes an array of numbers and returns a new array with each number multiplied by 2:

Imperative data mapping

This **declarative** mapping does the same thing, but abstracts the flow control away using the functional `Array.prototype.map()` utility, which allows you to more clearly express the flow of data:

**Imperative** code frequently utilizes statements. A **statement** is a piece of code which performs some action. Examples of commonly used statements include `for`, `if`, `switch`, `throw`, etc…

**Declarative** code relies more on expressions. An **expression** is a piece of code which evaluates to some value. Expressions are usually some combination of function calls, values, and operators which are evaluated to produce the resulting value.

These are all examples of expressions:
    
    
    2 * 2  
    doubleMap([2, 3, 4])  
    Math.max(4, 3, 2)

Usually in code, you'll see expressions being assigned to an identifier, returned from functions, or passed into a function. Before being assigned, returned, or passed, the expression is first evaluated, and the resulting value is used.

### Conclusion

Functional programming favors:

* Pure functions instead of shared state & side effects
* Immutability over mutable data
* Function composition over imperative flow control
* Lots of generic, reusable utilities that use higher order functions to act on many data types instead of methods that only operate on their colocated data
* Declarative rather than imperative code (what to do, rather than how to do it)
* Expressions over statements
* Containers & higher order functions over ad-hoc polymorphism

### Homework

Learn & practice this core group of functional array extras:

Use map to transform the following array of values into an array of item names:

Use filter to select the items where points are greater than or equal to 3:

Use reduce to sum the points:

#### Explore the Series

### Level Up Your Skills

[Learn JavaScript with Eric Elliott][10]. If you're not a member, you're missing out!

![][11]

[1]: https://cdn-images-1.medium.com/max/1600/1*1OxglOpkZHLITbIKEVCy2g.jpeg
[2]: https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976
[3]: https://medium.com/javascript-scene/master-the-javascript-interview-what-is-function-composition-20dfb109a1a0
[4]: https://medium.com/javascript-scene/10-tips-for-better-redux-architecture-69250425af44
[5]: https://medium.com/javascript-scene/the-dao-of-immutability-9f91a70c88cd
[6]: https://github.com/facebook/immutable-js
[7]: https://github.com/swannodette/mori
[8]: https://en.wikipedia.org/wiki/Monad_%28functional_programming%29
[9]: https://github.com/fantasyland/fantasy-land
[10]: http://ericelliottjs.com/product/lifetime-access-pass/
[11]: https://cdn-images-1.medium.com/max/1600/1*3njisYUeHOdyLCGZ8czt_w.jpeg
