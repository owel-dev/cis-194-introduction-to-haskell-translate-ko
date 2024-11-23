# CIS 194: Homework 2

> Due Monday January 28

Something has gone terribly wrong!  
뭔가 정말 끔찍하게 잘못되었습니다!

- Files you will need: Log.hs, error.log, sample.log  
**필요한 파일:** Log.hs, error.log, sample.log
- Files you should submit: LogAnalysis.hs  
**제출해야 할 파일:** LogAnalysis.hs

<br>

<img src="../images/rabbit.png" width="200px" />

## Log file parsing

We’re really not sure what happened, but we did manage to recover the log file error.log. It seems to consist of a different log message on each line. Each line begins with a character indicating the type of log message it represents:  
무슨 일이 있었는지 정확히 모르겠지만, error.log 로그 파일을 복구하는 데 성공했습니다. 이 파일은 각 줄마다 다른 로그 메시지로 구성된 것 같습니다. 각 줄은 해당 로그 메시지의 유형을 나타내는 문자로 시작합니다:

- ’I’ for informational messages,  
’I’는 정보 메시지,
- ’W’ for warnings, and  
’W’는 경고
- ’E’ for errors.  
’E’는 오류를 의미합니다.

The error message lines then have an integer indicating the severity of the error, with 1 being the sort of error you might get around to caring about sometime next summer, and 100 being epic, catastrophic failure. All the types of log messages then have an integer time stamp followed by textual content that runs to the end of the line. Here is a snippet of the log file including an informational message followed by a level 2 error message:  
오류 메시지 줄에는 오류의 심각도를 나타내는 정수가 있으며, 1은 내년에 봐도 될 정도의 사소한 오류이고, 100은 치명적인 실패를 의미합니다. 모든 유형의 로그 메시지는 정수 형태의 타임스탬프가 뒤따르고, 줄 끝까지 이어지는 텍스트 내용이 있습니다. 다음은 정보 메시지와 레벨 2 오류 메시지가 포함된 로그 파일의 일부입니다:

```
I 147 mice in the air, I’m afraid, but you might catch a bat, and  
E 2 148 #56k istereadeat lo d200ff] BOOTMEM
```

It’s all quite confusing; clearly we need a program to sort through this mess. We’ve come up with some data types to capture the structure of this log file format:  
모두 매우 혼란스럽습니다. 분명히 이 혼란을 정리할 프로그램이 필요합니다. 우리는 이 로그 파일 형식의 구조를 포착하기 위해 몇 가지 데이터 타입을 고안했습니다:

```haskell
data MessageType = Info
                | Warning
                | Error Int
                deriving (Show, Eq)

type TimeStamp = Int

data LogMessage = LogMessage MessageType TimeStamp String
                | Unknown String
                deriving (Show, Eq)
```

Note that LogMessage has two constructors: one to represent normally formatted log messages, and one to represent anything else that does not fit the proper format.  
LogMessage에는 두 개의 생성자가 있습니다: 하나는 정상적으로 형식화된 로그 메시지를 나타내고, 다른 하나는 적절한 형식에 맞지 않는 모든 것을 나타냅니다.

We’ve provided you with a module Log.hs containing these data type declarations, along with some other useful functions. Download Log.hs and put it in the same folder where you intend to put your homework assignment. Please name your homework assignment LogAnalysis.hs (or .lhs if you want to make it a literate Haskell document). The first few lines of LogAnalysis.hs should look like this:  
우리는 이러한 데이터 타입 선언과 몇 가지 유용한 함수가 포함된 Log.hs 모듈을 제공했습니다. Log.hs를 다운로드하여 과제를 넣을 디렉토리에 넣으세요. 과제 파일의 이름을 LogAnalysis.hs로 지정하세요. (_Literate Haskell Document_ 를 만들고 싶다면 .lhs 사용). LogAnalysis.hs의 처음 몇 줄은 다음과 같아야 합니다:

```haskell
{-# OPTIONS_GHC -Wall #-}
module LogAnalysis where

import Log
```

which sets up your file as a module named LogAnalysis, and imports the module from Log.hs so you can use the types and functions it provides.  
이는 파일을 LogAnalysis라는 모듈로 설정하고, Log.hs 모듈을 가져와 제공하는 타입과 함수를 사용할 수 있게 합니다.

### Exercise 1

The first step is figuring out how to parse an individual message. Define a function  
첫 번째 단계는 개별 메시지를 파싱하는 방법을 알아내는 것입니다. 다음 함수를 정의하세요:

```haskell
parseMessage :: String -> LogMessage
```

which parses an individual line from the log file. For example,  
이는 로그 파일의 각 줄을 파싱합니다. 예를 들어,

```haskell
parseMessage "E 2 562 help help"
    == LogMessage (Error 2) 562 "help help"

parseMessage "I 29 la la la"
    == LogMessage Info 29 "la la la"

parseMessage "This is not in the right format"
    == Unknown "This is not in the right format"
```

