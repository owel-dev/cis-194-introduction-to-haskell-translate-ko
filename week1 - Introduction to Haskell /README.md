# Haskell Basics

> CIS 194 Week 1 - 14 January 2013

Suggested reading:

- [Learn You a Haskell for Great Good, chapter 2](http://learnyouahaskell.com/starting-out)
- [Real World Haskell](http://book.realworldhaskell.org/), chapters 1 and 2

## What is Haskell?

Haskell is a _lazy, functional_ programming language created in the late 1980's by a committee of academics. There were a plethora of lazy functional languages around, everyone had their favorite, and it was hard to communicate ideas. So a bunch of people got together and designed a new language, taking some of the best ideas from existing languages (and a few new ideas of their own). Haskell was born.  
하스켈은 1980년대 후반에 학자들로 구성된 위원회에 의해 만들어진 _지연 평가, 함수형_ 프로그래밍 언어입니다. 당시에는 수많은 지연 평가 함수형 언어들이 있었고, 업계 사람들은 각기 선호하는 언어가 달라 아이디어를 소통하는데 어려움이 있었습니다. 이러한 불편을 해결하고자 몇몇 사람이 모여 기존 언어들의 장점에 추가적인 아이디어들을 더하여 새로운 언어를 설계했고, 하스켈이 탄생했습니다.

<img src="images/haskell-logo-small.png" />

So what is Haskell like? Haskell is:  
그렇다면 하스켈은 어떤 언어일까요?

### Functional

<img src="images/function-machine.png" width="300px" />

There is no precise, accepted meaning for the term "functional". But when we say that Haskell is a _functional_ language, we usually have in mind two things:    
"함수형(functional)"이라는 용어에 대한 정확하고 일반적인 정의는 없습니다. 하지만 우리가 하스켈이 _함수형_ 언어라고 할 때, 보통 두 가지를 염두에 둡니다:
- Functions are _first-class_, that is, functions are values which can be used in exactly the same ways as any other sort of value.  
함수는 _일급(first-class)_ 객체입니다. 즉, 함수는 다른 모든 종류의 값과 동일한 방식으로 사용할 수 있는 값입니다.
- The meaning of Haskell programs is centered around _evaluating expressions_ rather than _executing instructions_.  
하스켈 프로그램의 의미는 _명령 실행(executing instructions)_ 보다는 _식 평가(evaluating expressions)_ 에 중점을 둡니다.

Taken together, these result in an entirely different way of thinking about programming. Much of our time this semester will be spent exploring this way of thinking.  
위 두 가지를 받아들인다면, 프로그래밍에 대한 전혀 다른 사고 방식을 갖게 됩니다. 이번 학기 동안 우리는 이러한 사고 방식을 탐구하는 데 많은 시간을 할애할 것입니다.

### Pure

<img src="images/pure.jpg" width="200px" />

Haskell expressions are always _referentially transparent_, that is:  
하스켈의 표현식은 항상 _참조 투명성_ 을 가집니다. 즉:
- No mutation! Everything (variables, data structures...) is _immutable_.  
변이가 없습니다! 모든 것(변수, 데이터 구조 등...)은 _불변_ 입니다.
- Expressions never have "side effects" (like updating global variables or printing to the screen).  
표현식은 절대 "부작용"을 가지지 않습니다 (예: 전역 변수 업데이트나 화면에 출력하는 것).
- Calling the same function with the same arguments results in the same output every time.  
동일한 함수에 동일한 인수를 호출하면 매번 동일한 결과가 나옵니다.

This may sound crazy at this point. How is it even possible to get anything done without mutation or side effects? Well, it certainly requires a shift in thinking (if you're used to an imperative or object-oriented paradigm). But once you've made the shift, there are a number of wonderful benefits:  
지금은 이것이 미친 소리처럼 들릴 수 있습니다. 어떻게 변이나 부작용 없이 동작하는게 가능할까요? 물론, 사고 방식을 전환해야 합니다(명령형이나 객체 지향 패러다임에 익숙하다면). 하지만 일단 전환하고나면 여러 훌륭한 이점을 얻게됩니다.
- _Equational reasoning and refactoring_: In Haskell one can always "replace equals by equals", just like you learned in algebra class.  
_등식적 추론과 리팩토링_: 하스켈에서는 대수학 수업에서 배운 것처럼 "등식 대체 원리"가 적용됩니다.
- _Parallelism_: Evaluating expressions in parallel is easy when they are guaranteed not to affect one another.  
_병렬성_: 표현식들이 서로 영향을 주지 않는다는 것이 보장될 때, 병렬로 표현식을 평가하기 쉽습니다.
- _Fewer headaches_: Simply put, unrestricted effects and action-at-a-distance makes for programs that are hard to debug, maintain, and reason about.  
_문제 해결이 쉬움_: 간단히 말해, 무제한적인 부작용과 먼 거리에서의 작용은 디버깅, 유지보수, 추론이 어려운 프로그램을 만듭니다.

### Lazy

<img src="images/relax.jpg" width="200px" />

In Haskell, expressions are _not evaluated until their results are actually needed_. This is a simple decision with far-reaching consequences, which we will explore throughout the semester. Some of the consequences include:  
하스켈에서는 표현식이 _결과가 실제로 필요할 때까지 평가되지 않습니다_. 이 단순한 결정은 광범위한 영향을 미치며, 우리는 이번 학기 동안 이를 탐구할 것입니다. 그 결과 중 일부는 다음과 같습니다:
- It is easy to define a new _control structure_ just by defining a function.  
함수를 정의함으로써 새로운 _제어 구조_ 를 쉽게 정의할 수 있습니다.
- It is possible to define and work with _infinite data structures_.  
_무한 데이터 구조_ 를 정의하고 작업하는 것이 가능합니다.
- It enables a more compositional programming style (see _wholemeal programming_ below).  
보다 조합적인 프로그래밍 스타일을 가능하게 합니다 (이후 _wholemeal programming_ 단락에서 추가적으로 설명합니다).
- One major downside, however, is that reasoning about time and space usage becomes much more complicated!  
그러나 한 가지 주요 단점은 시간 및 공간 사용에 관한 추론이 훨씬 더 복잡해진다는 것입니다!

### Statically typed

<img src="images/static.jpg" width="200px" />

Every Haskell expression has a type, and types are all checked at _compile-time_. Programs with type errors will not even compile, much less run.  
모든 하스켈 표현식은 타입을 가지며, 타입은 모두 _컴파일 타임_ 에 검사됩니다. 타입 오류가 있는 프로그램은 컴파일되지도 않고, 실행될 수도 없습니다.

## Themes

Throughout this course, we will focus on three main themes.  
이번 과정에서 우리는 세 가지 주요 주제에 집중할 것입니다.

### Types

Static type systems can seem annoying. In fact, in languages like C++ and Java, they _are_ annoying. But this isn't because static type systems _per se_ are annoying; it's because C++ and Java's type systems are insufficiently expressive! This semester we'll take a close look at Haskell's type system, which  
정적 타입 시스템은 짜증나 보일 수 있습니다. 실제로 C++나 Java 같은 언어에서는 짜증납니다. 하지만 이는 정적 타입 시스템 자체가 짜증나서 그런 것이 아니라, C++와 Java의 타입 시스템이 충분히 표현력이 없기 때문입니다! 이번 학기에는 하스켈의 타입 시스템을 면밀히 살펴볼 것입니다. 하스켈의 타입 시스템은
- _Helps clarify thinking and express program structure_

  The first step in writing a Haskell program is usually to _write down all the types_. Because Haskell's type system is so expressive, this is a non-trivial design step and is an immense help in clarifying one's thinking about the program.

  _생각을 명확하게 하고 프로그램 구조를 표현하는 데 도움이 됨_

  하스켈 프로그램을 작성하는 첫 번째 단계는 보통 _모든 타입을 작성하는 것_ 입니다. 하스켈의 타입 시스템이 매우 표현력이 뛰어나기 때문에, 이것은 사소하지 않은 설계 단계이며 프로그램에 대한 사고를 명확하게 하는 데 큰 도움이 됩니다.

- _Serves as a form of documentation_

  Given an expressive type system, just looking at a function's type tells you a lot about what the function might do and how it can be used, even before you have read a single word of written documentation.

  _문서화 형태로서의 역할_

  표현력이 뛰어난 타입 시스템 덕분에, 함수의 타입만 봐도 그 함수가 무엇을 할 수 있는지, 어떻게 사용할 수 있는지에 대해 많은 것을 알 수 있습니다. 문서의 한 단어를 읽기도 전에 말이죠.

- _Turns run-time errors into compile-time errors_

  It's much better to be able to fix errors up front than to just test a lot and hope for the best. "If it compiles, it must be correct" is mostly facetious (it's still quite possible to have errors in logic even in a type-correct program), but it happens in Haskell much more than in other languages.

  _런타임 오류를 컴파일 타임 오류로 전환_

  많은 테스트를 하고 문제가 없기를 바라는 것보다, 미리 오류를 수정할 수 있는 것이 훨씬 낫습니다. "컴파일이 되면 올바르게 동작할 것이다" 라는 말은 대체로 잘못된 말이지만 (type-correct 프로그램에서도 논리적 오류가 발생할 가능성은 높습니다), 다른 언어들과 비교했을때 하스켈에서는 자주 맞는 말입니다.

### Abstraction

"Don't Repeat Yourself" is a mantra often heard in the world of programming. Also known as the "Abstraction Principle", the idea is that nothing should be duplicated: every idea, algorithm, and piece of data should occur exactly once in your code. Taking similar pieces of code and factoring out their commonality is known as the process of _abstraction_.  
프로그래밍 세계에서 자주 들을 수 있는 만트라는 "반복하지 마라"입니다. "추상화 원칙"이라고도 알려진 이 아이디어는 아무것도 중복되어서는 안 된다는 것입니다: 모든 아이디어, 알고리즘, 데이터 조각은 코드에서 정확히 한 번만 나타나야 합니다. 유사한 코드 조각을 가져와서 그 공통점을 추출하는 과정을 _추상화_ 라고 합니다.

Haskell is very good at abstraction: features like parametric polymorphism, higher-order functions, and type classes all aid in the fight against repetition. Our journey through Haskell this semester will in large part be a journey from the specific to the abstract.  
하스켈은 추상화하기 좋습니다: 파라메트릭 다형성, 고차 함수, 타입 클래스와 같은 기능들은 반복을 줄이는 데 도움이 됩니다. 이번 학기 하스켈을 배우는 과정은 대부분 구체적인 것에서 추상적인 것으로의 여정이 될 것입니다.

### Wholemeal programming

Another theme we will explore is _wholemeal programming_. A quote from Ralf Hinze:  
또 다른 주제로 우리는 _wholemeal programming_ 을 탐구할 것입니다.  아래는 Ralf Hinze의 인용문입니다:
> "Functional languages excel at wholemeal programming, a term coined by Geraint Jones. Wholemeal programming means to think big: work with an entire list, rather than a sequence of elements; develop a solution space, rather than an individual solution; imagine a graph, rather than a single path. The wholemeal approach often offers new insights or provides new perspectives on a given problem. It is nicely complemented by the idea of projective programming: first solve a more general problem, then extract the interesting bits and pieces by transforming the general program into more specialised ones."  
> "함수형 언어는 Geraint Jones가 만든 용어인 전체적인 프로그래밍에 탁월합니다. 전체적인 프로그래밍은 크게 생각하는 것을 의미합니다: 요소들의 순서 대신 전체 리스트를 다루고; 개별적인 해결책 대신 해결 공간을 개발하며; 단일 경로 대신 그래프를 상상하는 것입니다. 전체적인 접근 방식은 주어진 문제에 대해 새로운 통찰력을 제공하거나 새로운 관점을 제공합니다. 이는 투사 프로그래밍이라는 아이디어와 잘 보완됩니다: 먼저 더 일반적인 문제를 해결한 다음, 일반 프로그램을 보다 전문화된 프로그램으로 변환하여 흥미로운 부분들을 추출하는 것입니다."

For example, consider this pseudocode in a C/Java-ish sort of language:
예를 들어, C/Java와 유사한 언어로 작성된 이 의사 코드를 보세요:

```c
int acc = 0;
for ( int i = 0; i < lst.length; i++ ) {
  acc = acc + 3 * lst[i];
}
```

This code suffers from what Richard Bird refers to as "indexitis": it has to worry about the low-level details of iterating over an array by keeping track of a current index. It also mixes together what can more usefully be thought of as two separate operations: multiplying every item in a list by 3, and summing the results.  
이 코드는 Richard Bird가 "indexitis"라고 부르는 문제를 겪고 있습니다: 현재 인덱스를 추적하면서 배열을 반복(iterating)하는 저수준의 세부 사항을 신경 써야 합니다. 또한 리스트의 모든 항목에 3을 곱하는 것과 결과를 합산하는 두 개의 별도 작업이 하나로 합쳐져 있습니다: 

In Haskell, we can just write  
하스켈에서는 단순히 다음과 같이 쓸 수 있습니다:

```haskell
sum (map (3*) lst)
```

This semester we'll explore the shift in thinking represented by this way of programming, and examine how and why Haskell makes it possible.  
이번 학기에는 이러한 프로그래밍 방식으로의 사고의 전환에 대해 탐구하고, 하스켈이 어떻게 그리고 왜 이를 가능하게 하는지에 대해서도 살펴볼 것입니다.

## Literate Haskell

This file is a "literate Haskell documents": only lines preceded by `>` and a space (see below) are code; everything else (like this paragraph) is a comment. Your programming assignments do not have to be literate Haskell, although they may be if you like. Literate Haskell documents have an extension of `.lhs`, whereas non-literate Haskell source files use `.hs`.  
이 파일은 "literate Haskell documents" 입니다: `>`와 공백으로 시작하는 줄만 코드이며(아래 참조), 그 외의 모든 것은 주석입니다(이 단락과 같은). 프로그래밍 과제는 반드시 **literate Haskell documents** 일 필요는 없지만, 원한다면 그렇게 할 수 있습니다. **literate Haskell documents는** `.lhs` 확장자를 가지며, **non-literate** 하스켈 소스 파일은 `.hs`를 사용합니다.

## Declarations and variables

Here is some Haskell code:  
다음은 하스켈 코드입니다:

```haskell
x :: Int
x = 3

-- Note that normal (non-literate) comments are preceded by two hyphens
{- or enclosed
   in curly brace/hyphen pairs. -}
```

The above code declares a variable `x` with type `Int` (`::` is pronounced "has type") and declares the value of `x` to be `3`. Note that _this will be the value of `x` forever_ (at least, in this particular program). The value of `x` cannot be changed later.  
위 코드는 타입이 `Int`인 변수 `x`를 선언하며 (`::`는 "has type"으로 발음됩니다), `x`의 값을 `3`으로 선언합니다. _이 값은 영원히 `x`의 값이 됩니다_ (적어도 이 특정 프로그램에서는 그렇습니다). 이후에 `x`의 값을 변경할 수 없습니다.

Try uncommenting the line below; it will generate an error saying something like `Multiple declarations of 'x'`.  
아래 줄의 주석을 해제해 보세요; `Multiple declarations of 'x'`와 같은 오류가 발생할 것입니다.

```haskell
-- x = 4
```

In Haskell, _variables are not mutable boxes_; they are just names for values!  
하스켈에서는 _변수는 변경 가능한 상자(mutables boxes)_ 가 아닙니다; 단지 값에 대한 이름일 뿐입니다!

Put another way, `=` does _not_ denote "assignment" like it does in many other languages. Instead, `=` denotes _definition_, like it does in mathematics. That is, `x = 4` should not be read as "`x` gets `4`" or "assign `4` to `x`", but as "`x` is _defined to be_ `4`".  
다시 말해, `=`는 많은 다른 언어에서 그렇듯이 "할당"을 나타내지 않습니다. 대신, `=`는 수학에서처럼 _정의_ 를 나타냅니다. 즉, `x = 4`는 "`x`가 `4`를 받는다"거나 "`4` 할당"으로 읽어서는 안 되며, "`x`는 `4`로 _정의된다"로 읽어야 합니다.

What do you think this code means?  
이 코드가 의미하는 바는 무엇일까요?

```haskell
y :: Int
y = y + 1
```

## Basic Types

```haskell
-- Machine-sized integers
i :: Int
i = -78
```

`Int`s are guaranteed by the Haskell language standard to accommodate values at least up to ±2^29, but the exact size depends on your architecture. For example, on my 64-bit machine the range is ±2^63. You can find the range on your machine by evaluating the following:  
하스켈 언어 표준에 의해 `Int`는 최소한 ±2^29 범위의 값을 수용할 수 있도록 보장되지만, 정확한 크기는 사용자의 아키텍처에 따라 다릅니다. 예를 들어, 64비트 머신에서는 ±2^63의 범위를 가집니다. 다음과 같은 코드를 평가하여 자신의 아키텍처에서의 범위를 확인할 수 있습니다:

```haskell
biggestInt, smallestInt :: Int
biggestInt  = maxBound
smallestInt = minBound
```

(Note that idiomatic Haskell uses `camelCase` for identifier names. If you don't like it, tough luck.)  
(참고로, 관용적인 하스켈에서는 식별자 이름에 `camelCase`를 사용합니다. 마음에 들지 않는다면 안타깝습니다.)

The `Integer` type, on the other hand, is limited only by the amount of memory on your machine.
반면에 `Integer` 타입은 당신 컴퓨터의 메모리 용량에 의해서만 제한됩니다.


```haskell
-- Arbitrary-precision integers
n :: Integer
n = 1234567890987654321987340982334987349872349874534

reallyBig :: Integer
reallyBig = 2^(2^(2^(2^2)))

numDigits :: Int
numDigits = length (show reallyBig)
```

For floating-point numbers, there is `Double`:  
부동 소수점 숫자에는 `Double`이 있습니다:

```haskell
-- Double-precision floating point
d1, d2 :: Double
d1 = 4.5387
d2 = 6.2831e-4
```

There is also a single-precision floating point number type, `Float`.  
또한 단일 정밀도 부동 소수점 숫자 타입인 `Float`도 있습니다.

Finally, there are booleans, characters, and strings:  
마지막으로, 불리언, 문자, 그리고 문자열이 있습니다:

```haskell
-- Booleans
b1, b2 :: Bool
b1 = True
b2 = False

-- Unicode characters
c1, c2, c3 :: Char
c1 = 'x'
c2 = '횠'
c3 = '😊'

-- Strings are lists of characters with special syntax
s :: String
s = "Hello, Haskell!"
```

## GHCi

GHCi is an interactive Haskell REPL (Read-Eval-Print-Loop) that comes with GHC. At the GHCi prompt, you can evaluate expressions, load Haskell files with `:load` (`:l`) (and reload them with `:reload` (`:r`)), ask for the type of an expression with `:type` (`:t`), and many other things (try `:?` for a list of commands).  
GHCi는 GHC와 함께 제공되는 대화형 하스켈 REPL(Read-Eval-Print-Loop)입니다. GHCi 프롬프트에서 표현식을 평가하고, `:load` (`:l`)를 사용하여 하스켈 파일을 로드하며 (`:reload` (`:r`)로 다시 로드), `:type` (`:t`)을 사용하여 표현식의 타입을 물어볼 수 있고, 많은 다른 작업도 할 수 있습니다(`:?`를 시도하여 명령 목록을 확인).

## Arithmetic

Try evaluating each of the following expressions in GHCi:  
다음 각 표현식을 GHCi에서 평가해 보세요:

```haskell
ex01 = 3 + 2
ex02 = 19 - 27
ex03 = 2.35 * 8.6
ex04 = 8.7 / 3.1
ex05 = mod 19 3
ex06 = 19 `mod` 3
ex07 = 7 ^ 222
exNN = (-3) * (-7)
```

Note how `backticks` make a function name into an infix operator. Note also that negative numbers must often be surrounded by parentheses, to avoid having the negation sign parsed as subtraction. (Yes, this is ugly. I'm sorry.)  
함수 이름을 중위 연산자로 만드는 `백틱(backticks)` 사용법을 주목하세요. 또한 음수는 종종 괄호로 감싸야 한다는 점도 주목하세요, 그렇지 않으면 -(음수) 기호가 뺄셈으로 해석될 수 있기 때문입니다. (네, 이것은 보기 흉합니다. 죄송합니다.)

This, however, gives an error:  
그러나, 다음은 오류를 발생시킵니다:

```haskell
-- badArith1 = i + n
```

Addition is only between values of the same numeric type, and Haskell does not do implicit conversion. You must explicitly convert with:  
덧셈은 동일한 숫자 타입의 값들 사이에서만 가능하며, 하스켈은 암묵적인 변환을 하지 않습니다. 명시적으로 다음과 같이 변환해야 합니다:

- `fromIntegral`: converts from any integral type (`Int` or `Integer`) to any other numeric type.  
`fromIntegral`: 모든 정수 타입(`Int` 또는 `Integer`)에서 다른 숫자 타입으로 변환합니다.
- `round`, `floor`, `ceiling`: convert floating-point numbers to `Int` or `Integer`.  
`round`, `floor`, `ceiling`: 부동 소수점 숫자를 `Int` 또는 `Integer`로 변환합니다.

Now try this:
이제 다음을 시도해 보세요:

```haskell
-- badArith2 = i / i
```

This is an error since `/` performs floating-point division only. For integer division we can use `div`.  
이는 `/`가 부동 소수점 나눗셈만 수행할 수 있기에 오류가 발생하는 것입니다. 정수 나눗셈을 위해서는 `div`를 사용해야 합니다.

```haskell
ex08 = i `div` i
ex09 = 12 `div` 5
```

If you are used to other languages which do implicit conversion of numeric types, this can all seem rather prudish and annoying at first. However, I promise you'll get used to it—and in time you may even come to appreciate it. Implicit numeric conversion encourages sloppy thinking about numeric code.  
만약 당신이 숫자 타입의 암묵적 변환을 하는 다른 언어에 익숙하다면, 이것은 처음에는 다소 과분하고 짜증나 보일 수 있습니다. 하지만, 제가 약속합니다, 당신은 익숙해질 것이고—시간이 지나면 심지어 그것을 감사하게 여길 수도 있습니다. 암묵적 숫자 변환은 숫자 코드에 대한 부주의한 사고를 조장합니다.

## Boolean logic

As you would expect, Boolean values can be combined with `(&&)` (logical and), `(||)` (logical or), and `not`. For example,  
예상대로, 불리언 값들은 `(&&)` (논리적 AND), `(||)` (논리적 OR), 그리고 `not`과 결합될 수 있습니다. 예를 들어,

```haskell
ex10 = True && False
ex11 = not (False || True)
```

Things can be compared for equality with `(==)` and `(/=)`, or compared for order using `(<)`, `(>)`, `(<=)`, and `(>=)`.    
값들은 `(==)`와 `(/=)`을 사용하여 동등성을 비교하거나, `(<)`, `(>)`, `(<=)`, 그리고 `(>=)`을 사용하여 순서를 비교할 수 있습니다.

```haskell
ex12 = ('a' == 'a')
ex13 = (16 /= 3)
ex14 = (5 > 3) && ('p' <= 'q')
ex15 = "Haskell" > "C++"
```

Haskell also has `if`-expressions: `if b then t else f` is an expression which evaluates to `t` if the Boolean expression `b` evaluates to `True`, and `f` if `b` evaluates to `False`. Notice that `if`-_expressions_ are very different than `if`-_statements_. For example, with an if-statement, the `else` part can be optional; an omitted `else` clause means "if the test evaluates to `False` then do nothing". With an `if`-expression, on the other hand, the `else` part is required, since the `if`-expression must result in some value.  
하스켈에는 `if`-표현식도 있습니다: `if b then t else f`는 불리언 표현식 `b`가 `True`로 평가되면 `t`를, `False`로 평가되면 `f`를 평가하는 표현식입니다. `if`-_표현식_ 이 `if`-_문_ 과 매우 다르다는 점에 주목하세요. 예를 들어, if-문에서는 `else` 부분이 선택적일 수 있습니다; `else` 절이 생략되면 "테스트가 `False`로 평가되면 아무 것도 하지 않는다"는 의미입니다. 반면에 `if`-표현식에서는 `if`-표현식이 어떤 값을 결과로 내야 하기 때문에 `else` 부분이 필수적입니다.

Idiomatic Haskell does not use `if` expressions very much, often using pattern-matching or _guards_ instead (see the next section).  
관용적인 하스켈에서는 `if` 표현식을 많이 사용하는 대신, 패턴 매칭(pattern-matching)이나 _가드_ 를 사용합니다 (다음 섹션 참조).

## Defining basic functions

We can write functions on integers by cases.  
우리는 케이스별로 정수에 대한 함수를 작성할 수 있습니다.

```haskell
-- Compute the sum of the integers from 1 to n.
sumtorial :: Integer -> Integer
sumtorial 0 = 0
sumtorial n = n + sumtorial (n-1)
```

Note the syntax for the type of a function: `sumtorial :: Integer -> Integer` says that `sumtorial` is a function which takes an `Integer` as input and yields another `Integer` as output.  
함수의 타입에 대한 문법에 주목하세요: `sumtorial :: Integer -> Integer`는 `sumtorial`이 `Integer`를 입력으로 받아 또 다른 `Integer`를 출력하는 함수임을 나타냅니다.  

Each clause is checked in order from top to bottom, and the first matching clause is chosen. For example, `sumtorial 0` evaluates to `0`, since the first clause is matched. `sumtorial 3` does not match the first clause (`3` is not `0`), so the second clause is tried. A variable like `n` matches anything, so the second clause matches and `sumtorial 3` evaluates to `3 + sumtorial (3-1)` (which can then be evaluated further).  
각 절은 위에서 아래로 순서대로 검사되며, 첫 번째로 일치하는 절이 선택됩니다. 예를 들어, `sumtorial 0`은 첫 번째 절과 일치하므로 `0`으로 평가됩니다. `sumtorial 3`은 첫 번째 절과 일치하지 않으므로 (`3`은 `0`이 아니기 때문에), 두 번째 절이 시도됩니다. `n`과 같은 변수는 무엇이든 일치하므로 두 번째 절과 일치하며, `sumtorial 3`은 `3 + sumtorial (3-1)`로 평가됩니다(이후 추가로 평가될 수 있습니다).

Choices can also be made based on arbitrary Boolean expressions using _guards_. For example:  
_가드_ 를 사용하여 임의의 불리언 표현식에 기반한 선택도 할 수 있습니다. 예를 들어:

```haskell
hailstone :: Integer -> Integer
hailstone n
  | n `mod` 2 == 0 = n `div` 2
  | otherwise      = 3 * n + 1
```

Any number of guards can be associated with each clause of a function definition, each of which is a Boolean expression. If the clause's patterns match, the guards are evaluated in order from top to bottom, and the first one which evaluates to `True` is chosen. If none of the guards evaluate to `True`, matching continues with the next clause.  
함수 정의의 각 절에 여러 개의 가드를 연결할 수 있으며, 각각은 불리언 표현식입니다. 절의 패턴이 일치하면, 가드들이 위에서 아래로 순서대로 평가되며, 처음으로 `True`로 평가되는 가드가 선택됩니다. 어떤 가드도 `True`로 평가되지 않으면, 다음 절과의 일치가 계속됩니다.

For example, suppose we evaluate `hailstone 3`. First, `3` is matched against `n`, which succeeds (since a variable matches anything). Next, ``n `mod` 2 == 0`` is evaluated; it is `False` since `n = 3` does not result in a remainder of `0` when divided by `2`. otherwise is just a convenient synonym for `True`, so the second guard is chosen, and the result of `hailstone 3` is thus `3 * 3 + 1 = 10`.  
예를 들어, `hailstone 3`을 평가한다고 가정해 봅시다. 먼저, `3`은 `n`과 일치하며, 이는 성공합니다 (변수는 무엇이든 일치하기 때문입니다). 다음으로, ``n `mod` 2 == 0``이 평가됩니다; 이는 `False`입니다, 왜냐하면 `n = 3`은 `2`로 나눌 때 나머지가 `0`이 아니기 때문입니다. `otherwise`는 단지 `True`의 편리한 동의어일 뿐이므로, 두 번째 가드가 선택되며, `hailstone 3`의 결과는 `3 * 3 + 1 = 10`이 됩니다.

As a more complex (but more contrived) example:  
좀 더 복잡한 (하지만 더 인위적인) 예로:

```haskell
foo :: Integer -> Integer
foo 0 = 16
foo 1
  | "Haskell" > "C++" = 3
  | otherwise         = 4
foo n
  | n < 0            = 0
  | n `mod` 17 == 2  = -43
  | otherwise        = n + 3
```

What is `foo (-3)`? `foo 0`? `foo 1`? `foo 36`? `foo 38`?  
`foo (-3)`는 무엇일까요? `foo 0`은? `foo 1`은? `foo 36`은? `foo 38`은?

As a final note about Boolean expressions and guards, suppose we wanted to abstract out the test of evenness used in defining `hailstone`. A first attempt is shown below:  
불리언 표현식과 가드에 대한 마지막 주의 사항으로, `hailstone`을 정의할 때 사용된 짝수성(evenness) 테스트를 추상화하고 싶다고 가정해 봅시다. 첫 시도는 아래에 나타나 있습니다:

```haskell
isEven :: Integer -> Bool
isEven n
  | n `mod` 2 == 0 = True
  | otherwise      = False
```

This _works_, but it is much too complicated. Can you see why?  
이것은 _작동_ 하지만, 너무 복잡합니다. 왜 그런지 알 수 있나요?

## Pairs

We can pair things together like so:  
다음과 같이 두 데이터를 쌍으로 만들 수도 있습니다:


```haskell
p :: (Int, Char)
p = (3, 'x')
```

Notice that the `(x, y)` notation is used both for the _type_ of a pair and a pair _value_.  
쌍의 타입과 쌍의 값 모두에 대해 `(x, y)` 표기법이 사용된 것을 주목하세요.

The elements of a pair can be extracted again with _pattern matching_:  
쌍의 요소들은 _패턴 매칭_ 을 사용하여 다시 추출할 수 있습니다:

---

```haskell
sumPair :: (Int, Int) -> Int
sumPair (x, y) = x + y
```

Haskell also has triples, quadruples, ... but you should never use them. As we'll see next week, there are much better ways to package three or more pieces of information together.  
하스켈에는 triples, quadruples 등이 있지만, 절대 사용해서는 안 됩니다. 다음 주에 볼 것처럼, 세 개 이상의 정보를 함께 패키징하는 훨씬 더 좋은 방법들이 있습니다.

## Using functions, and multiple arguments

To apply a function to some arguments, just list the arguments after the function, separated by spaces, like this:  
함수에 몇 가지 인수를 적용하려면, 함수 뒤에 공백으로 구분된 인수들을 나열하면 됩니다, 다음과 같이:

```haskell
f :: Int -> Int -> Int -> Int
f x y z = x + y + z
exFF = f 3 17 8
```

The above example applies the function `f` to the three arguments `3`, `17`, and `8`. Note also the syntax for the type of a function with multiple arguments, like `Arg1Type -> Arg2Type -> ... -> ResultType`. This might seem strange to you (and it should!). Why all the arrows? Wouldn't it make more sense for the type of `f` to be something like `Int Int Int -> Int`? Actually, the syntax is no accident: it is the way it is for a very deep and beautiful reason, which we'll learn about in a few weeks; for now you just have to take my word for it!  
위 예제는 함수 `f`에 세 개의 인수 `3`, `17`, 그리고 `8`을 적용합니다. 또한 `Arg1Type -> Arg2Type -> ... -> ResultType`과 같은 다중 인수를 가진 함수의 타입 문법에 주목하세요. 이는 당신에게 이상하게 보일 수 있습니다(그럴 것입니다!). 이 화살표들은 무엇일까요? `f`의 타입이 `Int Int Int -> Int`와 같은 형태인 것이 더 말이 되지 않을까요? 실제로, 이 문법은 우연이 아닙니다: 매우 깊고 아름다운 이유로 이렇게 되어 있으며, 우리는 몇 주 후에 그것에 대해 배울 것입니다; 지금은 그냥 받아들여 주세요!

Note that **function application has higher precedence than any infix operators**. So it would be incorrect to write  
**함수 적용이 모든 중위 연산자보다 우선순위가 높다**는 것을 주목하세요. 따라서 다음과 같이 쓰는 것은 잘못된 것입니다:
`f 3 n+1 7`

if you intend to pass `n+1` as the second argument to `f`, because this parses as  
`f`에 `n+1`을 두 번째 인수로 전달하려는 의도라면, 이는 다음과 같이 해석됩니다:   
`(f 3 n) + (1 7)`.

Instead, one must write  
대신에, 다음과 같이 써야 합니다:  
`f 3 (n+1) 7`.


## Lists

_Lists_ are one of the most basic data types in Haskell.  
_리스트_ 는 하스켈에서 가장 기본적인 데이터 타입 중 하나입니다.

```haskell
nums, range, range2 :: [Integer]
nums   = [1, 2, 3, 19]
range  = [1..100]
range2 = [2, 4..100]
```

Haskell (like Python) also has _list comprehensions_; you can read about them in [LYAH](http://learnyouahaskell.com/starting-out).  
하스켈은 (파이썬처럼) _list comprehensions_ 도 가지고 있습니다; 이에 대해서는 [LYAH](http://learnyouahaskell.com/starting-out)에서 읽을 수 있습니다.

Strings are just lists of characters. That is, `String` is just an abbreviation for `[Char]`, and string literal syntax (text surrounded by double quotes) is just an abbreviation for a list of `Char` literals.  
문자열은 단순히 문자들의 리스트입니다. 즉, `String`은 `[Char]`의 약어일 뿐이며, 문자열 리터럴 문법(쌍따옴표로 둘러싸인 텍스트)은 단순히 `Char` 리터럴들의 리스트의 약어일 뿐입니다.


```haskell
-- hello1 and hello2 are exactly the same.

hello1 :: [Char]
hello1 = ['h', 'e', 'l', 'l', 'o']

hello2 :: String
hello2 = "hello"

helloSame = hello1 == hello2
```

This means that all the standard library functions for processing lists can also be used to process `String`.  
이는 모든 표준 라이브러리의 리스트 처리 함수들이 `String`을 처리하는 데에도 사용할 수 있음을 의미합니다.

## Constructing lists

The simplest possible list is the empty list:  
가장 단순한 리스트는 빈 리스트입니다:

```haskell
emptyList = []
```

Other lists are built up from the empty list using the _cons_ operator, `(:)`. Cons takes an element and a list, and produces a new list with the element prepended to the front.  
다른 리스트들은 _cons_ 연산자인 `(:)`을 사용하여 빈 리스트에서부터 구축됩니다. Cons는 요소와 리스트를 받아, 그 요소를 앞에 추가한 새로운 리스트를 생성합니다.

```haskell
ex17 = 1 : []
ex18 = 3 : (1 : [])
ex19 = 2 : 3 : 4 : []

ex20 = [2, 3, 4] == 2 : 3 : 4 : []
```

We can see that `[2, 3, 4]` notation is just convenient shorthand for `2 : 3 : 4 : []`. Note also that these are really _singly linked lists_, NOT arrays.  
우리는 `[2, 3, 4]` 표기법이 단지 `2 : 3 : 4 : []`의 편리한 약어임을 알 수 있습니다. 또한 이것들이 실제로는 _단일 연결 리스트_ 임을 주목하세요, 배열이 아닙니다.

```haskell
-- Generate the sequence of hailstone iterations from a starting number.
hailstoneSeq :: Integer -> [Integer]
hailstoneSeq 1 = [1]
hailstoneSeq n = n : hailstoneSeq (hailstone n)
```

We stop the hailstone sequence when we reach 1. The hailstone sequence for a general `n` consists of `n` itself, followed by the hailstone sequence for `hailstone n`, that is, the number obtained by applying the hailstone transformation once to `n`.  
우리는 1에 도달했을 때 하일스톤 시퀀스를 멈춥니다. 일반적인 `n`에 대한 하일스톤 시퀀스는 `n` 자체와 그 뒤에 `hailstone n`에 대한 하일스톤 시퀀스로 구성됩니다, 즉 `n`에 하일스톤 변환을 한 번 적용하여 얻은 숫자입니다.

## Functions on lists

We can write functions on lists using _pattern matching_.  
우리는 _패턴 매칭_ 을 사용하여 리스트에 대한 함수를 작성할 수 있습니다.

```haskell
-- Compute the length of a list of Integers.
intListLength :: [Integer] -> Integer
intListLength []     = 0
intListLength (x:xs) = 1 + intListLength xs
```

The first clause says that the length of an empty list is 0. The second clause says that if the input list looks like `(x:xs)`, that is, a first element `x` consed onto a remaining list `xs`, then the length is one more than the length of `xs`.  
첫 번째 절은 빈 리스트의 길이가 0임을 말합니다. 두 번째 절은 입력 리스트가 `(x:xs)`처럼 보인다면, 즉 첫 번째 요소 `x`가 나머지 리스트 `xs`에 연결돼있다면, 길이는 `xs`의 길이보다 하나 더 많다는 것을 의미합니다.

Since we don't use `x` at all we could also replace it by an underscore:   
우리는 `x`를 전혀 사용하지 않기 때문에, 이를 밑줄로 대체할 수도 있습니다: 
`intListLength (_:xs) = 1 + intListLength xs`

We can also use nested patterns:  
또한 중첩된 패턴을 사용할 수 있습니다:

```haskell
sumEveryTwo :: [Integer] -> [Integer]
sumEveryTwo []         = []     -- Do nothing to the empty list
sumEveryTwo (x:[])     = [x]    -- Do nothing to lists with a single element
sumEveryTwo (x:(y:zs)) = (x + y) : sumEveryTwo zs
```

Note how the last clause matches a list starting with `x` and followed by... a list starting with `y` and followed by the list `zs`. We don't actually need the extra parentheses, so `sumEveryTwo (x:y:zs) = ...` would be equivalent.  
마지막 절이 `x`로 시작하고 그 뒤에 `y`로 시작하며 그 뒤에 리스트 `zs`가 오는 리스트와 일치하는 방식을 주목하세요. 실제로 추가적인 괄호는 필요 없으므로 `sumEveryTwo (x:y:zs) = ...`는 동등합니다.

## Combining functions

It's good Haskell style to build up more complex functions by combining many simple ones.  
하스켈의 좋은 스타일은 많은 간단한 함수들을 결합하여 더 복잡한 함수를 구축하는 것입니다.

```haskell
-- The number of hailstone steps needed to reach 1 from a starting
-- number.
hailstoneLen :: Integer -> Integer
hailstoneLen n = intListLength (hailstoneSeq n) - 1
```

This may seem inefficient to you: it generates the entire hailstone sequence first and then finds its length, which wastes lots of memory... doesn't it? Actually, it doesn't! Because of Haskell's lazy evaluation, each element of the sequence is only generated as needed, so the sequence generation and list length calculation are interleaved. The whole computation uses only O(1) memory, no matter how long the sequence. (Actually, this is a tiny white lie, but explaining why (and how to fix it) will have to wait a few weeks.)  
이것은 비효율적으로 보일 수 있습니다: 전체 하일스톤 시퀀스를 먼저 생성한 다음 그 길이를 찾기 때문에 많은 메모리를 낭비합니다... 그렇지 않나요? 실제로는 그렇지 않습니다! 하스켈의 지연 평가 덕분에 시퀀스의 각 요소는 필요할 때만 생성되므로, 시퀀스 생성과 리스트 길이 계산이 교차하여 이루어집니다. 전체 계산은 시퀀스의 길이에 관계없이 O(1) 메모리만 사용합니다. (실제로, 이것은 작은 거짓말이지만, 그 이유와 이를 고치는 방법을 설명하려면 몇 주를 더 기다려야 합니다.)

We'll learn more about Haskell's lazy evaluation strategy in a few weeks. For now, the take-home message is: don't be afraid to write small functions that transform whole data structures, and combine them to produce more complex functions. It may feel unnatural at first, but it's the way to write idiomatic (and efficient) Haskell, and is actually a rather pleasant way to write programs once you get used to it.  
몇 주 후에 하스켈의 지연 평가 전략에 대해 더 많이 배울 것입니다. 지금의 핵심 메시지는: 전체 데이터 구조를 변환하는 작은 함수를 작성하는 것을 두려워하지 말고, 그것들을 결합하여 더 복잡한 함수를 만들어내세요. 이는 처음에 부자연스럽게 느껴질 수 있지만, 이는 관용적(그리고 효율적인) 하스켈을 작성하는 방법이며, 익숙해지면 실제로 꽤 즐거운 프로그램 작성 방식입니다.

## A word about error messages

Actually, six:  
여섯 번째로:  
**Don't be scared of error messages!**  
에러 메시지를 두려워하지 마세요!

GHC's error messages can be rather long and (seemingly) scary. However, usually they're long not because they are obscure, but because they contain a lot of useful information! Here's an example:  
GHC의 오류 메시지는 꽤 길고 (겉보기에는) 무서울 수 있습니다. 그러나 보통 길어지는 이유는 그것들이 불명확해서가 아니라, 많은 유용한 정보를 포함하고 있기 때문입니다! 예를 들어:

```haskell
Prelude> 'x' ++ "foo"

<interactive>:1:1:
    Couldn't match expected type `[a0]' with actual type `Char'
    In the first argument of `(++)', namely 'x'
    In the expression: 'x' ++ "foo"
    In an equation for `it': it = 'x' ++ "foo"
```

First we are told "Couldn't match expected type `[a0]` with actual type `Char`". This means that _something_ was expected to have a list type, but actually had type `Char`. What something? The next line tells us: it's the first argument of `(++)`, which is at fault, namely, `'x'`. The next lines go on to give us a bit more context. Now we can see what the problem is: clearly `'x'` has type `Char`, as the first line said. Why would it be expected to have a list type? Well, because it is used as an argument to `(++)`, which takes a list as its first argument.  
먼저 "Couldn't match expected type `[a0]` with actual type `Char`"라고 알려줍니다. 이는 _어떤 것_ 이 리스트 타입을 가질 것으로 예상되었지만 실제로는 `Char` 타입을 가졌다는 것을 의미합니다. 그 어떤 것이 무엇일까요? 다음 줄에서 알려줍니다: `(++)`의 첫 번째 인수, 즉 `'x'`가 문제입니다. 그리고 다음 줄에서 힌트를 조금 더 제공합니다. 이제 문제의 원인이 무엇인지 알 수 있습니다: 분명히 `'x'`는 `Char` 타입을 가지고 있으며, 첫 번째 줄에서 그렇게 말했습니다. 왜 리스트 타입을 가질 것으로 예상되었을까요? 이는 `(++)`의 인수로 사용되었기 때문입니다, `(++)`는 첫 번째 인수로 리스트를 받습니다.

When you get a huge error message, resist your initial impulse to run away; take a deep breath; and read it carefully. You won't necessarily understand the entire thing, but you will probably learn a lot, and you may just get enough information to figure out what the problem is.  
거대한 오류 메시지를 받았을 때, 도망가려는 충동을 억제하세요; 깊게 숨을 쉬고; 신중하게 읽어보세요. 전체 내용을 반드시 이해하지 못할 수도 있지만, 아마도 많은 것을 배울 것이고, 문제의 원인을 찾아낼 만큼 충분한 정보를 얻을 수도 있습니다.

---
