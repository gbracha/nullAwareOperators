#Null-aware Operators

## Contact information

1. **Gilad Bracha.** 

2. **gbracha@google.com.** 

3. **https://github.com/gbracha/nullAwareOperators** 



##Summary 


We propose the addition of the following operators:

* `?.` where *a?.b* is sugar for `a == ` **null** `? ` **null** ` : a.b`.

* The null-coalescing operator (aka ifNull) `??` where *a??b* is sugar for `a == ` **null**` ? b : a`.

* `??=` where *v ??= e* is sugar for `v = v == ` **null** `? ` *e*  `: v`.

##Motivation

See bugs:

https://code.google.com/p/dart/issues/detail?id=41



###Pros

* Simple. Effects are entirely local, specified by translation to existing Dart.

###Cons

* Syntactic sugar causes cancer of the semicolon.

* There might be demand for extensions. For example, compound null-aware assignment, e.g.,  `a?+= x`. These are not supported. The fact that they are not supported is not, in my view, a weakness of the proposal. The weakness is that it gives rise to such temptations.

* Allows for 'elvis closurization' as in `a?.m` where *m* is a method, not because this is desired, but because it is not easily distinguished from other cases. In some sense this runs counter to the desire to clean up closurization as described in the [generalized tear offs proposal](https://github.com/gbracha/generalizedTearOffs).  I'd rather not introduce things like `a?#m`.

* Does not support operators.


##Examples

```
if ((stringMightBeNull ?? "").length == 0) {
    // string is empty or null
}
```

`zip = lottery?.drawWinner()?.address?.zipcode`


## Proposal

See above. The proposal is strictly limited to the above syntactic transforms. 




## Deliverables


### Language specification changes




#### 16.19 Assignment

An assignment changes the value associated with a mutable variable or property.

```
assignmentOperator:
‘=’ | 
‘??=’ | 
compoundAssignmentOperator
;
```
A conditional assignment of the form *v ??= e* is equivalent to the expression 
`v = v == ` **null** `? ` *e*  `: v`.

**Rest of section 16.19 is unchanged** 

**?? can either replace unaryExpression or logicalOrExpression. Assuming the latter.**

#### 16.20 Conditional

A conditional expression evaluates one of two expressions based on a boolean condition.
conditionalExpression:

```
ifNullExpression (‘ ?’ expressionWithoutCascade ‘:’ expressionWithoutCascade)?
;
```
**Rest of section 16.20 is unchanged**


#### 16.xx If-**null** Expression

An *if-null* expression evaluates an expression and if the result is **null**, evaluates another.

```
ifNullExpression:
  logicalOrExpression ‘??’ logicalOrExpression
```

An if-null expression of the form *e1??e2* is equivalent to the expression *e1 == null? e2: e1*.

#### 16.31 Assignable Expressions

Assignable expressions are expressions that can appear on the left hand side of an assignment. This section describes how to evaluate these expressions when they do not constitute the complete left hand side of an assignment.

*Of course, if assignable expressions always appeared as the left hand side, one would have no need for their value, and the rules for evaluating them would be unnecessary. However, assignable expressions can be subexpressions of other expressions and therefore must be evaluated.*

```
assignableExpression:
primary (arguments* assignableSelector)+ | 
super assignableSelector |
identifier
;

assignableSelector:
‘[’ expression ‘]’ |
 ‘.’ identifier |
 ‘?.’ identifier 
;
```
An assignable expression can be conditional or unconditional. 

A conditional assignable expression of the form *e1?.identifier* is equivalent to the expression 
`e1 == ` **null** `? ` **null** ` : e1.identifier`.

An unconditional assignable expression is either:

**Rest of section 16.31 is unchanged**

### A working implementation

TBD

### Tests

TBD

## Patents rights

[tex]: http://www.latex-project.org/
[language spec]: https://www.dartlang.org/docs/spec/
[dart standard]: http://www.ecma-international.org/publications/standards/Ecma-408.htm
[rfpp]: http://www.ecma-international.org/memento/TC52%20policy/Ecma%20Experimental%20TC52%20Royalty-Free%20Patent%20Policy.pdf
[external contributer form]: http://www.ecma-international.org/memento/TC52%20policy/Contribution%20form%20to%20TC52%20Royalty%20Free%20Task%20Group%20as%20a%20non-member.pdf
