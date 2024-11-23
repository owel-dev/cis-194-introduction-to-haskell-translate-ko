# CIS 194: Homework 3

## Code golf!

<img src="../images/golf.png" width="300px" />

This assignment is simple: there are three tasks listed below. For each task, you should submit a Haskell function with the required name and type signature which accomplishes the given task and is as short *as possible.*  
이 과제는 간단합니다: 아래에 세 가지 과제가 나열되어 있습니다. 각 과제에 대해 요구된 이름과 타입 시그니처를 가진 Haskell 함수를 제출해야 하며, 해당 작업을 가능한 짧게 수행해야 합니다.

## Rules

- Along with your solution for each task you must include a comment explaining your **solution and how it works. Solutions without an explanatory comment will get a score of zero.** Your comment should demonstrate a complete understanding of your solution. In other words, anything is fair game but you must demonstrate that your understand how it works. If in doubt, include more detail.  
각 과제에 대한 해결책과 함께 **해결 방법과 작동 방식을 설명하는 주석**을 포함해야 합니다. 설명 주석이 없는 해결책은 점수가 0점이 됩니다. 주석은 해결 방법에 대한 완전한 이해를 보여주어야 합니다. 즉, 무엇이든 사용 가능하지만 작동 방식을 이해하고 있음을 보여야 합니다. 의심스러울 경우 더 많은 세부 정보를 포함하세요.

- Comments do not count towards the length of your solution.  
주석은 해결책의 길이에 포함되지 않습니다.

- Type signatures do not count towards the length of your solutions.  
타입 시그니처는 해결책의 길이에 포함되지 않습니다.

- import statements do not count towards the length of your solutions. You may import any modules included in the Haskell Platform.  
import 문은 해결책의 길이에 포함되지 않습니다. Haskell 플랫폼에 포함된 모든 모듈을 import할 수 있습니다.

- **Whitespace does not count towards the length** of your solutions. So there is no need to shove all your code onto one line and take all the spaces out. Use space as appropriate, indent nicely, etc., but otherwise try making your code as short as you can.  
**공백은 해결책의 길이에 포함되지 않습니다.** 따라서 모든 코드를 한 줄로 몰아서 공백을 제거할 필요가 없습니다. 적절하게 공백을 사용하고, 들여쓰기를 깔끔하게 하는 등, 그 외에는 가능한 코드를 짧게 작성하세요.

- You are welcome to include additional functions beyond the ones required. That is, you are welcome to break up your solutions into several functions if you wish (indeed, sometimes this may lead to a very short solution). Of course, such additional functions will be counted towards the length of your solution (excluding their type signatures).  
요구된 함수 외에 추가 함수를 포함해도 됩니다. 즉, 원한다면 해결책을 여러 함수로 나눌 수 있습니다 (실제로 때로는 매우 짧은 해결책을 만들 수 있습니다). 물론, 이러한 추가 함수는 해결책의 길이에 포함됩니다 (타입 시그니처 제외).

- Your final submission should be named Golf.hs. Your file should define a module named Golf, that is, at the top of your file you should have   
최종 제출 파일 이름은 Golf.hs이어야 합니다. 파일은 Golf라는 모듈을 정의해야 하며, 파일 상단에 다음과 같이 작성해야 합니다.

    *module Golf where*

- The three shortest solutions (counting the total number of characters, excluding whitespace and the other exceptions listed above) for each task will receive two points of extra credit each. You can get up to a total of four extra credit points.  
각 과제에 대해 (공백과 위에 나열된 예외를 제외한) 총 문자 수로 계산한 세 가지 가장 짧은 해결책은 각 과제당 2점의 추가 점수를 받습니다. 총 4점의 추가 점수를 받을 수 있습니다.

- Otherwise, the length does not really matter; long but correct solutions will receive full credit for correctness (although they may or may not get full credit for style, depending on their style).  
그 외에는, 길이는 별로 중요하지 않습니다; 길지만 올바른 해결책은 정확성에 대해 전부 만점을 받게 됩니다 (물론, 스타일에 따라 스타일 점수를 만점을 받을 수도 있고 아닐 수도 있습니다).

## Hints

- Use functions from the standard libraries as much as possible— that’s part of the point of this assignment. Using (say) map is much shorter than implementing it yourself!  
가능한 한 표준 라이브러리의 함수를 사용하세요—이것이 이 과제의 일부입니다. 예를 들어, map을 사용하는 것이 직접 구현하는 것보다 훨씬 짧습니다!

- In particular, try to use functions from the standard libraries that encapsulate recursion patterns, rather than writing explicitly recursive functions yourself.  
특히, 명시적으로 재귀 함수를 작성하는 대신 재귀 패턴을 캡슐화하는 표준 라이브러리의 함수를 사용하려고 노력하세요.

