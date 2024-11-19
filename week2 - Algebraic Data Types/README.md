# Algebraic Data Types

> CIS 194 Week 2 - 21 January 2013

## Enumeration Types

Like many programming languages, Haskell allows programmers to create their own *enumeration* types. Here's a simple example:  
많은 프로그래밍 언어와 마찬가지로, Haskell은 프로그래머가 자체적인 *열거형* 타입을 생성할 수 있도록 허용합니다. 다음은 간단한 예제입니다:

```haskell
data Thing = Shoe 
           | Ship 
           | SealingWax 
           | Cabbage 
           | King
  deriving Show
```

This declares a new type called `Thing` with five *data constructors* `Shoe`, `Ship`, etc., which are the (only) values of type `Thing`. (The `deriving Show` is a magical incantation which tells GHC to automatically generate default code for converting `Thing`s to `String`s. This is what `ghci` uses when printing the value of an expression of type `Thing`.)  
이는 `Thing`이라는 새로운 타입을 선언하며, `Shoe`, `Ship` 등 다섯 개의 *데이터 생성자*를 가집니다. 이 생성자들이 `Thing` 타입의 (유일한) 값들입니다. (`deriving Show`는 마법 같은 주문으로, GHC에게 `Thing`을 `String`으로 변환하는 기본 코드를 자동으로 생성하도록 지시합니다. 이것이 `ghci`가 `Thing` 타입의 표현식을 출력할 때 사용하는 것입니다.)

```haskell
shoe :: Thing
shoe = Shoe

listO'Things :: [Thing]
listO'Things = [Shoe, SealingWax, King, Cabbage, King]
```

We can write functions on `Thing`s by *pattern-matching*.  
우리는 *패턴 매칭*을 통해 `Thing`에 대한 함수를 작성할 수 있습니다.

```haskell
isSmall :: Thing -> Bool
isSmall Shoe       = True
isSmall Ship       = False
isSmall SealingWax = True
isSmall Cabbage    = True
isSmall King       = False
```

Recalling how function clauses are tried in order from top to bottom, we could also make the definition of `isSmall` a bit shorter like so:  
함수 절이 위에서 아래로 순서대로 시도되는 방식을 기억하면, `isSmall`의 정의를 다음과 같이 조금 더 간단하게 만들 수 있습니다:

```haskell
isSmall2 :: Thing -> Bool
isSmall2 Ship = False
isSmall2 King = False
isSmall2 _    = True
```

## Beyond Enumerations

`Thing` is an *enumeration type*, similar to those provided by other languages such as Java or C++. However, enumerations are actually only a special case of Haskell's more general *algebraic data types*. As a first example of a data type which is not just an enumeration, consider the definition of `FailableDouble`:  
`Thing`은 Java나 C++과 같은 다른 언어에서 제공되는 것과 유사한 *열거형 타입*입니다. 그러나 열거형은 실제로 Haskell의 보다 일반적인 *대수적 데이터 타입*의 특별한 경우에 불과합니다. 열거형이 아닌 데이터 타입의 첫 번째 예로 `FailableDouble`의 정의를 고려해 봅시다:

```haskell
data FailableDouble = Failure
                    | OK Double
  deriving Show
```

This says that the `FailableDouble` type has two data constructors. The first one, `Failure`, takes no arguments, so `Failure` by itself is a value of type `FailableDouble`. The second one, `OK`, takes an argument of type `Double`. So `OK` by itself is not a value of type `FailableDouble`; we need to give it a `Double`. For example, `OK 3.4` is a value of type `FailableDouble`.  
이는 `FailableDouble` 타입이 두 개의 데이터 생성자를 가진다는 것을 의미합니다. 첫 번째 생성자인 `Failure`는 인수를 받지 않으므로, `Failure` 자체가 `FailableDouble` 타입의 값입니다. 두 번째 생성자인 `OK`는 `Double` 타입의 인수를 받습니다. 따라서 `OK` 자체는 `FailableDouble` 타입의 값이 아니며, `Double`을 제공해야 합니다. 예를 들어, `OK 3.4`는 `FailableDouble` 타입의 값입니다.

```haskell
exD1 = Failure
exD2 = OK 3.4
```

*Thought exercise: what is the type of `OK`?*  
*생각해 볼 문제: `OK`의 타입은 무엇일까요?*

```haskell
safeDiv :: Double -> Double -> FailableDouble
safeDiv _ 0 = Failure
safeDiv x y = OK (x / y)
```

