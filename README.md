前言

写本文的目的，是为了更好的理解promise，通过解读翻译原文，逐行解析原文通过代码一行一行实现。希望通过这篇文章，让我们能对promise有更深入的了解。

首先介绍promises是什么，英文的字面意思是“承诺”的意思，接下来promises翻译我没有用承诺翻译这个单词，因为我觉得有些英文只是一个词汇，还是直接用英文原文叫法好。

promise的是解决回调的问题的，通过then的链式调用，让我们能更清晰的理解阅读代码。下面直接看原文解读：

英: An open standard for sound, interoperable JavaScript promises—by implementers, for implementers.

中: 一个开源标准，可与JS互操作的Promises。由 implementers（语意创作这个promises的人或团体）创作，implementers（实现者）。

英: A promise represents the eventual result of an asynchronous operation. The primary way of interacting with a promise is through its then method, which registers callbacks to receive either a promise’s eventual value or the reason why the promise cannot be fulfilled.

中: 一个promise代表了异步操作的最终结果。与一个promise完成交互的主要方法是then方法，该方法会记录回调以接收一个promise的成功返回值或者失败的原因。

英: This specification details the behavior of the then method, providing an interoperable base which all Promises/A+ conformant promise implementations can be depended on to provide. As such, the specification should be considered very stable. Although the Promises/A+ organization may occasionally revise this specification with minor backward-compatible changes to address newly-discovered corner cases, we will integrate large or backward-incompatible changes only after careful consideration, discussion, and testing.

中: 本规范详细的描述了then方法的行为，提供了一个可互操作的基础，所有的Promises / A +符合promise实现都可依赖提供。因此，本规范应该被认为是非常稳定的。虽然Promises / A +组织偶尔会修改这个规范，但向后兼容更改次数较少，主要解决新发现的案例，我们只有在仔细考虑，讨论以及测试之后才会整合大的改动或者向后兼容的更改。

英: Historically, Promises/A+ clarifies the behavioral clauses of the earlier Promises/A proposal, extending it to cover de facto behaviors and omitting parts that are underspecified or problematic.

中: 历史上，Promises / A +阐明了先前Promises / A提案的行为条款，将其扩展为涵盖事实上的行为，并略去了未指定或存在问题的部分。

英: Finally, the core Promises/A+ specification does not deal with how to create, fulfill, or reject promises, choosing instead to focus on providing an interoperable then method. Future work in companion specifications may touch on these subjects.

中: 最后，核心Promises / A+规范不涉及如何创建，履行或拒绝promises，而是选择专注于提供可互操作的方法。未来的配套规范工作可能涉及这些主题。

英: 1.Terminology
中: 1、术语
英: 1.1 “promise” is an object or function with a then method whose behavior conforms to this specification.

中: 1.1 “promise”是一个对象或函数，它的行为符合这个规范。

英: 1.2 “thenable” is an object or function that defines a then method.

中: 1.2 “thenable”是定义then方法的对象或函数。

英: 1.3 “value” is any legal JavaScript value (including undefined, a thenable, or a promise).

中: 1.3 “value”是任何合法的JavaScript值（包括undefined，thenable或promise）。

英: 1.4 “exception” is a value that is thrown using the throw statement.

中: 1.4 “异常”是使用throw语句抛出的值。

英: 1.5 “reason” is a value that indicates why a promise was rejected.

中: 1.5 “原因”是一个值（结果）表明promise被拒绝的原因。

英: 2.Requirements
2.1.Promise States
中: 2、要求
2.1.Promise状态
英: A promise must be in one of three states: pending, fulfilled, or rejected.

中: 一个promise必须包含初始态, 成功（完成）态, 或者失败（拒绝）态这三个状态中的一种。

英: 2.1.1 When pending, a promise:
2.1.1.1 may transition to either the fulfilled or rejected state.

中: 2.1.1 当状态是初始态, promise:
2.1.1.1 可能转换到成功态或失败态。

英: 2.1.2. When fulfilled, a promise:
2.1.2.1. must not transition to any other state.
2.1.2.2. must have a value, which must not change.

中: 2.1.2 当状态是成功态，promise:
2.1.2.1 不能更改成别的状态。
2.1.2.2 必须有个不能更改的值（结果）

