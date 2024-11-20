# Recursion Patterns, Polymorphism, and the Prelude

> CIS 194 Week 3 - 28 January 2013

While completing HW 2, you probably spent a lot of time writing explicitly recursive functions. At this point, you might think that's what Haskell programmers spend most of their time doing. In fact, experienced Haskell programmers *hardly ever* write recursive functions!  
week 2 과제를 완료하는 동안 아마도 많은 시간을 명시적으로 재귀 함수를 작성하는 데 보냈을 것입니다. 이 시점에서 Haskell 프로그래머들이 대부분의 시간을 그렇게 보낸다고 생각할 수도 있습니다. 실제로 경험 많은 Haskell 프로그래머들은 *거의* 재귀 함수를 작성하지 않습니다!

How is this possible? The key is to notice that although recursive functions can theoretically do pretty much anything, in practice there are certain common patterns that come up over and over again. By abstracting out these patterns into library functions, programmers can leave the low-level details of actually doing recursion to these functions, and think about problems at a higher level—that's the goal of *wholemeal programming*.  
이것이 어떻게 가능한 걸까요? 핵심은 재귀 함수가 이론적으로는 거의 모든 것을 할 수 있지만, 실제로는 반복적으로 나타나는 특정한 공통 패턴이 있다는 점을 인식하는 것입니다. 이러한 패턴을 라이브러리 함수로 추상화함으로써, 프로그래머들은 실제로 재귀를 수행하는 저수준의 세부 사항을 이러한 함수에 맡기고 문제를 더 높은 수준에서 생각할 수 있습니다. 이것이 *wholemeal programming*의 목표입니다.

## Recursion Patterns

Recall our simple definition of lists of `Int` values:  
`Int` 값의 리스트에 대한 간단한 정의를 기억하세요:

```haskell
data IntList = Empty | Cons Int IntList
  deriving Show
```

What sorts of things might we want to do with an `IntList`? Here are a few common possibilities:  
`IntList`를 활용하여 어떤 작업들을 할 수 있을까요? 다음은 몇 가지 일반적인 예시입니다:

- Perform some operation on every element of the list  
리스트의 모든 요소에 대해 어떤 연산을 수행하기
- Keep only some elements of the list, and throw others away, based on a test  
테스트를 기반으로 리스트의 일부 요소만 유지하고 나머지는 버리기
- "Summarize" the elements of the list somehow (find their sum, product, maximum...)  
리스트의 요소들을 어떤 식으로든 "요약"하기(합계, 곱, 최대값 찾기 등)
- You can probably think of others!  
아마도 다른 것들도 생각해낼 수 있을 것입니다!


### Map

Let's think about the first one ("perform some operation on every element of the list"). For example, we could add one to every element in a list:  
첫 번째 경우("리스트의 모든 요소에 대해 어떤 연산을 수행하기")를 생각해 봅시다. 예를 들어, 리스트의 모든 요소에 1을 더할 수 있습니다:

```haskell
addOneToAll :: IntList -> IntList
addOneToAll Empty       = Empty
addOneToAll (Cons x xs) = Cons (x+1) (addOneToAll xs)
```

Or we could ensure that every element in a list is nonnegative by taking the absolute value:  
또는 절댓값을 취하여 리스트의 모든 요소가 음이 아니도록 할 수 있습니다:

```haskell
absAll :: IntList -> IntList
absAll Empty       = Empty
absAll (Cons x xs) = Cons (abs x) (absAll xs)
```

Or we could square every element:  
또는 모든 요소를 제곱할 수 있습니다:

```haskell
squareAll :: IntList -> IntList
squareAll Empty       = Empty
squareAll (Cons x xs) = Cons (x*x) (squareAll xs)
```

At this point, big flashing red lights and warning bells should be going off in your head. These three functions look way too similar. There ought to be some way to abstract out the commonality so we don't have to repeat ourselves!  
이 시점에서 머릿속에 큰 빨간 불과 경고 벨이 울리고 있을 것입니다. 이 세 함수는 너무 비슷해 보입니다. 공통점을 추상화하여 반복하지 않아도 되는 방법이 분명히 있어야 합니다!

There is indeed a way—can you figure it out? Which parts are the same in all three examples and which parts change?  
실제로 방법이 있습니다—찾아낼 수 있나요? 세 가지 예제에서 동일한 부분과 다른 부분은 무엇인가요?

