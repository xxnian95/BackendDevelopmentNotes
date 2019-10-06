# RuntimeException和Checked Exception

## Checked Exception
我们经常遇到的 IO 异常及 sql 异常就属于检查式异常。 对于这种异常， java 编译器要求我们必须对出现的这些异常进行 catch 所以面对这种异常不管我们是否愿意，只能自己去写一堆catch 来捕捉这些异常。

## RuntimeException
我们可以不处理。 当出现这样的异常时， 总是由虚拟机接管。 比如： 我们从来没有人去处理过 NullPointerException 异常， 它就是运行时异常， 并且这种异常还是最常见的异常之一。

被检查的异常应该用 try-catch 块代码处理或用 throws 关键字抛出，不受检查的异常在程序中不要求被处理或用 throws 抛出；Exception 是所有被检查异常的基类，而 RuntimeException（是 Exception 的子类） 是所有不受检查异常的基类；被检查的异常适用于那些不是因程序引起的错误情况（如 *FileNotFoundException*），而不被检查的异常通常都是由于糟糕的编程引起（如 *NullPointerException*）。
