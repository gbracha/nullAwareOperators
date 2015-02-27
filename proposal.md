#Null-aware Operators

## Contact information

1. **Gilad Bracha.** 

2. **gbracha@google.com.** 

3. **https://github.com/gbracha/nullAwareOperators** 



##Summary 


We propose the addition of the following operators:

* `?.` where *a?.b* is sugar for `a == ` **null** `? ` **null** ` : a.b`.

* The null-coalescing operator (aka ifNull) `??` where *a??b* is sugar for `a == ` **null**` ? b : a`.

* `?=` where *v ?= e* is sugar for **if**` (v == `**null**`) v = e`.

##Motivation

See bugs:

https://code.google.com/p/dart/issues/detail?id=41



###Pros

* Simple. Effects are entirely local, specified by translation to existing Dart.

###Cons

* Syntactic sugar causes cancer of the semicolon.

* There might be demand for extensions. For example, compound null-aware assignment, e.g.,  `a?+= x`. These are not supported. The fact that they are not supported is not, in my view, a weakness of the proposal. The weakness is that it gives rise to such temptations.

* Allows for 'elvis closurization' as in `a?.m` where *m* is a method, not because this is desired, but because it is not easily distinguished from other cases. In some sense this runs counter to the desire to clean up closurization as described in the generalized tera offs proposal.  I'd rather not introduce things like `a?#m`.


##Examples

```
if ((stringMightBeNull ?? "").Length == 0) {
    // string is empty or null
}
```

`zip = lottery.drawWinner?().address?.zipcode`


## Proposal

See above. The proposal is strictly limited to the above syntactic transforms. 




## Deliverables


### Language specification changes


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
