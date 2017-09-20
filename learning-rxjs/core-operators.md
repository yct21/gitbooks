# 操作符

RxJS 通过操作符来对数据流进行处理。操作符都是 pure function，对一个 stream 使用操作符
会返回一个新的 stream，而不会对原有的流产生影响。

> You can lift sequences of data of any size into an observable context, as well
> as data generated or emitted over time, with the goal of creating a unified
> programming model for any type of data source, static or dynamic.

## self-contained pipeline

对 Observable 使用多个操作符可以构成 Observable pipeline。一个不产生副作用的
pipeline 可以称为 self-contained pipeline。一个 self-contained pipeline 不能泄露
出任何能改变 pipeline 上下文的引用。

> It’s important to understand that when you create an observable, you’re
> creating an ecosystem or a bounded context. That ecosystem is a closed loop
> that begins with a subscription and ends with a disposal.