The thing that changes, of course, is the operation we want to perform on each element of the list. We can specify this operation as a *function* of type `Int -> Int`. Here is where we begin to see how incredibly useful it is to be able to pass functions as inputs to other functions!  
물론 변경되는 것은 리스트의 각 요소에 대해 수행하려는 연산입니다. 이 연산을 `Int -> Int` 타입의 *함수*로 지정할 수 있습니다. 여기서 우리는 함수를 다른 함수의 입력으로 전달할 수 있는 것이 얼마나 놀랍게 유용한지 보기 시작합니다!

```haskell
mapIntList :: (Int -> Int) -> IntList -> IntList
mapIntList _ Empty       = Empty
mapIntList f (Cons x xs) = Cons (f x) (mapIntList f xs)
```

We can now use `mapIntList` to implement `addOneToAll`, `absAll`, and `squareAll`:  
이제 `mapIntList`를 사용하여 `addOneToAll`, `absAll`, `squareAll`을 구현할 수 있습니다:

```haskell
exampleList = Cons (-1) (Cons 2 (Cons (-6) Empty))

addOne x = x + 1
square x = x * x

mapIntList addOne exampleList
mapIntList abs    exampleList
mapIntList square exampleList
```

### Filter

Another common pattern is when we want to keep only some elements of a list, and throw others away, based on a test. For example, we might want to keep only the positive numbers:  
또 다른 일반적인 패턴은 어떠한 테스트 로직을 기반으로 리스트의 일부 요소만 유지하고 나머지는 버리고 싶을 때입니다. 예를 들어, 양수만 유지하고 싶을 수 있습니다:

```haskell
keepOnlyPositive :: IntList -> IntList
keepOnlyPositive Empty = Empty
keepOnlyPositive (Cons x xs) 
  | x > 0     = Cons x (keepOnlyPositive xs)
  | otherwise = keepOnlyPositive xs
```

Or only the even ones:  
또는 짝수만 유지할 수도 있습니다:

```haskell
keepOnlyEven :: IntList -> IntList
keepOnlyEven Empty = Empty
keepOnlyEven (Cons x xs)
  | even x    = Cons x (keepOnlyEven xs)
  | otherwise = keepOnlyEven xs
```

How can we generalize this pattern? What stays the same, and what do we need to abstract out?  
이 패턴을 어떻게 일반화할 수 있을까요? 무엇이 동일하게 유지되고, 무엇을 추상화해야 할까요?

The thing to abstract out is the *test* (or *predicate*) used to determine which values to keep. A predicate is a function of type `Int -> Bool` which returns `True` for those elements which should be kept, and `False` for those which should be discarded. So we can write `filterIntList` as follows:  
추상화할 것은 어떤 값을 유지할지 결정하는 데 사용되는 *test*(또는 *predicate*)입니다. predicate는 유지되어야 할 요소에 대해 `True`를 반환하고 버려야 할 요소에 대해 `False`를 반환하는 `Int -> Bool` 타입의 함수입니다. 따라서 `filterIntList`를 다음과 같이 작성할 수 있습니다:

```haskell
filterIntList :: (Int -> Bool) -> IntList -> IntList
filterIntList _ Empty = Empty
filterIntList p (Cons x xs)
  | p x       = Cons x (filterIntList p xs)
  | otherwise = filterIntList p xs
```

### Fold

The final pattern we mentioned was to "summarize" the elements of the list; this is also variously known as a "fold" or "reduce" operation. We'll come back to this next week. In the meantime, you might want to think about how to abstract out this pattern!  
우리가 언급한 마지막 패턴은 리스트의 요소를 "요약"하는 것이었습니다; 이것은 "폴드(fold)" 또는 "리듀스(reduce)" 연산이라 알려져 있습니다. 이에 대해서는 다음 주에 다시 다룰 것입니다, 그동안 이 패턴을 어떻게 추상화할지 생각해 볼 수 있습니다!

## Polymorphism

We've now written some nice, general functions for mapping and filtering over lists of `Int`s. But we're not done generalizing! What if we wanted to filter lists of `Integer`s? Or `Bool`s? Or lists of lists of trees of stacks of `String`s? We'd have to make a new data type and a new function for each of these cases. Even worse, the *code would be exactly the same*; the only thing that would be different is the *type signatures*. Can't Haskell help us out here?  
이제 `Int` 리스트에 대해 맵핑과 필터링을 위한 멋지고 일반적인 함수를 작성했습니다. 하지만 일반화를 끝낸 것은 아닙니다! `Integer` 리스트를 필터링하고 싶다면 어떨까요? 또는 `Bool`? 또는 `String`의 스택 트리 리스트의 리스트는? 이러한 각 경우마다 새로운 데이터 타입과 새로운 함수를 만들어야 할 것입니다. 더 나쁜 것은 *코드가 정확히 동일*하다는 것입니다; 유일하게 다른 것은 *타입 시그니처*일 뿐입니다. 여기서 Haskell이 우리를 도와줄 수 없을까요?