英: 2.1.3. When rejected, a promise:
2.1.3.1.must not transition to any other state.
2.1.3.2. must have a reason, which must not change.

中: 2.1.3 当状态是失败态，promise:
2.1.3.1. 不能更改成别的状态。
2.1.3.2. 必须有个不能更改的失败（错误）原因

英: Here, “must not change” means immutable identity (i.e. ===), but does not imply deep immutability.

中: 上面,“不能改变”的意思是不可改变的状态（即 ===），但并不意味着深不可变。

英: 2.2.The then Method
A promise must provide a then method to access its current or eventual value or reason.
A promise’s then method accepts two arguments:
promise.then(onFulfilled, onRejected)

中: 2.2 then方法
一个promise必须有一个then方法来获取成功的值（结果）或失败（错误）的原因。
一个promise方法接收两个参数：
promise.then(onFulfilled, onRejected)

英: 2.2.1. Both onFulfilled and onRejected are optional arguments:
2.2.1.1.If onFulfilled is not a function, it must be ignored.
2.2.1.2.If onRejected is not a function, it must be ignored.

中: 2.2.1. onFulfilled和onRejected都是可选参数：
2.2.1.1 如果onFulfilled不是函数，则必须忽略它。
2.2.1.2 如果onRejected不是函数，则必须忽略它。

英: 2.2.2 If onFulfilled is a function:
2.2.2.1.it must be called after promise is fulfilled, with promise’s value as its first argument.
2.2.2.2.it must not be called before promise is fulfilled.
2.2.2.3.it must not be called more than once.

中: 2.2.2 如果onFulfilled是一个函数：
2.2.2.1 必须在promise执行完成后调用，promise的返回值作为第一个参数。
2.2.2.2 在promise执行前不得调用。
2.2.2.3 只能调用一次。

英: 2.2.3.If onRejected is a function,
2.2.3.1.it must be called after promise is rejected, with promise’s reason as its first argument.
2.2.3.2.it must not be called before promise is rejected.
2.2.3.3.it must not be called more than once.

中: 2.2.3 如果onRejected是一个函数：
2.2.3.1 必须在promise执行完成后调用，promise的错误原因作为第一个参数。
2.2.3.2 在promise执行前不得调用。
2.2.3.3 只能调用一次。

英: 2.2.4.onFulfilled or onRejected must not be called until the execution context stack contains only platform code. [3.1].

中: 2.2.4 在执行上下文堆栈仅包含平台代码之前，不能调用onFulfilled或onRejected。[3.1]。

英: 2.2.5 onFulfilled and onRejected must be called as functions (i.e. with no this value). [3.2]

中: onFulfilled和onRejected必须是函数（即 没有这个值）。[3.2]

英: 2.2.6.then may be called multiple times on the same promise.
2.2.6.1.If/when promise is fulfilled, all respective onFulfilled callbacks must execute in the order of their originating calls to then.
2.2.6.2.If/when promise is rejected, all respective onRejected callbacks must execute in the order of their originating calls to then.

中: 2.2.6 then方法可能会在相同的promise被多次调用。
2.2.6.1 如果/当promise成功时，所有各自的onFulfilled回调必须按照其始发调用的顺序执行。
2.2.6.2 如果/当promise失败时，所有各自的onRejected回调必须按照其始发调用的顺序执行。

英: 2.2.7. then must return a promise [3.3].
promise2 = promise1.then(onFulfilled, onRejected);
2.2.7.1.If either onFulfilled or onRejected returns a value x, run the Promise Resolution Procedure [[Resolve]](promise2, x).
2.2.7.2.If either onFulfilled or onRejected throws an exception e, promise2 must be rejected with e as the reason.
2.2.7.3.If onFulfilled is not a function and promise1 is fulfilled, promise2 must be fulfilled with the same value as promise1.
2.2.7.4.If onRejected is not a function and promise1 is rejected, promise2 must be rejected with the same reason as promise1.

中: 2.2.7 then方法必须返回一个promise。[3.3]
例如：promise2 = promise1.then(onFulfilled, onRejected);
2.2.7.1 如果onFulfilled或onRejected返回一个值（结果）x，运行Promise的解决程序 [[Resolve]]（promise2，x）。
2.2.7.2 如果onFulfilled或onRejected引发异常e，promise2必须以e作为拒绝原因。
2.2.7.3 如果onFulfilled不是一个函数并且promise1是被成功，那么promise2必须用与promise1相同的值执行。
2.2.7.4 如果onRejected不是函数并且promise1是被失败，那么promise2必须用与promise1相同的失败原因。