More pattern-matching! Notice how in the `OK` case we can give a name to the `Double` that comes along with it.  
더 많은 패턴 매칭! `OK`의 경우, 함께 오는 `Double`에 이름을 줄 수 있다는 점에 주목하세요.

```haskell
failureToZero :: FailableDouble -> Double
failureToZero Failure = 0
failureToZero (OK d)  = d
```

Data constructors can have more than one argument.  
데이터 생성자는 두 개 이상의 인수를 가질 수 있습니다.

```haskell
-- Store a person's name, age, and favourite Thing.
data Person = Person String Int Thing
  deriving Show

brent :: Person
brent = Person "Brent" 31 SealingWax

stan :: Person
stan  = Person "Stan" 94 Cabbage

getAge :: Person -> Int
getAge (Person _ a _) = a
```

Notice how the type constructor and data constructor are both named `Person`, but they inhabit different namespaces and are different things. This idiom (giving the type and data constructor of a one-constructor type the same name) is common, but can be confusing until you get used to it.  
타입 생성자와 데이터 생성자가 모두 `Person`이라는 이름을 가진다는 점에 주목하세요. 그러나 이들은 서로 다른 네임스페이스에 속하며 다른 것입니다. 이 관용구(단일 생성자 타입의 타입과 데이터 생성자에 동일한 이름을 부여하는 것)는 일반적이지만, 익숙해질 때까지는 혼란스러울 수 있습니다.

## Algebraic Data Types in General

In general, an algebraic data type has one or more data constructors, and each data constructor can have zero or more arguments.  
일반적으로, 대수적 데이터 타입은 하나 이상의 데이터 생성자를 가지며, 각 데이터 생성자는 0개 이상의 인수를 가질 수 있습니다.

```haskell
data AlgDataType = Constr1 Type11 Type12
                 | Constr2 Type21
                 | Constr3 Type31 Type32 Type33
                 | Constr4
```

This specifies that a value of type `AlgDataType` can be constructed in one of four ways: using `Constr1`, `Constr2`, `Constr3`, or `Constr4`. Depending on the constructor used, an `AlgDataType` value may contain some other values. For example, if it was constructed using `Constr1`, then it comes along with two values, one of type `Type11` and one of type `Type12`.  
이는 `AlgDataType` 타입의 값이 `Constr1`, `Constr2`, `Constr3`, 또는 `Constr4` 중 하나를 사용하여 네 가지 방법 중 하나로 생성될 수 있음을 명시합니다. 사용된 생성자에 따라, `AlgDataType` 값은 다른 일부 값을 포함할 수 있습니다. 예를 들어, `Constr1`을 사용하여 생성되었다면, `Type11` 타입의 값 하나와 `Type12` 타입의 값 하나를 함께 가집니다.

One final note: type and data constructor names must always start with a capital letter; variables (including names of functions) must always start with a lowercase letter. (Otherwise, Haskell parsers would have quite a difficult job figuring out which names represent variables and which represent constructors).  
마지막으로 한 가지 주의할 점: 타입과 데이터 생성자의 이름은 항상 대문자로 시작해야 하며, 변수(함수 이름 포함)는 항상 소문자로 시작해야 합니다. (그렇지 않으면, Haskell 파서가 어떤 이름이 변수를 나타내고 어떤 이름이 생성자를 나타내는지 구별하는 데 상당히 어려움을 겪을 것입니다.)

## Pattern-Matching

We've seen pattern-matching in a few specific cases, but let's see how pattern-matching works in general. Fundamentally, pattern-matching is about taking apart a value by *finding out which constructor* it was built with. This information can be used as the basis for deciding what to do—indeed, in Haskell, this is the *only* way to make a decision.  
몇 가지 특정 경우에서 패턴 매칭을 보았지만, 이제 패턴 매칭이 일반적으로 어떻게 작동하는지 살펴보겠습니다. 기본적으로, 패턴 매칭은 *어떤 생성자로 생성되었는지를 파악*함으로써 값을 분해하는 것입니다. 이 정보는 무엇을 할지 결정하는 기초로 사용될 수 있습니다, 실제로, Haskell에서는 이것이 결정을 내리는 *유일한* 방법입니다.

For example, to decide what to do with a value of type `AlgDataType` (the made-up type defined in the previous section), we could write something like  
예를 들어, `AlgDataType` 타입의 값을 어떻게 처리할지 결정하기 위해 (앞 섹션에서 정의한 가상의 타입) 다음과 같은 코드를 작성할 수 있습니다:

```haskell
foo (Constr1 a b)   = ...
foo (Constr2 a)     = ...
foo (Constr3 a b c) = ...
foo Constr4         = ...
```

Note how we also get to give names to the values that come along with each constructor. Note also that parentheses are required around patterns consisting of more than just a single constructor.  
각 생성자와 함께 오는 값에 이름을 부여할 수 있다는 점에 주목하세요. 또한, 단일 생성자 이상의 패턴에는 괄호가 필요하다는 점도 주목하세요.

This is the main idea behind patterns, but there are a few more things to note.  
이것이 패턴의 기본 개념이지만, 몇 가지 더 주의할 점이 있습니다.

1. An underscore `_` can be used as a "wildcard pattern" which matches anything.  
밑줄 `_`은 아무것이나 매칭하는 "와일드카드 패턴"으로 사용할 수 있습니다.

2. A pattern of the form `x@pat` can be used to match a value against the pattern `pat`, but *also* give the name `x` to the entire value being matched. For example:  
`x@pat` 형태의 패턴은 값을 `pat` 패턴과 매칭하는 데 사용할 수 있지만, *또한* 매칭되는 전체 값에 `x`라는 이름을 부여할 수 있습니다. 예를 들어:

   ```haskell
   baz :: Person -> String
   baz p@(Person n _ _) = "The name field of (" ++ show p ++ ") is " ++ n
   ```

   ```haskell
   *Main> baz brent
   "The name field of (Person \"Brent\" 31 SealingWax) is Brent"
   ```

3. Patterns can be *nested*. For example:  
패턴은 *중첩*될 수 있습니다. 예를 들어:

   ```haskell
   checkFav :: Person -> String
   checkFav (Person n _ SealingWax) = n ++ ", you're my kind of person!"
   checkFav (Person n _ _)          = n ++ ", your favorite thing is lame."
   ```

   ```haskell
   *Main> checkFav brent
   "Brent, you're my kind of person!"
   *Main> checkFav stan
   "Stan, your favorite thing is lame."
   ```

   Note how we nest the pattern `SealingWax` inside the pattern for `Person`.  
   `Person` 패턴 내부에 `SealingWax` 패턴을 중첩한 방식을 주목하세요.

In general, the following grammar defines what can be used as a pattern:  
일반적으로, 다음의 문법은 패턴으로 사용할 수 있는 것을 정의합니다:

```
pat ::= _
     |  var
     |  var @ ( pat )
     |  ( Constructor pat1 pat2 ... patn )
```

The first line says that an underscore is a pattern. The second line says that a variable by itself is a pattern: such a pattern matches anything, and "binds" the given variable name to the matched value. The third line specifies `@`-patterns. The last line says that a constructor name followed by a sequence of patterns is itself a pattern: such a pattern matches a value if that value was constructed using the given constructor, *and* `pat1` through `patn` all match the values contained by the constructor, recursively.  
첫 번째 줄은 Underscore가 패턴임을 나타냅니다. 두 번째 줄은 변수 자체가 패턴임을 나타냅니다: 이러한 패턴은 무엇이든 매칭하며, 주어진 변수 이름을 매칭된 값에 "바인딩"합니다. 세 번째 줄은 `@`-패턴을 명시합니다. 마지막 줄은 생성자 이름 뒤에 일련의 패턴이 오는 것이 자체적으로 패턴임을 나타냅니다: 이러한 패턴은 해당 값이 주어진 생성자를 사용하여 생성되었고, `pat1`부터 `patn`까지가 생성자가 포함하는 값과 재귀적으로 모두 매칭될 때 값을 매칭합니다.

(In actual fact, the full grammar of patterns includes yet more features still, but the rest would take us too far afield for now.)  
(실제로, 패턴의 전체 문법에는 더 많은 기능이 존재하지만, 그것들은 지금 당장 다루기에는 너무 벗어난 주제들입니다.)

Note that literal values like `2` or `'c'` can be thought of as constructors with no arguments. It is as if the types `Int` and `Char` were defined like  
`2`나 `'c'`와 같은 리터럴 값은 인수가 없는 생성자로 생각할 수 있습니다. 마치 `Int`와 `Char` 타입이 다음과 같이 정의된 것과 같습니다:

```haskell
data Int  = 0 | 1 | -1 | 2 | -2 | ...
data Char = 'a' | 'b' | 'c' | ...
```