- You may want to start by getting something that works, without worrying about the length. Once you have solved the task, try to figure out ways to make your solution shorter.  
길이에 신경 쓰지 않고 작동하는 것을 먼저 구현하는 것부터 시작할 수 있습니다. 과제를 해결한 후, 해결책을 더 짧게 만들 수 있는 방법을 찾아보세요.

- If the specification of a task is unclear, feel free to ask for a clarification on Piazza.  
과제의 명세가 불명확하면 Piazza에서 명확한 설명을 요청하세요.

- We will test your functions on other inputs besides the ones given as examples, so to be safe, so should you!  
우리는 예시로 주어진 것 외의 다른 입력에도 여러분의 함수를 테스트할 것입니다, 따라서 안전하게 여러분도 테스트해야 합니다!

## Tasks

### Exercise 1 Hopscotch

Your first task is to write a function  
첫 번째 과제는 다음과 같은 함수를 작성하는 것입니다.

`skips :: [a] -> [[a]]`

The output of skips is a list of lists. The first list in the output should be the same as the input list. The second list in the output should contain every second element from the input list. . . and the nth list in the output should contain every nth element from the input list.  
skips의 출력은 리스트들의 리스트입니다. 출력의 첫 번째 리스트는 입력 리스트와 동일해야 합니다. 출력의 두 번째 리스트는 입력 리스트에서 두 번째 요소마다 포함해야 합니다. ... 그리고 출력의 n번째 리스트는 입력 리스트에서 n번째 요소마다 포함해야 합니다.

For example:

```haskell
skips "ABCD" == ["ABCD", "BD", "C", "D"]
skips "hello!" == ["hello!", "el!", "l!", "l", "o", "!"]
skips [1] == [[1]]
skips [True,False] == [[True,False], [False]]
skips [] == []
```

Note that the output should be the same length as the input.  
출력은 입력과 동일한 길이를 가져야 한다는 점에 유의하세요.

### Exercise 2 Local maxima

A _local maximum_ of a list is an element of the list which is strictly greater than both the elements immediately before and after it. For example, in the list [2,3,4,1,5], the only local maximum is 4, since it is greater than the elements immediately before and after it (3 and 1). 5 is not a local maximum since there is no element that comes after it.  
리스트의 _국소 최대값_은 리스트에서 바로 앞과 뒤의 요소보다 엄격히 큰 요소입니다. 예를 들어, 리스트 [2,3,4,1,5]에서 유일한 국소 최대값은 4입니다. 왜냐하면 4는 바로 앞과 뒤의 요소(3과 1)보다 크기 때문입니다. 5는 그 뒤에 요소가 없기 때문에 국소 최대값이 아닙니다.

Write a function

`localMaxima :: [Integer] -> [Integer]`  

which finds all the local maxima in the input list and returns them in order.  
입력 리스트에서 모든 국소 최대값을 찾아 순서대로 반환하는 함수입니다.

For example:

```haskell
localMaxima [2,9,5,6,1] == [9,6]
localMaxima [2,3,4,1,5] == [4]
localMaxima [1,2,3,4,5] == []
```

### Exercise 3 Histogram

For this task, write a function  
이 과제를 위해, 다음과 같은 함수를 작성하세요.

`histogram :: [Integer] -> String`

which takes as input a list of Integers between 0 and 9 (inclusive), and outputs a vertical histogram showing how many of each number were in the input list. You may assume that the input list does not contain any numbers less than zero or greater than 9 (that is, it does not matter what your function does if the input does contain such numbers). Your output must exactly match the output shown in the examples below.  
이 함수는 0에서 9 사이의 정수(포함) 리스트를 입력으로 받아, 입력 리스트에 각 숫자가 얼마나 있는지를 나타내는 수직 히스토그램을 출력합니다. 입력 리스트에 0보다 작거나 9보다 큰 숫자가 포함되지 않는다고 가정할 수 있습니다 (즉, 만약 입력에 그런 숫자가 포함되더라도 함수가 어떻게 동작하는지는 중요하지 않습니다). 출력은 아래 예시와 정확히 일치해야 합니다.

```
histogram [1,1,1,5] ==
*
*
* *
==========
0123456789


histogram [1,4,5,4,6,6,3,4,2,4,9] ==
*
*
* *
****** *
==========
0123456789
```

**Important note:** If you type something like histogram [3,5] at the ghci prompt, you should see something like this:  
**중요한 참고 사항:** ghci 프롬프트에서 histogram [3,5]와 같은 것을 입력하면, 다음과 같은 것을 보게 될 것입니다:

```
" * * \n==========\n0123456789\n"
```

This is a textual _representation_ of the String output, including \n escape sequences to indicate newline characters. To actually visualize the histogram as in the examples above, use putStr, for example,  
이것은 개행 문자를 나타내는 \n 이스케이프 시퀀스를 포함한 String 출력의 텍스트 _표현_입니다. 위 예시처럼 히스토그램을 실제로 시각화하려면, 예를 들어 putStr을 사용하세요.

`putStr (histogram [3,5]).`