英: 2.3.The Promise Resolution Procedure

中: 2.3 Promise的解决程序

英: The promise resolution procedure is an abstract operation taking as input a promise and a value, which we denote as [[Resolve]](promise, x). If x is a thenable, it attempts to make promise adopt the state of x, under the assumption that x behaves at least somewhat like a promise. Otherwise, it fulfills promise with the value x.

中: Promise的解决程序是一个抽象操作，取得输入的promise和一个值（结果），我们表示为 [[Resolve]](promise, x)。 [[Resolve]](promise, x)的意思是创建一个方法Resolve方法执行时传入两个参数promise和x（promise成功态时返回的值）。如果x是一个thenable（见上文术语1.2），它试图创造一个promise采用x的状态，假设x的行为至少貌似promise。否则，它用x值执行promise。

英: This treatment of thenables allows promise implementations to interoperate, as long as they expose a Promises/A+-compliant then method. It also allows Promises/A+ implementations to “assimilate” nonconformant implementations with reasonable then methods.

中: 对thenable的这种处理允许promise实现交互操作，只要它们暴露Promise / A +兼容的方法即可。 它还允许Promises / A +实现通过合理的方法“吸收”不合格的实现。

英: To run [[Resolve]](promise, x), perform the following steps:
2.3.1. if promise and x refer to the same object, reject promise with a TypeError as the reason.

中: 去运行 [[Resolve]](promise, x)，需执行以下步骤：
2.3.1 如果promise和x引用同一个对象，则以TypeError为原因拒绝promise。

英: 2.3.2. If x is a promise, adopt its state [3.4]:
2.3.2.1.If x is pending, promise must remain pending until x is fulfilled or rejected.
2.3.2.2.If/when x is fulfilled, fulfill promise with the same value.
2.3.2.3.If/when x is rejected, reject promise with the same reason.

中: 2.3.2 如果x是一个promise，采用它的状态【3.4】：
2.3.2.1 如果x是初始态，promise必须保持初始态(即递归执行这个解决程序)，直到x被成功或被失败。（即，直到resolve或者reject执行）
2.3.2.2 如果/当x被成功时，用相同的值（结果）履行promise。
2.3.2.3 如果/当x被失败时，用相同的错误原因履行promise。

英: 2.3.3.Otherwise, if x is an object or function,
2.3.3.1.Let then be x.then. [3.5]
2.3.3.2.If retrieving the property x.then results in a thrown exception e, reject promise with e as the reason.
2.3.3.3.If then is a function, call it with x as this, first argument resolvePromise, and second argument rejectPromise, where:
2.3.3.3.1.If/when resolvePromise is called with a value y, run [[Resolve]](promise, y).
2.3.3.3.2.If/when rejectPromise is called with a reason r, reject promise with r.
2.3.3.3.3.If both resolvePromise and rejectPromise are called, or multiple calls to the same argument are made, the first call takes precedence, and any further calls are ignored.
2.3.3.3.4.If calling then throws an exception e,
2.3.3.3.4.1.If resolvePromise or rejectPromise have been called, ignore it.
2.3.3.3.4.2.Otherwise, reject promise with e as the reason.
2.3.3.4.If then is not a function, fulfill promise with x.

中: 2.3.3 否则，如果x是一个对象或函数,
2.3.3.1 让then等于x.then。【3.5】
2.3.3.2 如果x.then导致抛出异常e，拒绝promise并用e作为失败原因。
2.3.3.3 如果then是一个函数，则使用x作为此参数调用它，第一个参数resolvePromise，第二个参数rejectPromise，其中：
2.3.3.3.1 如果使用值（结果）y调用resolvePromise，运行[[Resolve]]（promise，y）我的解决程序的名字是resolveExecutor。
2.3.3.3.2 如果使用拒绝原因r调用resolvePromise，运行reject(r)。
2.3.3.3.3 如果resolvePromise和rejectPromise都被调用，或者对同一个参数进行多次调用，则第一次调用优先，并且任何进一步的调用都会被忽略。
2.3.3.3.4 如果调用then方法抛出异常e，
2.3.3.3.4.1 如果resolvePromise或rejectPromise已经调用了，则忽略它。
2.3.3.3.4.2 否则，以e作为失败原因拒绝promise。
2.3.3.4 如果then不是一个对象或者函数，则用x作为值（结果）履行promise。

