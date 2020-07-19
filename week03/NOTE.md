# JavaScript按层级剖析

- Atom：一般包含关键字、直接量
    + Grammer角度：语法树、优先级、左值和右值
        * 语法树与优先级间的关系：优先级越高，位于语法树的层级越底
        * 运算符优先级从高到低依次为：以下Expression章节按运算符从高到低排列

    + Runtime角度：类型转换与引用

- Expression：Atom通过运算符Operator以及辅助符号Punctuator，形成表达式。表达式通常是级联结构
    + Grammer-Member Expression：
        * a.b：成员访问，取出来的不是属性的值，而是引用
        * a[b]：成员访问，但是b可以是运行时的字符串
        * foo`string`：函数访问，函数名后带``字符串，会将字符串拆开作为函数参数
        * super.b：只有在class的构造函数里可用
        * super[`b`]：只有在class的构造函数里可用
        * new.target：前后2词位置固定，不可更换
        * new Foo()

    + Grammer-New Expression：
        * new Foo

    ex: new a()()：按照运算符优先级，是(new a())()
        new new a()：按照运算符优先级，是new (new a())

    + Grammer-Call Expression：函数调用
        * foo()
        * super()
        * foo()['b']
        * foo().b
        * foo()`abc`

    ex: new a()['b']：按照运算符优先级，是先new出一个对象，再去访问它的成员

    + Grammer-Update Expression：自增、自减
        * a++
        * a--
        * --a
        * ++a
    + Grammer-Unary Expression：单目运算符
        * delete a.b
        * void foo()
        * typeof a
        * +a
        * -a
        * ~a
        * !a
        * await a

    + Grammer-Exponental Expression：
        * 唯一一个右结合的运算符**，乘方
    
    + Grammer-Multipliative Expression:
        * /、*、%

    + Grammer-Additive Expression：
        * +、-

    + Grammer-Shift Expression：
        * << >> >>>

    + Grammer-Relationship Expression：
        * <、>、<=、>=、instance of 、in

    + Grammer-Equality Expression:
        * ==、===、！=、！===

    + Grammer-Bitwise Expression：
        * &、^、|

    + Grammer-Logical Expression：
        * &&、||

    + Grammer-Conditional Expression：
        * ?:

    + Grammer-Left HandSide & Rigth HandSide：
        * Left Hanside一定是Right HandSide
        * Right Handside 从 Update Expression开始

    + Runtime-Reference：
        * 引用分为Object和Key两个部分
        * delete和assign操作就会用到引用：
            * delete后跟Member Expression，删除哪个对象哪个key
            * Member Expression放等号左边，赋值给哪个对象的哪个属性

    + Runtime-Type Convertion：
        * ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week03/Type%20Convertion.png)
        * Unboxing：拆箱转换，将Object转换为基本类型
            * ToPrimitive：发生在诸如Object参与运算的情况，都会调用ToPrimitive。一个对象有3个方法会影响到ToPrimitive，分别是ToString                ()、valueOf()和[Symbol.toPrimitive]()。其中[Symbol.toPrimitive]()如果有定义，会忽略其他2个方法。                    转Number会优先调用valueOf()，一定会用到字符串的场景会调用toString()
        * Boxing：装箱转换
          ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week03/Boxing.png)

- Statement：表达式+关键字+辅助符号，形成语句
    + Grammer角度：简单语句、复合语句和声明

    + Runtime角度：作用域与语句执行结果的记录

    + Runtime-Completion Record：
        * 存储语句完成的结果
        * 分为三大部分：[[type]]、[[value]]和[[taeget]]
        * [[type]]：normal、break、continue、return、throw
        * [[value]]：基本类型
        * [[target]]：label，即语句前加标识符和冒号

    +  Runtime-Lexical Environment：
        * var的作用域是它所在的函数体，不管其外面套上什么。例如function test(){a = 1;{var a;}}语句是生效的
        * const的作用域是它所在的花括号。但如果在循环语句里加const或let声明，则其作用域是整个循环体

    + Grammer-简单语句：
        * ExpresionStatement：表达式语，表达式加分号
        * EmptyStatement：单独的分号;
        * DebuggerStatement：debugger;
        * ThrowStatement：throw+表达式，抛出异常
        * ContinueStatement：continue; [[type]]为continue，[[value]]未明，[[target]]为label
        * BreakStatement：break; [[type]]为break，[[value]]未明，[[target]]为label
        * ReturnStatement：return；

    + Grammer-复合语句：
        * BlockStatement：{语句列表}，[[type]]为normal，[[value]]和[[target]]不明
        * IfStatement
        * SwitchStatement
        * IterationStatement：循环语句，其中for(A;B;C)D;、for(A in B) C;、for(A of B)C;3种for循环中，A和BCD分属2个不同的作用域
        * WithStatement
        * LabelledStatement：在简单/复合语句前加label
        * TryStatement：try、catch、finally。其中catch(){}的()会单独产生一个作用域。[[type]]return，[[value]]未明，[[target]]为                  label

    + Grammer-声明：
        * FunctionDeclatraion
        * GeneratorDeclaration
        * AsyncFunctionDeclaration
        * AsyncGeneratorDeclaration
        * VariableDeclaration
        * ClassDeclaration
        * LexicalDeclaration
        * 老一代的声明：funtion、function*、async function、async function*、var，只认Fcuntion Body，会当成出现在函数的第一行一样去处理
        * 新一代的声明：class、const和let，则只能在声明后被使用
    
    + 预处理pre-process：
        * 会提前找到所有被声明为var的变量，并使其生效

- Structure：结构化的层级。Function、Class、Process、Namespace
    + JS执行粒度(运行时)
        * 宏任务：传给JS引擎的任务，一个宏任务可以由多个微任务组成
        * 微任务(Promise)：在JS引擎内部执行的任务
        * 函数调用(Execution Context)
        * 语句/声明(Completion Record)
        * 表达式(Reference)
        * 直接量、变量、this

    + 函数调用
        * 函数调用会形成一个Execution Context Stack，栈中内容为Execution Context，栈顶指针称为Running Execution Context（执行上下文）
        * Execution Context:
            * code evaluation state：用于async和generator函数，保存代码执行位置的信息
            * Function
            * Script or Module：在Script里的代码或Module里的代码所拥有的上下文
            * Generator
            * Realm：保存所有内置对象
            * LexicalEnvironment：执行代码中所需要访问的环境，即保存变量的环境，this、new.target、super、变量
            * VariableEnvironment：用var声明的变量所声明的环境。历史包袱
        * Environment会形成链式结构，每个节点又称为Environment Record：
           ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week03/Environment%20Record.png)

        * 每个函数都会形成一个闭包(Closure)，其分为代码部分和环境部分。
            * 环境部分由一个Object和一个变量的序列来组成，即Environment Record
            * ex:
              ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week03/Closure.png)

    + Realm
        * 在JS引擎中，所有内置对象都被放置在Realm中。根据不同的外部环境，会创建不同的Realm
        * 在JS中，函数表达式、对象直接量和使用.做隐式装换，都会创建对象。需要有记录这些对象原型的东西，这就是Realm
        * 在不同的iframe中创建的对象，其原型也是不同的，记录在Realm中
        * 在不同的Realm实例间，它们是互相独立的。但它们之间也可以互相传递对象。