Of course it can! Haskell supports *polymorphism* for both data types and functions. The word "polymorphic" comes from Greek (πολυμορφος) and means "having many forms": something which is polymorphic works for multiple types.  
물론 가능합니다! Haskell은 데이터 타입과 함수 모두에 대해 *다형성(polymorphism)*을 지원합니다. "다형적(polymorphic)"이라는 단어는 그리스어(πολυμορφος)에서 유래했으며 "여러 형태를 가진"이라는 의미입니다: 다형적인 것은 여러 타입에 대해 동일하게 적용할 수 있습니다.

### Polymorphic Data Types

First, let's see how to declare a polymorphic data type.  
먼저, 다형적 데이터 타입을 선언하는 방법을 살펴봅시다.

```haskell
data List t = E | C t (List t)
```

(We can't reuse `Empty` and `Cons` since we already used those for the constructors of `IntList`, so we'll use `E` and `C` instead.) Whereas before we had `data IntList = ...`, we now have `data List t = ...` The `t` is a *type variable* which can stand for any type. (Type variables must start with a lowercase letter, whereas types must start with uppercase.) `data List t = ...` means that the `List` type is *parameterized* by a type, in much the same way that a function can be parameterized by some input.  
(`IntList`의 생성자에 이미 `Empty`와 `Cons`를 사용했기 때문에 재사용할 수 없으므로 대신 `E`와 `C`를 사용합니다.) 이전에는 `data IntList = ...`가 있었지만, 이제는 `data List t = ...`가 있습니다. 여기서 `t`는 *타입 변수*로, 어떤 타입이든 대체할 수 있습니다. (타입 변수는 소문자로 시작해야 하며, 타입은 대문자로 시작해야 합니다.) `data List t = ...`는 `List` 타입이 어떤 타입에 의해 *매개변수화(parameterized)*되었다는 것을 의미하며, 이는 함수가 일부 입력에 의해 매개변수화될 수 있는 방식과 매우 유사합니다.

Given a type `t`, a `(List t)` consists of either the constructor `E`, or the constructor `C` along with a value of type `t` and another `(List t)`. Here are some examples:  
타입 `t`가 주어지면, `(List t)`는 생성자 `E`이거나, 생성자 `C`와 타입 `t`의 값 그리고 또 다른 `(List t)`로 구성됩니다. 몇 가지 예는 다음과 같습니다:

```haskell
lst1 :: List Int
lst1 = C 3 (C 5 (C 2 E))

lst2 :: List Char
lst2 = C 'x' (C 'y' (C 'z' E))

lst3 :: List Bool
lst3 = C True (C False E)
```

### Polymorphic Functions

Now, let's generalize `filterIntList` to work over our new polymorphic `List`s. We can just take code of `filterIntList` and replace `Empty` by `E` and `Cons` by `C`:  
이제 `filterIntList`를 일반화하여 새로운 다형적 `List`에서 작동하도록 합시다. 우리는 단순히 `filterIntList`의 코드를 가져와 `Empty`를 `E`로, `Cons`를 `C`로 교체하면 됩니다:

```haskell
filterList :: (t -> Bool) -> List t -> List t
filterList _ E = E
filterList p (C x xs)
  | p x       = C x (filterList p xs)
  | otherwise = filterList p xs
```

Now, what is the type of `filterList`? Let's see what type `ghci` infers for it:  
이제, `filterList`의 타입은 무엇일까요? `ghci`가 이를 어떻게 추론하는지 봅시다:

```
*Main> :t filterList
filterList :: (t -> Bool) -> List t -> List t
```

We can read this as: "for any type `t`, `filterList` takes a function from `t` to `Bool`, and a list of `t`s, and returns a list of `t`s."  
이는 다음과 같이 해석할 수 있습니다: "어떤 타입 `t`에 대해서도, `filterList`는 `t`에서 `Bool`로 가는 함수와 `t`들의 리스트를 받아 `t`들의 리스트를 반환합니다."