英: 2.3.4.If x is not an object or function, fulfill promise with x.
If a promise is resolved with a thenable that participates in a circular thenable chain, such that the recursive nature of [[Resolve]](promise, thenable) eventually causes [[Resolve]](promise, thenable) to be called again, following the above algorithm will lead to infinite recursion. Implementations are encouraged, but not required, to detect such recursion and reject promise with an informative TypeError as the reason. [3.6]

中: 2.3.4 如果x不是一个对象或函数，则用x作为值履行promise。
如果一个primse是通过一个thenable参与一个循环的可链接表达式来解决的thenable链，那么[[Resolve]]（promise，thenable）的递归性质最终会导致[[Resolve]]（promise，thenable）被再次调用， 上述算法将导致无限递归。 支持这种实现，但不是必需的，来检测这种递归并以一个信息性的TypeError为理由拒绝promise。【3.6】

英: 3.Notes
3.1.Here “platform code” means engine, environment, and promise implementation code. In practice, this requirement ensures that onFulfilled and onRejected execute asynchronously, after the event loop turn in which then is called, and with a fresh stack. This can be implemented with either a “macro-task” mechanism such as setTimeout or setImmediate, or with a “micro-task” mechanism such as MutationObserver or process.nextTick. Since the promise implementation is considered platform code, it may itself contain a task-scheduling queue or “trampoline” in which the handlers are called.
中: 3.注释
3.1 这里的“平台代码”是指引擎，环境和primise实现代码。在实践中，这个要求确保onFulfilled和onRejected异步执行，在事件循环开始之后then被调用，和一个新的堆栈。这可以使用诸如setTimeout或setImmediate之类的“宏任务”机制，或者使用诸如MutationObserver或process.nextTick的“微任务”机制来实现。由于promise实现被认为是经过深思熟虑的平台代码，因此它本身可能包含调用处理程序的任务调度队列或或称为“trampoline”（可重用的）的处理程序。
英: 3.2.That is, in strict mode this will be undefined inside of them; in sloppy mode, it will be the global object.

中: 3.2 即：在严格模式下是undefined；非严格模式下是全局对象。

英: 3.3.Implementations may allow promise2 === promise1, provided the implementation meets all requirements. Each implementation should document whether it can produce promise2 === promise1 and under what conditions.

中: 3.3 实现可能会允许promise2 === promise1, 前提是实现符合所有的要求。每个实现应该记录它是否可以产生promise2 === promise1以及在什么条件下。

英: 3.4.Generally, it will only be known that x is a true promise if it comes from the current implementation. This clause allows the use of implementation-specific means to adopt the state of known-conformant promises.

中: 3.4 通常，只有当它是来自当前的实现时才会知道x是一个真正的promise。该条款允许使用具体实现方法来采用已知符合性promise的状态。

英: 3.5.This procedure of first storing a reference to x.then, then testing that reference, and then calling that reference, avoids multiple accesses to the x.then property. Such precautions are important for ensuring consistency in the face of an accessor property, whose value could change between retrievals.

中: 3.5 首先存储对x.then的引用，然后测试和调用该引用，避免多次使用x.then属性。这样的注意事项对于确保访问器的属的一致性性非常重要，其值（结果）可能在取回时改变。

英: 3.6.Implementations should not set arbitrary limits on the depth of thenable chains, and assume that beyond that arbitrary limit the recursion will be infinite. Only true cycles should lead to a TypeError; if an infinite chain of distinct thenables is encountered, recursing forever is the correct behavior.

中: 3.6 实现不应该对thenable链做任意设定限制，假定超出该任意设定限制递归将是无限的。只有真正的循环才导致TypeError; 如果遇到不同的thenables的无限链，一直递归是正确的行为。

英: To the extent possible under law, the Promises/A+ organization has waived all copyright and related or neighboring rights to Promises/A+ Promise Specification. This work is published from: United States.

中: 在法律允许的范围内，Promises / A +组织已放弃所有版权及Promises / A +规范的相关或相邻权利。本作品发表于：美国。

---------以上是原文翻译