Once we can parse one log message, we can parse a whole log file. Define a function  
한 개의 로그 메시지를 파싱할 수 있게 되면, 전체 로그 파일을 파싱할 수 있습니다. 다음 함수를 정의하세요:

```haskell
parse :: String -> [LogMessage]
```

which parses an entire log file at once and returns its contents as a list of LogMessages.
이는 전체 로그 파일을 한 번에 파싱하여 LogMessages의 리스트를 반환합니다.

To test your function, use the testParse function provided in the Log module, giving it as arguments your parse function, the number of lines to parse, and the log file to parse from (which should also be in the same folder as your assignment). For example, after loading your assignment into GHCi, type something like this at the prompt:  
함수를 테스트하려면 Log 모듈에서 제공하는 testParse 함수를 사용하세요. 이 함수에 parse 함수, 파싱할 줄 수, 파싱할 로그 파일 이름을 인수로 전달합니다 (로그 파일도 과제 파일과 같은 디렉토리에 있어야 합니다). 예를 들어, GHCi에 과제를 로드한 후 프롬프트에 다음과 같이 입력하세요:

`testParse parse 10 "error.log"`

Don’t reinvent the wheel! (That’s so last week.) Use Prelude functions to make your solution as concise, high-level, and functional as possible. For example, to convert a `String` like "562" into an Int, you can use the read function. Other functions which may (or may not) be useful to you include `lines`, `words`, `unwords`, `take`, `drop`, and (.).  
휠을 다시 만들지 마세요! (그건 지난주 일이에요.) Prelude 함수를 사용하여 가능한 한 간결하고, 고수준이며, 함수형으로 솔루션을 만드세요. 예를 들어, "562"와 같은 `String`을 Int로 변환하려면 read 함수를 사용할 수 있습니다. `lines`, `words`, `unwords`, `take`, `drop`, 그리고 (.)과 같은 다른 함수들도 유용할 수 있습니다.

## Putting the logs in order

Unfortunately, due to the error messages being generated by multiple servers in multiple locations around the globe, a lightning storm, a failed disk, and a bored yet incompetent programmer, the log messages are horribly out of order. Until we do some organizing, there will be no way to make sense of what went wrong! We’ve designed a data structure that should help—a binary search tree of `LogMessages`:  
불행히도, 여러 위치의 여러 서버에서 생성된 오류 메시지, 천재지변, 저장장치 문제, 그리고 지루하고 무능한 프로그래머로 인해 로그 메시지의 순서가 끔찍하게 어긋났습니다. 정리를 하지 않으면 무슨 일이 잘못되었는지 이해할 방법이 없습니다! 우리는 이를 도울 수 있는 데이터 구조를 설계했습니다—`LogMessages`의 이진 탐색 트리:

```haskell
data MessageTree = Leaf
                | Node MessageTree LogMessage MessageTree
```

Note that MessageTree is a recursive data type: the Node constructor itself takes two children as arguments, representing the left and right subtrees, as well as a LogMessage. Here, Leaf represents the empty tree.  
MessageTree가 재귀 데이터 타입임을 주목하세요: Node 생성자는 왼쪽과 오른쪽 서브트리를 나타내는 두 자식과 하나의 LogMessage를 인수로 받습니다. 여기서 Leaf는 빈 트리를 나타냅니다.

A MessageTree should be sorted by timestamp: that is, the timestamp of a LogMessage in any Node should be greater than all timestamps of any LogMessage in the left subtree, and less than all timestamps of any LogMessage in the right child. Unknown messages should not be stored in a MessageTree since they lack a timestamp.  
MessageTree는 타임스탬프로 정렬되어야 합니다: 즉, 어떤 Node의 LogMessage 타임스탬프는 왼쪽 서브트리의 모든 LogMessage 타임스탬프보다 커야 하고, 오른쪽 자식의 모든 LogMessage 타임스탬프보다 작아야 합니다. Unknown 메시지는 타임스탬프가 없기 때문에 MessageTree에 저장해서는 안 됩니다.

### Exercise 2 

Define a function  
다음과 같은 함수를 정의하세요:

`insert :: LogMessage -> MessageTree -> MessageTree`

which inserts a new LogMessage into an existing MessageTree, producing a new MessageTree. insert may assume that it is given a sorted MessageTree, and must produce a new sorted MessageTree containing the new LogMessage in addition to the contents of the original MessageTree.  
이는 새로운 LogMessage를 기존의 MessageTree에 삽입하여 새로운 MessageTree를 생성합니다. insert는 정렬된 MessageTree가 주어진다고 가정할 수 있으며, 원래 MessageTree의 내용에 새로운 LogMessage를 추가하여 새로운 정렬된 MessageTree를 생성해야 합니다.

However, note that if insert is given a LogMessage which is Unknown, it should return the MessageTree unchanged.  
단, insert에 Unknown LogMessage가 주어지면 MessageTree를 변경하지 않고 그대로 반환해야 합니다.

### Exercise 3 

Once we can insert a single LogMessage into a MessageTree, we can build a complete MessageTree from a list of messages. Specifically, define a function  
한 개의 LogMessage를 MessageTree에 삽입할 수 있게 되면, 메시지 리스트로부터 완전한 MessageTree를 만들 수 있습니다. 구체적으로, 다음 함수를 정의하세요:

`build :: [LogMessage] -> MessageTree`

which builds up a MessageTree containing the messages in the list, by successively inserting the messages into a MessageTree (beginning with a Leaf).  
이는 리스트에 있는 메시지들을 차례대로 MessageTree에 삽입하여 메시지를 포함하는 MessageTree를 구축합니다 (Leaf에서 시작).

### Exercise 4 

Finally, define the function  
마지막으로, 다음 함수를 정의하세요:

`inOrder :: MessageTree -> [LogMessage]`

which takes a sorted MessageTree and produces a list of all the LogMessages it contains, sorted by timestamp from smallest to biggest.  
(This is known as an in-order traversal of the MessageTree.)  
With these functions, we can now remove Unknown messages and sort the well-formed messages using an expression such as:   
이는 정렬된 MessageTree를 받아 그 안에 포함된 모든 LogMessages의 리스트를 타임스탬프 순으로 (작은 것부터 큰 것 순으로) 생성합니다.  
(이것은 MessageTree의 중위 순회로 알려져 있습니다.)  
이 함수들을 사용하면 Unknown 메시지를 제거하고, 잘 형식화된 메시지들을 다음과 같은 표현식을 사용하여 정렬할 수 있습니다:

`inOrder (build tree)`

[Note: there are much better ways to sort a list; this is just an exercise to get you working with recursive data structures!]  
[참고: 리스트를 정렬하는 더 좋은 방법들이 많이 있습니다; 이것은 단지 재귀 데이터 구조를 다루는 연습일 뿐입니다!]

## Log file postmortem

### Exercise 5 

Now that we can sort the log messages, the only thing left to do is extract the relevant information. We have decided that “relevant” means “errors with a severity of at least 50”.  
이제 로그 메시지를 정렬할 수 있게 되었으니, 남은 일은 관련 정보를 추출하는 것입니다. 우리는 "관련"을 "심각도가 최소 50인 오류"로 정의하기로 했습니다.

Now write a function  
다음 함수를 작성하세요:

`whatWentWrong :: [LogMessage] -> [String]`

which takes an _unsorted_ list of LogMessages, and returns a list of the messages corresponding to any errors with a severity of 50 or greater, sorted by timestamp. (Of course, you can use your functions from the previous exercises to do the sorting.)  
이는 _정렬되지 않은_ LogMessages 리스트를 받아 심각도가 50 이상인 모든 오류에 해당하는 메시지들의 리스트를 타임스탬프 순으로 반환합니다. (물론, 정렬을 위해 이전 연습문제의 함수들을 사용할 수 있습니다.)

For example, suppose our log file looked like this:  
예를 들어, 로그 파일이 다음과 같다고 가정해 봅시다:

```
I 6 Completed armadillo processing
I 1 Nothing to report
E 99 10 Flange failed!
I 4 Everything normal
I 11 Initiating self-destruct sequence
E 70 3 Way too many pickles
E 65 8 Bad pickle-flange interaction detected
W 5 Flange is due for a check-up
I 7 Out for lunch, back in two time steps
E 20 2 Too many pickles
I 9 Back from lunch
```

This file is provided as `sample.log`. There are four errors, three of which have a severity of greater than 50. The output of `whatWentWrong` on `sample.log` ought to be  
이 파일은 `sample.log`로 제공됩니다. 오류는 네 개이며, 그 중 세 개는 심각도가 50 이상입니다. `sample.log`에서 `whatWentWrong`의 출력은 다음과 같아야 합니다:

```
[ "Way too many pickles"
, "Bad pickle-flange interaction detected"
, "Flange failed!"
]
```

You can test your whatWentWrong function with testWhatWentWrong, which is also provided by the Log module. You should provide testWhatWentWrong with your parse function, your whatWentWrong function, and the name of the log file to parse.  
Log 모듈에서 제공하는 testWhatWentWrong으로 whatWentWrong 함수를 테스트할 수 있습니다. testWhatWentWrong 에는 parse 함수, whatWentWrong 함수, 그리고 파싱할 로그 파일의 이름을 인수로 제공해야 합니다.

## Miscellaneous

- We will test your solution on log files other than the ones we have given you, so no hardcoding!  
우리는 제공한 것 외의 로그 파일로도 당신의 솔루션을 테스트할 것이므로, 하드코딩하지 마세요!
- You are free (in fact, encouraged) to discuss the assignment with any of your classmates as long as you type up your own solution.  
자신의 솔루션을 작성하는 한, 어떤 동료와도 과제를 논의하는 것이 자유롭습니다 (사실 권장됩니다).

## Epilogue

### Exercise 6 (Optional)

For various reasons we are beginning to suspect that the recent mess was caused by a single, egotistical hacker. Can you figure out who did it?  
여러 가지 이유로 최근의 혼란이 한 명의 자기중심적인 해커에 의해 야기된것이라 의심하기 시작했습니다. 누가 그랬는지 알아낼 수 있나요?