What about generalizing `mapIntList`? What type should we give to a function `mapList` that applies a function to every element in a `List t`?  
`mapIntList`를 일반화하는 것은 어떨까요? `List t`의 모든 요소에 함수를 적용하는 `mapList` 함수에는 어떤 타입을 줘야 할까요?

Our first idea might be to give it the type  
우리의 첫 번째 생각은 다음과 같은 타입을 부여하는 것일 수 있습니다:

```haskell
mapList :: (t -> t) -> List t -> List t
```

This works, but it means that when applying `mapList`, we always get a list with the same type of elements as the list we started with. This is overly restrictive: we'd like to be able to do things like `mapList show` in order to convert, say, a list of `Int`s into a list of `String`s. Here, then, is the most general possible type for `mapList`, along with an implementation:  
이것은 작동하지만, `mapList`를 적용할 때 항상 시작한 리스트와 동일한 타입의 요소를 가진 리스트를 얻는다는 의미입니다. 이는 지나치게 제한적입니다: 예를 들어, `Int` 리스트를 `String` 리스트로 변환하기 위해 `mapList show`와 같은 작업을 할 수 있기를 원합니다. 따라서, 다음은 `mapList`의 가능한 가장 일반적인 타입과 그 구현입니다:

```haskell
mapList :: (a -> b) -> List a -> List b
mapList _ E        = E
mapList f (C x xs) = C (f x) (mapList f xs)
```

One important thing to remember about polymorphic functions is that **the caller gets to pick the types**. When you write a polymorphic function, it must work for every possible input type. This—together with the fact that Haskell has no way to directly make decisions based on what type something is—has some interesting implications which we'll explore later.  
다형적 함수에 대해 기억해야 할 중요한 점은 **호출자가 타입을 선택할 수 있다는 것**입니다. 다형적 함수를 작성할 때, 그것은 모든 가능한 입력 타입에 대해 작동해야 합니다. 이것은 Haskell이 어떤 타입인지에 따라 직접적으로 결정을 내릴 방법이 없다는 사실과 함께, 나중에 탐구할 몇 가지 흥미로운 함의를 가집니다.

## The Prelude