which means that we can pattern-match against literal values. (Of course, `Int` and `Char` are not *actually* defined this way.)  
이는 우리가 리터럴 값과 패턴 매칭을 할 수 있다는 것을 의미합니다. (물론, `Int`와 `Char`는 실제로 *그런 방식으로* 정의되어 있지는 않습니다.)

## Case Expressions

The fundamental construct for doing pattern-matching in Haskell is the `case` expression. In general, a `case` expression looks like  
Haskell에서 패턴 매칭을 수행하는 기본적인 구조는 `case` 표현식입니다. 일반적으로, `case` 표현식은 다음과 같습니다:

```haskell
case exp of
  pat1 -> exp1
  pat2 -> exp2
  ...
```

When evaluated, the expression `exp` is matched against each of the patterns `pat1`, `pat2`, ... in turn. The first matching pattern is chosen, and the entire `case` expression evaluates to the expression corresponding to the matching pattern. For example,  
평가될 때, 표현식 `exp`는 차례대로 각 패턴 `pat1`, `pat2`, ...과 매칭됩니다. 첫 번째로 매칭되는 패턴이 선택되며, 전체 `case` 표현식은 매칭되는 패턴에 해당하는 표현식으로 평가됩니다. 예를 들어,

```haskell
exCase = case "Hello" of
           []      -> 3
           ('H':s) -> length s
           _       -> 7
```

evaluates to `4` (the second pattern is chosen; the third pattern matches too, of course, but it is never reached).  
이는 `4`로 평가됩니다 (두 번째 패턴이 선택됩니다; 물론 세 번째 패턴도 매칭되지만, 결코 도달하지 않습니다).

In fact, the syntax for defining functions we have seen is really just convenient syntax sugar for defining a `case` expression. For example, the definition of `failureToZero` given previously can equivalently be written as  
실제로, 우리가 본 함수 정의 구문은 `case` 표현식을 정의하기 위한 편리한 구문 설탕에 불과합니다. 예를 들어, 이전에 제공된 `failureToZero`의 정의는 동등하게 다음과 같이 작성할 수 있습니다:

```haskell
failureToZero' :: FailableDouble -> Double
failureToZero' x = case x of
                     Failure -> 0
                     OK d    -> d
```

## Recursive Data Types

Data types can be *recursive*, that is, defined in terms of themselves. In fact, we have already seen a recursive type—the type of lists. A list is either empty, or a single element followed by a remaining list. We could define our own list type like so:  
데이터 타입은 *재귀적*일 수 있습니다, 즉 자신을 기준으로 정의될 수 있습니다. 실제로, 우리는 이미 재귀적 타입인 리스트의 타입을 보았습니다. 리스트는 비어 있거나, 단일 요소와 그에 따른 나머지 리스트로 구성됩니다. 우리는 다음과 같이 자체 리스트 타입을 정의할 수 있습니다:

```haskell
data IntList = Empty | Cons Int IntList
```

Haskell's own built-in lists are quite similar; they just get to use special built-in syntax (`[]` and `:`). (Of course, they also work for any type of elements instead of just `Int`s; more on this next week.)  
Haskell의 자체 내장 리스트는 매우 유사합니다; 단지 특별한 내장 구문 (`[]`와 `:`)을 사용할 뿐입니다. (물론, 이는 `Int`뿐만 아니라 모든 타입의 요소에도 작동합니다; 다음 주에 더 다룰 예정입니다.)

We often use recursive functions to process recursive data types:  
우리는 종종 재귀 함수를 사용하여 재귀적 데이터 타입을 처리합니다:

```haskell
intListProd :: IntList -> Int
intListProd Empty      = 1
intListProd (Cons x l) = x * intListProd l
```

As another simple example, we can define a type of binary trees with an `Int` value stored at each internal node, and a `Char` stored at each leaf:  
또 다른 간단한 예로, 각 내부 노드에 `Int` 값을 저장하고, 각 리프에 `Char`를 저장하는 이진 트리 타입을 정의할 수 있습니다:

```haskell
data Tree = Leaf Char
          | Node Tree Int Tree
  deriving Show
```

(Don't ask me what you would use such a tree for; it's an example, OK?) For example,  
(이러한 트리를 무엇에 사용할지 묻지 마세요; 예제일 뿐입니다, 알겠죠?) 예를 들어,

```haskell
tree :: Tree
tree = Node (Leaf 'x') 1 (Node (Leaf 'y') 2 (Leaf 'z'))
```

---
