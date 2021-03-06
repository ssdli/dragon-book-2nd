2.8 Intermediate Code Generation
--------------------------------
#####Exercise 2.8.1
```
For-statements in C and Java have the form:

for ( exprl ; expr2 ; expr3 ) stmt

The first expression is executed before the loop; it is typically used for initializing
the loop index. The second expression is a test made before each iteration of the loop;
the loop is exited if the expression becomes 0. The loop itself can be thought of as the
statement {stmt expr3 ; }. The third expression is executed at the end of each iteration;
it is typically used to increment the loop index. The meaning of the for-statement is
similar to

expr1 ; while ( expr2 ) {stmt expr3 ; }

Define a class For for for-statements, similar to class If in Fig. 2.43.
```
 - Solution in c++ style pseudocode:
```cpp
class For : public Stmt
{
public:
    using Stmt::emit;

    For(const Expr& e1, const Expr& e2, const Expr& e3, const Stmt& s):
        expr1{e1}, 
        expr2{e2}, 
        expr3{e3}, 
        stmt{s},
        loop{new_label()},
        after(new_label())
    {}
    
    virtual void gen() const override
    {
        expr1.gen();
        emit(loop + ":");
        emit(" ifFalse " + expr2.rvalue().to_string() + " goto " + after);
        stmt.gen();
        expr3.gen();
        emit("goto " + loop);
        emit(after + ":");
    }
    
protected:
    Expr expr1;
    Expr expr2;
    Expr expr3;
    Stmt stmt;
    Label loop;
    Label after;
}
```
 - output should be somethng like :
```
expr1
loop : ifFalse expr2 goto after
stmt
expr3
goto loop
after : 
```
#####Exercise 2.8.2
```
The programming language C does not have a boolean type. Show how a C compiler might 
translate an if-statement into three-address code.
```
- Solution: use something like `ifEqual 0` instead.