The `Prelude` is a module with a bunch of standard definitions that gets implicitly imported into every Haskell program. It's worth spending some time [skimming through its documentation](http://haskell.org/ghc/docs/latest/html/libraries/base/Prelude.html) to familiarize oneself with the tools that are available.  
`Prelude`는 모든 Haskell 프로그램에 암묵적으로 임포트되는 여러 표준 정의가 포함된 모듈입니다. 사용할 수 있는 도구에 익숙해지기 위해 [이 문서](http://haskell.org/ghc/docs/latest/html/libraries/base/Prelude.html)를 훑어보는 데 시간을 할애할 가치가 있습니다.

Of course, polymorphic lists are defined in the `Prelude`, along with [many useful polymorphic functions for working with them](http://haskell.org/ghc/docs/latest/html/libraries/base/Prelude.html#11). For example, `filter` and `map` are the counterparts to our `filterList` and `mapList`. In fact, the [`Data.List` module contains many more list functions still](http://www.haskell.org/ghc/docs/latest/html/libraries/base/Data-List.html).   
물론, 다형적 리스트는 `Prelude`에 정의되어 있으며, [이를 다루기 위한 많은 유용한 다형적 함수들도 함께 제공됩니다](http://haskell.org/ghc/docs/latest/html/libraries/base/Prelude.html#11). 예를 들어, `filter`와 `map`은 우리의 `filterList`와 `mapList`의 대응물입니다. 실제로 [`Data.List` 모듈에는 더 많은 리스트 함수들이 포함되어 있습니다](http://www.haskell.org/ghc/docs/latest/html/libraries/base/Data-List.html).

Another useful polymorphic type to know is `Maybe`, defined as  
알아두면 유용한 또 다른 다형적 타입은 다음과 같이 정의된 `Maybe`입니다:

```haskell
data Maybe a = Nothing | Just a
```

A value of type `Maybe a` either contains a value of type `a` (wrapped in the `Just` constructor), or it is `Nothing` (representing some sort of failure or error). The [`Data.Maybe` module has functions for working with `Maybe` values](http://www.haskell.org/ghc/docs/latest/html/libraries/base/Data-Maybe.html).  
`Maybe a` 타입의 값은 타입 `a`의 값을 포함하고 있을 수도 있고 (`Just` 생성자로 감싸인), 아니면 `Nothing`일 수도 있습니다 (`Nothing`은 어떤 종류의 실패나 오류를 나타냅니다). [`Data.Maybe` 모듈에는 `Maybe` 값을 다루기 위한 함수들이 있습니다](http://www.haskell.org/ghc/docs/latest/html/libraries/base/Data-Maybe.html).

## Total and Partial Functions

Consider this polymorphic type:  
이 다형적 타입을 고려해 봅시다:

```haskell
[a] -> a
```

What functions could have such a type? The type says that given a list of things of type `a`, the function must produce some value of type `a`. For example, the Prelude function `head` has this type.  
어떤 함수가 이러한 타입을 가질 수 있을까요? 이 타입은 타입 `a`의 리스트가 주어지면, 함수가 타입 `a`의 어떤 값을 생성해야 한다고 말합니다. 예를 들어, Prelude 함수인 `head`는 이 타입을 가집니다.

But what happens if `head` is given an empty list as input? Let's look at the [source code](http://www.haskell.org/ghc/docs/latest/html/libraries/base/src/GHC-List.html#head) for `head`...  
그러나 `head`에 빈 리스트가 입력으로 주어지면 어떻게 될까요? `head`의 [소스 코드](http://www.haskell.org/ghc/docs/latest/html/libraries/base/src/GHC-List.html#head)를 살펴봅시다...

It crashes! There's nothing else it possibly could do, since it must work for *all* types. There's no way to make up an element of an arbitrary type out of thin air.  
크래시가 발생합니다! 모든 타입에 대해 작동해야 하기 때문에, 다른 방법이 없습니다. 아무데도 없는 상태에서 임의의 타입의 요소를 만들어낼 방법이 없습니다.

`head` is what is known as a *partial function*: there are certain inputs for which `head` will crash. Functions which have certain inputs that will make them recurse infinitely are also called partial. Functions which are well-defined on all possible inputs are known as *total functions*.  
`head`는 *부분 함수*로 알려져 있습니다: `head`가 크래시를 발생시킬 특정 입력이 있습니다. 특정 입력이 함수를 무한히 재귀하게 만드는 함수들도 부분 함수라고 불립니다. 모든 가능한 입력에 대해 잘 정의된 함수는 *총 함수*로 알려져 있습니다.

It is good Haskell practice to avoid partial functions as much as possible. Actually, avoiding partial functions is good practice in *any* programming language—but in most of them it's ridiculously annoying. Haskell tends to make it quite easy and sensible.  
Haskell에서는 가능한 한 부분 함수를 피하는 것이 좋은 관행입니다. 실제로, 부분 함수를 피하는 것은 *모든* 프로그래밍 언어에서 좋은 관행이지만, 대부분의 언어에서는 이를 피하는 것이 매우 짜증스럽습니다. Haskell은 이를 상당히 쉽고 합리적인 일로 만듭니다.

**`head` is a mistake!** It should not be in the `Prelude`. Other partial `Prelude` functions you should almost never use include `tail`, `init`, `last`, and `(!!)`.  
**`head`는 실수입니다!** `Prelude`에 있어서는 안 됩니다. 거의 사용해서는 안 되는 다른 부분 `Prelude` 함수로는 `tail`, `init`, `last`, `(!!)` 등이 있습니다.

From this point on, using one of these functions on a homework assignment will lose style points!  
이 시점부터, 과제에서 이러한 함수 중 하나를 사용하는 것은 스타일 점수를 잃게 될 것입니다!

### What to Do Instead?

#### Replacing Partial Functions

Often partial functions like `head`, `tail`, and so on can be replaced by pattern-matching. Consider the following two definitions:  
종종 `head`, `tail`과 같은 부분 함수는 패턴 매칭으로 대체할 수 있습니다. 다음 두 정의를 고려해 봅시다:

```haskell
doStuff1 :: [Int] -> Int
doStuff1 []  = 0
doStuff1 [_] = 0
doStuff1 xs  = head xs + (head (tail xs)) 
```

```haskell
doStuff2 :: [Int] -> Int
doStuff2 []        = 0
doStuff2 [_]       = 0
doStuff2 (x1:x2:_) = x1 + x2
```

These functions compute exactly the same result, and they are both total. But only the second one is *obviously* total, and it is much easier to read anyway.  
이 함수들은 정확히 동일한 결과를 계산하며, 둘 다 총 함수입니다. 그러나 두 번째 함수만이 *명백하게* 총 함수이며, 읽기도 훨씬 더 쉽습니다.

#### Writing Partial Functions

What if you find yourself *writing* a partial function? There are two approaches to take. The first is to change the output type of the function to indicate the possible failure. Recall the definition of `Maybe`:  
만약 자신이 *부분 함수*를 작성하고 있다고 느낀다면? 두 가지 접근 방식이 있습니다. 첫 번째는 함수의 출력 타입을 변경하여 가능한 실패를 나타내는 것입니다. `Maybe`의 정의를 기억하세요:

```haskell
data Maybe a = Nothing | Just a
```

Now, suppose we were writing `head`. We could rewrite it safely like this:  
이제, 우리가 `head`를 작성하고 있다고 가정해 봅시다. 우리는 이를 다음과 같이 안전하게 다시 작성할 수 있습니다:

```haskell
safeHead :: [a] -> Maybe a
safeHead []    = Nothing
safeHead (x:_) = Just x
```

Indeed, there is exactly such a function defined in the [`safe` package](http://hackage.haskell.org/package/safe).  
실제로, [`safe` 패키지](http://hackage.haskell.org/package/safe)에 정확히 그런 함수가 정의되어 있습니다.

Why is this a good idea?  
왜 이것이 좋은 생각일까요?

1. `safeHead` will never crash.  
`safeHead`는 절대 크래시되지 않습니다.
2. The type of `safeHead` makes it obvious that it may fail for some inputs.  
`safeHead`의 타입은 일부 입력에 대해 실패할 수 있음을 명확하게 합니다.
3. The type system ensures that users of `safeHead` must appropriately check the return value of `safeHead` to see whether they got a value or `Nothing`.  
타입 시스템은 `safeHead`의 사용자가 값을 받았는지 `Nothing`을 받았는지 적절히 확인하도록 보장합니다.

In some sense, `safeHead` is still "partial"; but we have reflected the partiality in the type system, so it is now safe. The goal is to have the types tell us as much as possible about the behavior of functions.  
어떤 면에서, `safeHead`는 여전히 "부분적"입니다; 하지만 우리는 타입 시스템에 부분성을 반영했기 때문에 이제 안전합니다. 목표는 타입이 함수의 동작에 대해 가능한 한 많이 알려주도록 하는 것입니다.

OK, but what if we know that we will only use `head` in situations where we are *guaranteed* to have a non-empty list? In such a situation, it is really annoying to get back a `Maybe a`, since we have to expend effort dealing with a case which we "know" cannot actually happen.  
알겠습니다, 하지만 만약 우리가 리스트가 비어 있지 않다는 것이 *보장된* 상황에서만 `head`를 사용할 것이라는 것을 안다면 어떨까요? 그런 상황에서는 실제로 발생하지 않을 것이라고 "알고 있는" 경우를 처리해야 하기 때문에 `Maybe a`를 반환받는 것이 정말 짜증나는 일이 됩니다.

The answer is that if some condition is really *guaranteed*, then the types ought to reflect the guarantee! Then the compiler can enforce your guarantees for you. For example:  
답은 어떤 조건이 정말로 *보장된다면*, 타입이 그 보장을 반영해야 한다는 것입니다! 그러면 컴파일러가 당신의 보장을 강제할 수 있습니다. 예를 들어:

```haskell
data NonEmptyList a = NEL a [a]

nelToList :: NonEmptyList a -> [a]
nelToList (NEL x xs) = x:xs

listToNel :: [a] -> Maybe (NonEmptyList a)
listToNel []     = Nothing
listToNel (x:xs) = Just $ NEL x xs

headNEL :: NonEmptyList a -> a
headNEL (NEL a _) = a

tailNEL :: NonEmptyList a -> [a]
tailNEL (NEL _ as) = as
```

You might think doing such things is only for chumps who are not coding super-geniuses like you. Of course, *you* would never make a mistake like passing an empty list to a function which expects only non-empty ones. Right? Well, there's definitely a chump involved, but it's not who you think.  
당신은 이런 것을 하는 것이 당신과 같은 슈퍼 천재가 아닌 바보들에게만 해당된다고 생각할 수 있습니다. 물론, *당신*은 비어 있지 않은 리스트만을 기대하는 함수에 빈 리스트를 전달하는 실수를 결코 하지 않을 것입니다. 그렇죠? 음, 분명히 바보가 관련되어 있지만, 당신이 생각하는 그런 바보는 아닙니다.

---
