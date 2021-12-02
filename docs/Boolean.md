## Boolean Expressions

**Basic Theorems**

逻辑函数的基本定理

1. a && false == false
2. a && true == a
3. a && a == a
4. a && !a == false
5. a || false == a
6. a || true == true
7. a || a == a
8. a || !a == true
9. !(!a) == a (The inverse of an inverse is the original value)

**Consensus Theorems (Nice to know, but not required)**

共识定理

- a || (!a && b) == a || b
- a || (!a && !b) == a || !b
- !a || (a && b) == !a || b
- !a || (a && !b) == !a || !b

**Commutative Law (Boolean AND and OR are commutative)**

交换律

- a && b == b && a
- a || b == b || a

**Associative Law (Boolean AND and OR are associative)**

结合律

- a && (b && c) == (a && b) && c
- a || (b || c) == (a || b) || c

**Distributive Law (Boolean AND distributes over boolean OR)**

分配律

- a && (b || c) == (a && b) || (a && c)

**DeMorgan's Theorems (Important to know) (Boolean NOT does not distribute over boolean OR or AND)**

德摩根定理

- !(a && b) == !a || !b

- !(a || b) == !a && !b

  

  `!(!A || B)`并且`A && !B`是等价的。您可以通过比较真值表轻松验证：

  ```
  A  B  !A  !A || B  !(!A || B)
  -  -  --  -------  ----------
  0  0   1     1         0
  0  1   1     1         0
  1  0   0     0         1
  1  1   0     1         0
  ```

  ```
  A  B  !B  A && !B
  -  -  --  -------
  0  0   1     0   
  0  1   0     0   
  1  0   1     1   
  1  1   0     0   
  ```
  
  ```
  !(!A || B) = !(!A) && !B = A && !B
  ```

[知识来源](https://fiveable.me/ap-comp-sci-a/unit-3/equivalent-boolean-expressions/study-guide/aMDnyFuOcAXnZigLW1vL)