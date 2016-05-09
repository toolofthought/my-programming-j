# Programming J #

# Table of Contents #
- [grasp on J programming language](#grasp_on_J_programming_language)
## 책의 구성 ##

- primitive에 대한 충실한 설명
- plot 패키지 사용을 강조했습니다. 가능하면 plot을 이용해 설명하려고 노력했습니다.
- 필요한 경우 APL기호를 설명합니다. APL을 배우는 것이 목적은 아니지만 연상작용을 의도했습니다.
- 인터넷에서 찾을 수 있는 자료라 하더라도 종이에 인쇄한 결과물의 가독성이 비교할 수 없을 정도로 높기때문에 출판을 결정합니다.

#### notation for numerals ####
e r j p: 1e3, 2r3, 3j4

p, of the form XpY means X * pi ^ Y 

pi =: 1p1 	twopi =: 2p1 	2p_1
3.14159 	6.28319 	0.63662

Similarly, a numeral written with letter x, of the form XxY means X * e ^ Y where e is the familiar value 2.718281828....

    	e =: 1x1 	2x_1 	2 * e ^ _1
    2.71828 	0.735759 	0.735759

These p and x forms of numeral provide a convenient way of writing constants accurately without writing out many digits. Finally, we can write numerals with a base other than 10. For example the binary or base-2 number with binary digits 101 has the value 5 and can be written as 2b101.

   2b101 
5

The general scheme is that NbDDD.DDD is a numeral in number-base N with digits DDD.DDD . With bases larger than 10, we will need digits larger than 9, so we take letter 'a' as a digit with value 10, 'b' with value 11, and so on up to 'z' with value 35.

For example, letter 'f' has digit-value 15, so in hexadecimal (base 16) the numeral written 16bff has the value 255. The number-base N is given in decimal.

16bff 	(16 * 15) + 15
255 	255

One more example. 10b0.9 is evidently a base-10 number meaning "nine-tenths" and so, in base 20, 20b0.f means "fifteen twentieths"

   10b0.9 20b0.f
0.9 0.75

<a name="grasp_on_J_programming_language"></a>
## grasp on J programming language ##
### what is rank? ###
J 언어를 처음 접하면 당황스러운 점이 있습니다. 어디 하나 밖일까요? 짝이 맞지 않은 [ 와 ], 생선을 닮은 기호 등 - 예를 들어 <@[ - 이 당혹스럽기 그지 없습니다. 여기에 한가지를 추가하면 반복문에 대한 언급이 없다는 점입니다. 반복문이 없는 프로그래밍 언어가 가당키나 한 것인가요?

그런데 그것이 정말 일어났습니다. J언어는 반복문이 없어도 코드가 실행됩니다. 도리어 반복문없는 코드를 권장하기까지 합니다. 이게 가능하냐고 물으신다면 이 이야기를 들려드리고 싶습니다. 1960년대 프로그래머들은 goto문이 없는 프로그래밍에 대한 이야기에 코웃음을 쳤습니다. [source](http://www.jsoftware.com/help/jforc/loopless_code_i_verbs_have_r.htm#_Toc191734331)

이 모든 것은 rank라는 개념에 의해 가능합니다. 물론 반복문이 rank를 배우는 절대적인 목적은 아닙니다. 그렇지만 반복문이 어떻게 실행되는냐를 톺아보는 것이 rank개념을 빠르게 이해하는 데 좋은 방법이기 때문에 이제 이런 맥락에서 이야기를 이어가겠습니다.

rank라는 용어는 세가지 다른 개념을 지칭하며 혼재되어 사용됩니다. 세가지 개념은 서로 긴밀하게 연과되어 있지만 대부분의 경우 따로 설명하지 않아도 문맥상 구분이 가능한 독립된 개념입니다.

* noun rank
* functional(verb) rank
* rank conjunction

#### noun rank ####
우선 noun rank부터 시작하겠습니다. 

J언어에서 noun은 atom이거나 array입니다. 다르게 표현하면 모든 noun은 낱개 하나(atom)이거나, 여러 개의 모음(array)입니다. 당연한 이야기입니다만 조금만 더 알아보겠습니다.

[source](http://code.jsoftware.com/wiki/Vocabulary/Nouns#Shape) 

가장 작은 데이터 단위를 atom이라고 칭하고 그리스 어원(a + temnein ⇔ not to cut ⇔ 나누어지지 않는)이 의미하듯 더이상 나누어지지 않는 데이터 단위를 말합니다. numeric atom은 숫자 하나, character atom은 문자 하나입니다. box 타입은 box가 atom입니다. 1 'a' (<'boxed')는 모두 atom입니다.

어떤 것인지 알 것 같습니다만 조금 더 공식적인 정의를 하려면 어떻게 할까요? 다시 말해 어떻게 낱개(atom)이라는 것을 알 수 있을까요? 해답은 shape과 rank에 있습니다. 모든 noun은 shape과 rank를 가지고 있습니다. noun이 atom이라면 그 noun의 shape은 '' 이며 rank는 0입니다.(용어를 설명하기 위해 다른 새로운 용어를 가져와서 미안합니다. 자세한 설명은 array에서 나오게 됩니다. 우선 편안한 마음으로 일단 받아들입시다. 그러려니 하면서 말입니다.) 이를 확인하려면 monad $ 를 이용합니다. $ y 를 실행하면 noun y의 shape을 얻을 수 있습니다.입력 다음에 나오는 빈 칸은 띄어쓰기가 아니라 '비어있는'이라는 의미로 ''를 반환한 결과입니다.
		
		$ 1

		$ 'a'

		$ <'I am boxed.'
	
데이터는 낱개로 다루는 것보다 필요한만큼 그리고 의미를 확보할 수 있을만큼 크게 덩어리를 지어 다루는 것이 편리합니다. 데이터를 한 손으로 셀 수 없을만큼 많아졌다면 각각의 데이터에 변수를 지정해 연산하는 것은 거의 불가능에 가까워집니다. 이런 문제는 array를 이용하면 쉽게 극복가능합니다.

array를 정의합시다. 여러 개의 atom이 'axe'를 따라 조직되어 있는 noun을 array라고 합니다. J언어는 하나 이상의 축을 사용할 수 있습니다. 축이 하나인 경우를 보통 list라고 지칭하고, 두 개인 경우를 table, 세 개인 경우를 brick이라고 지칭합니다. 축의 갯수에 제한은 없습니다.

		] list =: i. 2
	0 1
   		] table =: i. 2 3
	0 1 2
	3 4 5
   		] brick =: i. 2 3 4
 	0  1  2  3
 	4  5  6  7
 	8  9 10 11

	12 13 14 15
	16 17 18 19
	20 21 22 23
		NB. brick을 쌓아올려 도서관을 지었습니다.
		] library =: i. 2 2 2 2 
 	0  1
 	2  3

	4  5
 	6  7


 	8  9
	10 11

	12 13
	14 15
	 	] boxed =: 2 2 $ ;:'spring summer autumn winter'
	┌──────┬──────┐
	│spring│summer│
	├──────┼──────┤
	│autumn│winter│
	└──────┴──────┘

shape은 noun이 어떤 구조로 구성되어 있는지 알려줍니다. noun의 각 축마다 몇 개의 atom이 있는지를 numeric list로 표현합니다. monad $가 noun의 shape을 알려줍니다.

		$ 'This is a string' NB. character 16개가 하나의 축을 기준으로 나열되었습니다.
	16
		] m =: 2 3 1 ,: 1 2 3 NB.
	2 3 1
	1 2 3
		$ m
	2 3
		] boxed =: ('Harder' ; 'Better') ,: 'Faster' ; 'Stronger' 
	┌──────┬────────┐
	│Harder│Better  │
	├──────┼────────┤
	│Faster│Stronger│
	└──────┴────────┘
		$ boxed
	2 2

rank는 축의 갯수(⇔ shape 리스트의 길이)를 말합니다. atom의 rank는 0(축이 없습니다), list는 1, table은 2, brick은 3을 rank로 가집니다. 더 큰 rank를 가지는 noun도 얼마든지 가능합니다.

		# $ 1
	0
		# $ m =: 2 3 1 ,: 1 2 3
	2
		# $ b =: i. 2 3 4
	3

![이 사진은 본문 내용과 관계가 있을까요?](https://i.fltcdn.net/contents/1506/original_1437127097313_7ej5liejyvi.jpeg) 이 사진은 본문 내용과 관계가 있을까요?

그런데 말입니다. array를 이해하는 데는 하나가 아닌 여러가지 방법이 있습니다. array를 분해해서 array의 array로 이해하는 것도 가능하기 때문입니다.

군대 이야기를 하며 이해를 도와봅시다. 하나의 사단을 생각해봅시다. 여러분이 사단장이라면 사단 하나라고 말할 것입니다. 하지만 연대를 기본단위로 삼는 경우는 사단을 연대 4개와 동일합니다. 마찬가지 방법으로 사단은 4개 연대 4개 대대라고 말할 수도 있습니다. 대대 16개로 말하는 것보다 4개 연대 각 4대대라고 말하는 것이 사단의 구조를 더 쉽게 이해할 수 있도록합니다. 같은 방법으로 중대를 기본 단위로 하면 사단은 4개 연대, 각각 4개 대대, 각각 4개 중대 쯤으로 부를 수 있을 것입니다. 이런 식으로 계속 내려가 마지막에는 병사 개개인을 기본단위로 삼는 방법까지 있을 수 있습니다.

array도 마찬가지 방법으로 이해할 수 있습니다. 기본단위를 무엇으로 보느냐에 따라 array를 읽는 방법은 다양합니다. 예를 들어 noun shape이 2 3 4 5(rank는 4)이라면 여러가지 방법으로 shape을 이해할 수 있습니다. 아래의 목록에서 0은 atom을 뜻합니다(atom의 shape은 ''(empty)이고 rank는 0입니다).

* 0(atom) \\(\times\\) 2 3 4 5
* 2 \\(\times\\) 3 4 5
* 2 3 \\(\times\\) 4 5
* 2 3 4 \\(\times\\) 5
* 2 3 4 5 \\(\times\\) 0(atom)

[[:TODO:]]
atom의 shape이 ''인 이유. k-cells에 붙이면 되니까. 그러면 shape이 ''인 이유를 설명할 수 있습니다.
[[:TODO:]] negative rank 설명할 수 있도록 확장하기.

Frames and Cells. A rank r splits the argument shape into the frame and the cell shape; a positive r specifies the number of trailing cell axes, while a negative r specifies the negative of the number of leading frame axes.

[source](http://www.jsoftware.com/papers/rank.htm)

noun의 k-cells이 정해지면 여기에 대응되는 frame이 정해집니다. 예를 들어 noun shape이 2 3 4 5(rank는 4)이며 k-cells의 k값이 2라면, frame은 shape의 마지막 원소 2개를 제외한 2 3이 됩니다. k-cells을 atom처럼 본다면 noun의 새로운 shape이 얼마인지라는 질문에 답하는 개념입니다. 

이런 기본단위를 k-cell이라고 부르겠습니다. k-cell에서 k는 기본단위의 rank입니다. 위의 사단이야기를 여기에 적용하면 k-cell의 k값이 커질수록 단위 편재가 커집니다. k가 0일 경우 병사 개인을 기본단위로 삼는 것이고, k가 1이라면 k-cell은 분대를 지칭합니다. array를 이루는 기본묶음을 어떻게 볼 것인지가 k-cell에서 k의 의미입니다.
	
		NB. 1) atom(shape: ''), k-cell의 shape: 2 3 4
		<"3 i. 2 3 4
	┌───────────┐
	│ 0  1  2  3│
	│ 4  5  6  7│
	│ 8  9 10 11│
	│           │
	│12 13 14 15│
	│16 17 18 19│
	│20 21 22 23│
	└───────────┘
		NB. 2) list의 shape: 2, k-cell의 shape: 3 4
		<"2 i. 2 3 4
	┌─────────┬───────────┐
	│0 1  2  3│12 13 14 15│
	│4 5  6  7│16 17 18 19│
	│8 9 10 11│20 21 22 23│
	└─────────┴───────────┘
		NB. 3) table의 shape: 2 3, k-cell의 shape: 4
		<"1 i. 2 3 4
	┌───────────┬───────────┬───────────┐
	│0 1 2 3    │4 5 6 7    │8 9 10 11  │
	├───────────┼───────────┼───────────┤
	│12 13 14 15│16 17 18 19│20 21 22 23│
	└───────────┴───────────┴───────────┘
		NB. 4) brick의 shape: 2 3 4, k-cell의 shape: ''
		<"0 i. 2 3 4
	┌──┬──┬──┬──┐
	│0 │1 │2 │3 │
	├──┼──┼──┼──┤
	│4 │5 │6 │7 │
	├──┼──┼──┼──┤
	│8 │9 │10│11│
	└──┴──┴──┴──┘
	
	┌──┬──┬──┬──┐
	│12│13│14│15│
	├──┼──┼──┼──┤
	│16│17│18│19│
	├──┼──┼──┼──┤
	│20│21│22│23│
	└──┴──┴──┴──┘

noun rank shape, rank, k-cell. shape은 noun의 각 축을 따라 몇개의 atom이 배열되어 있는지 알려주는 numeric list입니다. rank는 shape의 길이를 말하며 rank를 이용해 몇 개의 축을 따라 atom이 배열되어 있는지 알 수 있습니다. k-cell은 array를 이루는 기본단위의 rank를 말합니다.

k-cells에 딸림으로 나오는 frame에 대한 설명을 마지막으로 추가하겠습니다. noun의 k-cells이 정해지면 여기에 대응되는 frame이 정해집니다. 예를 들어 noun shape이 2 3 4 5(rank는 4)이며 k-cells의 k값이 2라면, frame은 shape의 마지막 원소 2개를 제외한 2 3이 됩니다. k-cells을 atom처럼 본다면 noun의 새로운 shape이 얼마인지라는 질문에 답하는 개념입니다. 

Frames and Cells. A rank r splits the argument shape into the frame and the cell shape; a positive r specifies the number of trailing cell axes, while a negative r specifies the negative of the number of leading frame axes.

#### verb rank ####
> The rank of a verb speciﬁes the number of axes in the arrays to which the verb naturally applies.[- The Role of APL and J in High-performance Computation](http://www.snakeisland.com/aplhiperf.pdf)

> The rank of a verb is the highest rank the verb is prepared to handle
[- J Vocabulary](http://code.jsoftware.com/wiki/Vocabulary/VerbRank)

모든 verb는 rank를 가지고 있습니다. 이를 verb rank라고 부르며 verb가 적용되는 noun의 최대 rank를 지칭합니다. 만약 verb rank가 noun rank보다 크다면 verb는 한번만 실행됩니다. 반대로 noun rank가 더 크다면 noun은 array로 나누어져 verb rank가 허락하는 최대 rank에 각각 적용됩니다. 각 조각별로 실행된 결과는 한 덩어리로 묶여 verb를 실행한 결과가 됩니다. [source](http://code.jsoftware.com/wiki/Vocabulary/VerbRank)

(file:///C:/Users/yunskim/j64-804/addons/docs/help/dictionary/dictb.htm)

J의 verb는 모두 rank를 가지고 있습니다. 예외가 있지만 기본적으로 verb rank는 3개짜리 numeric list입니다. 이 리스트는 흔히 m l r 로 표기됩니다. m은 monad일 때의 verb rank, l은 dyad일 때 left argument에 적용되는 rank, 마지막으로 r은 dyad인 경우 right argument에 적용되는 rank입니다. 모든 verb가 rank를 가지고 있다고 하니 모두 외워야 하는거 아닌지 덜컥 겁이 날 수도 있습니다. 진정하세요. 저자도 다 못 외웁니다. 그리고 기억은 믿을만한 것이 아니기 때문에 다른 방법을 이용하는 것을 더 권장합니다. verb rank를 확인하는 두가지 방법이 있습니다. 첫번째는 vocabulary를 참고하는 것입니다. 머릿글에 verb rank가 크게 나오니 궁금할 때 확인하는 것도 primitive와 친해지는 좋은 방법입니다. 다른 방법으로 conjunction b. 를 이용하는 방법이 있습니다. conjunction u b. 0 은 verb u의 rank를 알려줍니다. 아래 예제를 확인해보세요.

		+ b. 0 NB. conjunction u b. 0 은 verb u의 rank를 알려줍니다.
	0 0 0
		- b. 0 NB. 
	0 0 0
		; b. 0
	_ _ _
		,. b. 0
	_ _ _
		{. b. 0
	_ 1 _
		%. b. 0
	2 _ 2

verb rank가 있다는 것도 알겠습니다. 어떻게 참고하는지도 알겠습니다. 그런데 이거 어디에 쓰나요? 이거 왜 필요한가요? 중요하다면서요?

다시 한 번 사단장이 되어 봅시다. 사단장이 내릴 수 있는 명령은 여러 종류가 있습니다. 사단 전체에 명령을 내리지만 어떤 명령은 대대 단위로 실행이 되어야 하고 어떤 명령은 병사 각각에게 전달되어야 합니다. 사단장이 사단에 명령을 내립니다. 연대 단위로 공격이나 수비를 하라고 명령합니다. 사단장이 사단에게 명령을 내립니다. 병사 개별로 휴식을 취하라는 명령입니다.

중요한 점은 사단장은 언제나 사단 전체에 명령을 한번만 내립니다. 연대마다 따로 명령을 내리거나 대대마다 전령을 따로 보내지도 않습니다. 언제나 명령은 사단 전체에 단 한번 전달됩니다. 그런데 어떻게 연대, 대대 그리고 각 병사가 명령을 듣고 반응할 수 있을까요?

비교하자면 파이썬이나 C에서 자주 사용되는 반복문은 마치 각 단위마다 따로 명령을 통지하거나 전령을 보내는 것과 비슷합니다. 배열이나 이터레이터를 사용한 반복문은 기본적으로 단위를 프로그래머가 정하고 각 단위에 접근할 수 있는 방법을 지정해 각 단위가 이해하는 명령을 여러번 반복해서 보내는 과정입니다. 너무나 필수적이고 대체가 불가능한 방식처럼 보입니다. J는 대체 무슨 믿는 구석이 있어서 반복문을 사용하지 않아도 된다고 말하는 걸까요? 

지혜로우신 J사단장은 미리 명령이 적용되는 최소단위를 정해놓았습니다. 어떤 명령은 대대를 최소단위로 하고 어떤 명령은 연대가 최소단위입니다. 이렇게 정해놓으면 여러 번 명령을 보낼 필요없이 사단 전체에 명령을 한 번만 보내도 각 단위가 명령을 알아듣고 사단장의 의도대로 움직입니다. 연대가 최소단위인 명령은 각 연대가 명령을 따라, 대대가 최소단위라면 각 대대가 명령을 실행합니다. 자비로우신 J사단장이 병사 개개인에게 휴식을 명령하면 각자 지친 몸을 가누고 양말을 갈아 신겠지요.

J사단장이 내리는 명령을 마치 verb처럼 생각해봅시다. 각각의 명령이 적용되는 최소단위가 있듯, verb가 적용되는 최소단위가 있습니다. 바로 그 최소단위가 verb rank입니다. verb rank가 k인 verb는 noun의 k-cell 각각에 적용됩니다. verb rank가 _(infinity)라면 noun 전체를 대상으로 verb가 적용되고, verb rank가 0이라면 verb는 noun의 0-cells에 적용됩니다. verb rank에 해당되는 모든 k-cells은 각각 verb의 적용을 받게됩니다. 따로 반복문을 사용하지 않더라도 모든 k-cells을 대상으로 verb를 적용할 수 있는 것입니다.

예를 들어 rank가 연대인 명령 'u'가 있다고 합시다. 사단장님이 가로되 '사단이여 u 하여라' 하시면 따로 신경쓰지 않더라도 사단은 연대의 array처럼 다루어져 각 연대별로 u를 적용하고 마지막으로 각 연대가 처리한 값을 하나로 합칩니다.

		>: b. 0 NB. monad >: 의 랭크는 0 
	0 0 0
		>: i. 5 NB. 0-cells 각각에 +1
	1 2 3 4 5
		%. b. 0 NB. monad %. 의 랭크는 2
	2 _ 2
		%. ?. 3 2 2 $ 10x NB. 세 개의 2 by 2 matrix inverse를 구합니다.
	 _1r4   1r4
	  1r3  _1r6
	
	 _1r3   4r9
	  1r3 _5r18
	
	_2r15   3r5
	  1r5  _2r5
		< b. 0 NB. monad < 의 rank는 _(infinity)입니다.
	_ 0 0 
		< %. ?. 3 2 2 $ 10x NB. 따라서 monad < 은 무엇을 입력값으로 받든 전체를 boxing합니다.
	┌───────────┐
	│ _1r4   1r4│
	│  1r3  _1r6│
	│           │
	│ _1r3   4r9│
	│  1r3 _5r18│
	│           │
	│_2r15   3r5│
	│  1r5  _2r5│
	└───────────┘

모든 값이 verb rank로 사용되는 것은 아닙니다. verb rank는 0 1 2 _ 중 하나를 값으로 택합니다. 비슷한 verb rank를 가진 verb는 비슷한 역할을 합니다.

verb rank가 _ 인 경우는 array를 조작하는 경우가 많습니다. verb가 처리할 수 있는 최대단위가 _(infinity)라면 아무리 큰 noun이라도 한번에 처리할 수 있습니다. 따라서 전체 noun을 하나의 큰 덩어리로 보고 처리하게 됩니다. array를 잇는 ,와 boxed list를 만드는 ; 등은 verb rank가 _ _ _ 입니다.

		'infinite rank'  ; i. 2 2 NB. noun의 rank에 관계없이 전체를 대상으로 ; 을 적용합니다.
	┌─────────────┬───┐
	│infinite rank│0 1│
	│             │2 3│
	└─────────────┴───┘

rank가 0인 verb는 scalar function이라고 불리며, 이들은 이름에 걸맞게 단일값의 수치연산과 관련된 경우가 일반적입니다. `+ - * % ^ ^. <. >. < <: = >: > +. *. +: *: ~ o. ? |` 등이 rank를 0으로 가지는 rank-0 verb입니다.

		%: _1 0 1 NB. 제곱근
	0j1 0 1
		%. _1 0 1 NB. 역수(reciprocal)
	_0.5 0 0.5

1 혹은 2를 rank로 가지는 경우는 verb의 정의와 rank가 밀접하게 연관되어 있는 경우가 많습니다. 보통 rank 1은 list를 다루는 verb와 관련된 경우가 많습니다. 2를 rank로 가지는 verb는 행렬을 다루는 경우가 많습니다. rank 2를 가지고 있는 대표적인 verb로 `%.`(matrix inverse / matrix divide)가 있습니다. 이름에서 알 수 있듯 `%.` 는 행렬과 긴밀한 관계를 가지고 있습니다.
		
		NB. verb rank 1		
		] m =: (,: -.) 1 1 0 0 1 0 1 0
	1 1 0 0 1 0 1 0
	0 0 1 1 0 1 0 1
		I. m NB. Boolean 'list'에서 1이 위치한 곳의 인덱스값을 돌려줍니다.
	0 1 4 6
	2 3 5 7
	
		NB. verb rank 2
		%. ?. 2 2 $ 5
	 1 _1
	_3  4
	
##### rank conjunction u"n #####
항상 정해진 verb rank를 사용할 필요는 없습니다. 필요하다면 원하는 verb rank를 가진 verb를 새로 만들 수 있습니다. conjunction "는 u"n 형식으로 사용되며 verb rank가 n인 verb를 새롭게 '정의'합니다. 새롭게 정의된 verb u"n은 noun을 n을 rank로 가지는 cell로 쪼개 각 cell에 u를 적용합니다.

다르게 말하면 u"n 은 u를 n-cells에 적용합니다.

u의 자체의 rank는 바꾸지 않습니다. 일단 n에 따라 cell이 쪼개지면 각 조각에 대해서는 u의 rank가 적용됩니다.

conjunction " 을 이용해 verb rank를 지정하는 방법에는 세가지가 있습니다.
- m l r 지정하기:		세 개의 숫자로 monad rank m과 left verb rank l, right verb rank r을 따로 지정합니다
- l r 지정하기:		두 개의 숫자로 left verb rank, right verb rank를 지정합니다. monad rank m는 right verb rank r을 준용합니다.
- m 지정하기:			monad verb rank와 left verb rank, right verb rank가 모두 동일하게 m으로 지정됩니다. 

###### rank conjunction 쓰임새 ######
가끔 프로그래밍 언어 책을 읽다보면 내가 이런 것까지 알아야 하는지 의구심이 들 때가 있습니다. 너무 문법설명에 치우친 것은 아닌지 내가 이걸 쓸 일이 정말 있을지 의심하게 됩니다. 지금 여러분이 그런 기분이실 수 있습니다. "아니 있는 것 그냥 쓰지 뭘 다시 정해서 새롭게 만드냐? 가뜩이나 모르는 것들이 나와 마음이 불편한데 이것까지 알아야 하는 건가?"

다시 생각하셔야 합니다. 여러분들은 conjunction "을 사랑하게 되실 겁니다. 이제 너무 단순하거나 또는 너무 거창한 예가 아니라 생활에서 접할 법한 사례를 살펴보고 여러분이 conjunction "을 사랑할 준비가 되었는지 알아봅시다.

여러분은 기계학습에 관심이 많아 J를 사용해 기계학습을 연구하고 싶습니다. 이런 알고리즘도 좋고 저런 알고리즘도 좋은 것 같습니다. 이제 데이터에 새로운 아이디어를 적용하고 싶습니다. 무엇을 하던 데이터가 필요하지요. 유명한 아이리스 데이터를 사용하겠습니다. [주소](http://archive.ics.uci.edu/ml/datasets/Iris)를 구했습니다. 이제 이 원자료를 읽어 전처리를 해야 합니다. 이 단계까지 일단 해볼까요?
		
		require 'web/gethttp'
		URL =: 'http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'
		data =: _5 ]\ (LF, ',') cutopen gethttp URL
		
		5 {. data NB. 데이터의 첫 5줄을 출력합니다. 꽃받침 길이 , 꽃받침 너비, 꽃잎 길이, 꽃잎 너비, 강(class)(class)
	┌───┬───┬───┬───┬───────────┐
	│5.1│3.5│1.4│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│4.9│3.0│1.4│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│4.7│3.2│1.3│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│4.6│3.1│1.5│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│5.0│3.6│1.4│0.2│Iris-setosa│
	└───┴───┴───┴───┴───────────┘

이제 꽃받침 너비를 list로 따로 꺼내고 싶습니다. 꽃받침 너비는 두번째 열에 기록되어 있습니다. 어떻게 하면 좋을까요? 간단하게 { 를 사용하겠습니다. 

만약 1&{ data 를 실행하면 두번째 열이 아니라 두번째 행을 꺼내옵니다. 왜냐하면 1&{ 의 rank는 _ 이기 때문에 두번째 아이템을 가지고 오는 것이지요. 
	
		 1&{ data
	┌───┬───┬───┬───┬───────────┐
	│4.9│3.0│1.4│0.2│Iris-setosa│
	└───┴───┴───┴───┴───────────┘

이 문제를 해결하려면 1&{ 의 rank를 바꿔 새로운 verb를 정의하면 가능합니다. 1&{"1 은 rank 1을 verb rank로 가집니다. 따라서 전체에 1&{ 를 적용하지 않고 리스트의 array로 data를 먼저 쪼갠 다음 각 리스트에 1&{ 를 적용합니다.

		  5 {. 1&{"1 data NB. 첫 5행의 꽃받침 너비
	┌───┬───┬───┬───┬───┐
	│3.5│3.0│3.2│3.1│3.6│
	└───┴───┴───┴───┴───┘

![참 쉽죠?](http://38.media.tumblr.com/tumblr_lz412ce5BK1r2yonh.jpg)


##### [the rank of verbs generated by modifiers](http://code.jsoftware.com/wiki/Vocabulary/RankFromV) #####
#### How rank works? ####
지금까지는 rank가 무엇인지 설명하는데 중점을 두었습니다. '어떻게' verb rank가 동작하는지 간략하게 언급하고 넘어갔는데 이제 rank 메커니즘이 어떻게 동작하는지 자세히 들여다 볼 시간이 왔습니다.

rank 메커니즘은 3단계를 거칩니다.

* 분할(Subdivision) - verb rank에 따라 입력값을 array of cells로 분할합니다.
* 실행(Execution) - 각각의 cell에 대해 verb를 실행합니다.
* 조립(Assembly) - cell의 결과값을 한데 모아 최종 결과값을 만듭니다.
	* 채우기(Fill) - 혹시 빈 자리가 있으면 적당한 값으로 채워넣습니다. 

##### 분할(subdivision) #####
하나의 array를 이해하는 데는 하나 이상의 방법이 있습니다. 기본 단위를 무엇으로 삼느냐에 따라 array는 여러 방식으로 이해됩니다. verb가 실행될 때 입력되는 array의 기본단위는 verb의 rank를 따릅니다. verb rank가 k일 때 array는 k-cell을 기본단위로 가지는 array로 이해됩니다. 이런 과정을 분할이라고 부릅니다.

##### 실행(execution) #####
분할된 각 cell에 대해 verb가 각각 적용됩니다. 

##### 조립(assembly) #####
각 cell에서 verb가 적용된 결과는 하나의 array로 합쳐집니다. 합쳐진 결과의 rank는 k-frame 입니다.

##### 채우기(fill) #####
조립과정에서 각 cell의 shape은 동일해야 합니다. 만약 shape이 다른 cell이 있는 경우 모자라는 cell에 fill을 채워 shape을 맞춥니다. 결과값이 numeric일 경우 0을 사용해 채우고, 문자일 경우 빈 칸을 삽입하며 혹시 boxed된 경우는 `a:`를 사용합니다.

아래의 예는 fill을 적용하지 않은 경우와 fill을 적용해 0을 추가하는 경우를 비교합니다. monad `i.` 의 rank는 1로 3 4 5 각각에 `i.`를 적용합니다.

		i. each  ,. 3 4 5
	┌─────────┐
	│0 1 2    │
	├─────────┤
	│0 1 2 3  │
	├─────────┤
	│0 1 2 3 4│
	└─────────┘

		i. ,. 3 4 5
	0 1 2 0 0
	0 1 2 3 0
	0 1 2 3 4

conjunction !.(fit)을 이용해 채우기를 할 때 사용할 문자나 숫자를 지정할 수 있습니다.
	
		'asdf' ,: 'jkl'
	asdf
	jkl 
		'asdf' ,:!.'*' 'jkl'
	asdf
	jkl*
		 ] b =: 1 2 3 ; i. 2 2
	┌─────┬───┐
	│1 2 3│0 1│
	│     │2 3│
	└─────┴───┘
		; b
	1 2 3
	0 1 0
	2 3 0
		(;!.100) b
	1 2   3
	0 1 100
	2 3 100

##### dyad verb rank #####
dyad verb는 어떻게 다뤄야 할까요? dyad의 경우는 한가지 더 주의를 기울여야 합니다. rank 문제로 length error 메세지를 만나는 경우를 들어 보겠습니다.

		NB. dyad + 의 rank가 0 0이라는 점을 생각해보면 납득이 갑니다.
		1 + 1 2 3 
	2 3 4
		NB. dyad 양쪽의 frame이 동일합니다.
		1 2 3 + 1 2 3
	2 4 6
		NB. rank가 일치하지 않아 에러 메시지를 보여줍니다.
		1 2 +  1 2 3 
	|length error
	|   1 2    +1 2 3


dyad는 한가지 단계를 더 거칩니다. left operand와 right operand의 noun rank를 비교하는 agreement과정입니다.
###### agreement ######
dyad는 left verb rank와 right verb rank 두 개를 verb rank로 가집니다. 양쪽의 verb rank에 따라 left operand와 right operand의 frame이 결정됩니다. 가장 간단한 예는 이 프레임이 일치하는 경우입니다.

		2 3 + 3 4
	5 7

위의 예에서 dyad `+`의 rank는 `0 0`입니다. left operand의 frame은 따라서 `2 3`이고 right operand의 frame도 `2 3`으로 동일합니다. 양쪽의 shape이 같으므로 `0 0` rank를 따라 dyad `+`를 적욯합니다.

		NB. subdivision
		2 3 , each 3 4
	┌───┬───┐
	│2 3│3 4│
	└───┴───┘
		NB. execution
		 +/ each 2 3 , each 3 4
	┌─┬─┐
	│5│7│
	└─┴─┘
		NB. assembly
		> +/ each 2 3 , each 3 4
	5 7

이번에는 argument의 rank를 더 올려볼까요? 양쪽 operand의 frame은 `2 2`로 동일합니다.
		
		NB. (2 3 ,: 4 5) * (6 7 ,: 8 9)
		* b. 0
	0 0 0
		NB. subdivision
		(2 3 ,: 4 5) <@(,"0) (6 7 ,: 8 9)
	┌───┬───┐
	│2 6│3 7│
	├───┼───┤
	│4 8│5 9│
	└───┴───┘
		NB. execution
		 */ each (2 3 ,: 4 5) <@(,"0) (6 7 ,: 8 9)
	┌──┬──┐
	│12│21│
	├──┼──┤
	│32│45│
	└──┴──┘
		NB. assembly
		> */ each (2 3 ,: 4 5) <@(,"0) (6 7 ,: 8 9)
	12 21
	32 45

하지만 양쪽의 frame이 같다는 것은 늘 기대할 수 없는 일이고 만약 이런 경우에만 dyad를 적용할 수 있다면 너무 불편할 것입니다. 양쪽의 frame이 다른 경우를 다루는 방법으로 몇가지 방법이 있습니다. J는 아래의 방법 중 'scalar agreement'와 'prefix agreement'를 채택했습니다.

* scalar agreement: 한 쪽의 frame이 empty일 때(scalar일 때) 다른 쪽 frame에 맞춰 shape을 수정합니다.
			
			NB. dyad + 의 rank는 0 0입니다. 따라서...
			NB. scalar 1의 frame은 ''(empty)입니다. 그리고...
			NB. i. 2 3의 frame은 2 3 입니다.
			NB. scalar 1은 (i. 2 3)의 frame에 맞게
			NB. 복사되어 frame 2 3을 만듭니다.

			1 + i. 2 3
		1 2 3
		4 5 6

* prefix agreement: 한 쪽의 frame이 다른 쪽 frame의 prefix 일 경우 prefix에 맞춰 재배열됩니다. prefix agreement는 leading axes에 더 큰 의미를 둡니다.
 			
			NB. dyad + 의 rank는 0 0입니다. 따라서...
			NB. 100 200의 frame은 2입니다. 그리고...
			NB. i. 2 3의 frame은 2 3 입니다.
			NB. 100 200 은 (i. 2 3) frame의 prefix 2에 대응되어...
			NB. (100 + 0 1 2)와 (200 + 3 4 5)로 분할됩니다.

			100 200 + i. 2 3
		100 101 102
		203 204 205

* suffix agreement: 한 쪽의 frame이 다른 쪽 frame의 suffix 일 경우 suffix에 맞춰 재배열됩니다. J에서 채택된 방법은 아니지만 여전히 정당한(legitimate) 방법입니다. J에서는  `"`(rank conjunction)을 이용해 이를 구현할 수 있습니다. 
			
			100 200 300 + i. 2 3
		|length error
		|   100 200 300    +i.2 3
			100 200 300 +"1 i. 2 3
		100 201 302
		103 204 305

###### zero frame ######
frame이 0을 포함하고 있는 경우 agreement를 진행할 수 없습니다.
	
		] placeholder =: i. 0 4
   
   		] data =: placeholder , 0 1 2 3
	0 1 2 3
   		] data =: data, 4 5 6 7
	0 1 2 3
	4 5 6 7

Zero Frame. If the frame contains 0 (as in 3 *"1 i. 0 4), there are no argument cells to apply v to, and the shape of a result cell (the value of sir) is indeterminate. Pesch [1986] describes a variety of strategies to address this problem. In J, the shape is calculated if v is uniform (see below); otherwise v is applied to a cell of fills.


[Agreement](http://code.jsoftware.com/wiki/Vocabulary/Agreement)
[Rank and Uniformity](http://www.jsoftware.com/papers/rank.htm)
###### subdivision ######
(http://code.jsoftware.com/wiki/Vocabulary/VerbRank#How_the_Rank_Mechanism_Works)
###### execution ######
###### assembly ######

Rank-0 verbs, the so-called scalar functions, obtain their name from their characteristic of independent operation on each scalar element of their argument array(s). Figure 1 shows the operations which are in both APL and J as scalar verbs. Fortran 90 has included some of these scalar verbs in its numeric functions and mathematical functions. 

* rank가 k인 verb가 매개변수의 k-cell 각각에 적용됩니다(a verb of rank k applies to each of the k-cells of its argument)


* verb의 rank는 verb가 적용되는 The rank of a verb speciﬁes the number of axes in the arrays to which the verb naturally applies.(The Role of APL and J in High-performance Computation)
* A verb of rank r is defined on arguments with rank bounded by r (Rank and Uniformity)
숫자는 차례로 monad일 때의 verb rank, dyad일 때의 left ope
verb rank는 
Verbs for arithmetic, which operate on numbers, have rank 0, indicating that they operate on each atom independently.

Verbs that operate on entire arrays generally have infinite rank.

Specialized verbs have appropriate rank. For example, the verb for matrix inverse, (%. y), has rank 2. 

The rank-mechanism sequence is:

    Subdivision - split the argument(s) into cells that have the desired rank
    Execution - execute the verb on each cell
    Assembly - assemble the cell-results into an array
        Fill - as part of assembly, pad the cell-results to a common shape

#### rank conjunction ####
#### expansion ####
[[:TODO:]]
누가 shape의 front에 주목했었지? whitney입니다.
[Rank and Uniformity](http://www.jsoftware.com/papers/rank.htm)
#### item ####


#### noun rank ####
The concept of function rank is fundamental to array parallelism in APL. The rank of a verb speciﬁes the number of axes in the arrays to which the verb naturally applies.


		For any verb, each valence (monad or dyad) has a rank which specifies the largest k-cell that valence can be applied to:

			The rank of the monad is a single number
			The rank of the dyad is two numbers, one for each argument, x and y.

		Typically the ranks of both valences are reported together (e.g. by b.0) as a list of 3 atoms:

			 Atom 0 - the monad rank (i.e. of the y-argument)
			 Atom 1 - the left dyad rank (i.e. of the x-argument)
			Atom 2 - the right dyad rank (i.e. of the y-argument).

	For example, if the verb rank of the monad v is k, J computes (v y) as follows:

		1. splits y into several k-cells 2. applies the verb v to each k-cell in turn 3. assembles the results for all k-cells into a single array result by use of the frame of y.

	If the rank of the argument is less than or equal to the verb rank k, the verb is applied to the entire argument.

	It follows that to specify a verb rank of Infinity (_) guarantees that the corresponding k-cell is the entire argument (x or y), however large its rank happens to be. 
*	noun rank
		Shape and Rank

		The shape of a noun is a list giving the number of atoms along each axis. Since an atom has no axes, its shape is an empty list.

		The number of axes, which is the same as the number of atoms in the shape, is called the rank of a noun.

	 Name 	Rank
	 atom 	0
	list 	1
	table 	2
	brick 	3 

	i. 4  NB. shape is 4
		0 1 2 3
	 i. 2 3   NB. shape is 2 3, i. e. 2 rows, 3 columns
	0 1 2
	3 4 5

The sentence $ y gives the shape of y. The sentence # $ y gives the rank of y. 
*	rank conjunction

세가지는 서로 밀접하게 연관되어 있지만 따로 구분하지 않아도 
The concept of function rank is fundamental to array parallelism in APL. The rank of a verb speciﬁes the number of axes in the arrays to which the verb naturally applies. For example, addition (x+y) is deﬁned on scalars addingto scalars, so is rank 0 0. Matrix inverse (%.y) is deﬁned on tables (matrices), so is rank 2. Rotate (x|.y), or end-around shift, is deﬁned on a scalar left argument, which speciﬁes the number of positions to be rotated, and a list or vector right argument to be rotated, so rotate is rank 0 1. Since the ravel verb makes its entire argument into a list, it is classiﬁed as rank ∞. Operation of a rank-k verb upon arrays of higher rank than k is deﬁned asindependent application of the verb to each rank-k subarray of the argument, with the ﬁnal result being formed by laminating the individual results. There is no speciﬁed temporal ordering, so side effects can not be depended on. Data dependencies simply do not exist. Consider a few examples of how this works in


array
atom
axis
cell
frame
list
rank of noun
rank of verb
scalar
shape
table
valence
vector
framing fill
verb fill

## dictionary ##
### arithmetics ###
#### < ####
##### < y #####
Box monad _

때때로 primitive의 모양을 보면 어떤 일은 하는지 짐작할 수 있는 경우가 있습니다. monad < 도 이런 경우에 해당합니다. 자세히 들여다보면 가끔씩 < 가 통발의 입구처럼 보입니다.  monad < 는 마치 통발처럼 y를 box안으로 집어넣습니다. 생선(y)이 통발의 입구(<)를 통과하면 그물(box) 안으로 들어오게 되고 생선(y)이라고 생각했던 것이 문어(noun), 장어(list), 쓰레기(string) 그리고 무엇이든 그물(boxed list)을 휘어잡기만 하면 단일 묶음으로 쉽게 다룰 수 있습니다.

###### 정의 ######
< y 는 y를 boxed noun으로 만듭니다. boxed noun은 y의 내용이나 크기와 관계없이 atom으로 취급됩니다(⇔ monad <의 rank는 _입니다). "<y is and atomic encoding of y."

		< i. 3 NB. y를 boxed noun으로 만들면 box가 y를 둘러싸고 있는 것처럼 화면에 출력됩니다.
	┌─────┐
	│0 1 2│
	└─────┘
		$ < i. 3 NB. (< i. 3)은 atom으로서 $ 를 적용하면 빈 칸이 나옵니다. 

		(<'Hello, world!') , (<i. 2 3 4) , (< i. 5 6) NB. boxed noun은 atom처럼 다룰 수 있고 list를 만들 수 있습니다.
	┌─────────────┬───────────┬─────────────────┐
	│Hello, world!│ 0  1  2  3│ 0  1  2  3  4  5│
	│             │ 4  5  6  7│ 6  7  8  9 10 11│
	│             │ 8  9 10 11│12 13 14 15 16 17│
	│             │           │18 19 20 21 22 23│
	│             │12 13 14 15│24 25 26 27 28 29│
	│             │16 17 18 19│                 │
	│             │20 21 22 23│                 │
	└─────────────┴───────────┴─────────────────┘

		< (<'like this'), a: , < 'boxed list contains boxed noun' NB. boxed noun을 다시 'boxing'할 수 있습니다.
	┌───────────────────────────────────────────┐
	│┌─────────┬┬──────────────────────────────┐│
	││like this││boxed list contains boxed noun││
	│└─────────┴┴──────────────────────────────┘│
	└───────────────────────────────────────────┘

###### 쓰임새 ######
내용에 상관없이 관련있는 noun을 묶을 수 있습니다. 마치 C언어의 구조체처럼 다른 데이터 타입을 하나로 묶을 수 있습니다.

		(<'abc') , (<+:`-:) , (<i. 2 3) , <<<'level 3 box' NB. 마음먹고 복잡하게 하려면 이것보다 더 복잡한 것도 가능합니다.
	┌───┬───────┬─────┬───────────────┐
	│abc│┌──┬──┐│0 1 2│┌─────────────┐│
	│   ││+:│-:││3 4 5││┌───────────┐││
	│   │└──┴──┘│     │││level 3 box│││
	│   │       │     ││└───────────┘││
	│   │       │     │└─────────────┘│
	└───┴───────┴─────┴───────────────┘
	
###### 조금 더 ######
monad ;: 를 사용하면 string을 boxed list로 만들 수 있습니다.

		;: 'what a lovely day! what a lovely programming language J is!'
	┌────┬─┬──────┬───┬─┬────┬─┬──────┬───────────┬────────┬─┬──┬─┐
	│what│a│lovely│day│!│what│a│lovely│programming│language│J│is│!│
	└────┴─┴──────┴───┴─┴────┴─┴──────┴───────────┴────────┴─┴──┴─┘

dyad ; 를 이용해 쉽게 boxed array를 만들 수 있습니다. 자세한 내용은 ;를 참고하세요.

		3 ; 9 ; 2 7
	┌─┬─┬───┐
	│3│9│2 7│
	└─┴─┴───┘

라이브러리 소스코드를 읽다보면 드물지 않게 boxopen과 boxxopen을 보실 수 있습니다. boxopen은 'box if open'으로 읽으세요. y가 open이거나 비어있을 때 boxing하고 이미 boxed되어 있으면 y를 돌려줍니다. 이런 방법으로 어떤 입력값 y가 항상 boxed noun임을 보증할 수 있습니다.

		boxopen 'open'
	┌────┐
	│open│
	└────┘
		boxopen ''
	┌┐
	││
	└┘
		boxopen <'boxed'
	┌─────┐
	│boxed│
	└─────┘
	
		boxxopen은 boxopen과 비슷하지만 한가지가 다릅니다. boxxopen은 비었을 때는 boxing하지 않습니다.
		
		boxxopen 'open'
	┌────┐
	│open│
	└────┘
		boxxopen '' NB. 비어있는 array를 돌여줍니다. boxopen ''과 대조해보세요.

###### 연관 primitive ######
L.(Level of) L:(Level at) {::(Map / Fetch) 과 같은 primitive가 boxed noun을 다룰 때 필요합니다. 
####= x < y ####=
Less Than dyad 0 0

monad < y 는 생소할 수도 있지만 dyad x < y 는 여러분들이 익숙한 바로 그 대소비교입니다. x < y 는 대소비교의 불린값(boolean value)을 전달합니다.

###### 정의 ######
x < y ⇔ 1, 만약 x < y 라면.
x < y ⇔ 0, 그밖의 경우.

대소를 비교할 때는 허용오차를 인정합니다. 허용오차는 =(Equal)을 참고하세요. !.(Fit)을 이용해 허용오차를 따로 정할 수 있습니다.

		t =: 2^_44
		1 < 1 + t	NB. 허용오차는 기본값(t =: 2^_44)
	0
		1 <!.0 ] 1 + t	NB. 허용오차를 더욱 엄격하게 적용합니다.
	1

#### > ####
####= > y ####=
Open monad 0
###### 정의 ######
boxed noun을 만드는 monad < 가 있다면 반대로 boxed noun을 여는 verb도 있어야 할 것입니다. monad > 가 monad <의 역함수입니다.(⇔ ><y is y.)
	
		< b. _1 NB. <의 역함수는?
	>

monad > 의 rank는 0입니다. 각 아이템에 대해 >를 적용한 다음 하나로 합칩니다. 합치는 과정에서 크기가 맞지 않으면 placeholder를 자동으로 삽입합니다. numeric array일 경우 0, string일 경우 ' '을 가장 크기가 큰 아이템에 맞게 삽입합니다.

		> 1 2 ; 3 4 5 ; 6 + i. 5
	1 2 0 0  0
	3 4 5 0  0
	6 7 8 9 10	
		> 'this' ; 'is' ; 'string.'
	this   
	is     
	string.
		$ > 'this' ; 'is' ; 'string.'
	3 7

혹시 y가 open일 경우는 어떻게 될까요? 이 경우 monad > 는  y에 영향을 끼치지 않습니다.

		original -: > original =: 'open'
	1

nested box일 경우는 한단계만 open됩니다. ">< y is y."라는 말과 정말로 부합합니다. 

		<<< 'I am in the box.'
	┌────────────────────┐
	│┌──────────────────┐│
	││┌────────────────┐││
	│││I am in the box.│││
	││└────────────────┘││
	│└──────────────────┘│
	└────────────────────┘
	
		> <<< 'I am in the box.'
	┌──────────────────┐
	│┌────────────────┐│
	││I am in the box.││
	│└────────────────┘│
	└──────────────────┘

open이 항상 성공하는 것은 아닙니다. box안 item의 type이 다를 경우 domain error메세지를 만날 수 있습니다.

		> (<'abc') , (<
		> 
		> `-:) , (<i. 2 3) , <<<'level 3 box' NB. 들어올 때는 마음대로였겠지만 나갈 때는 아니란다.
	|domain error
	|       >(<'abc'),(<+:`-:),(<i.2 3),<<<'level 3 box'
   
###### 쓰임새 ######
box로 보호가 끝난 noun을 꺼낼 때 사용하는 것이 기본입니다. 그 외에도 여러 용도가 가능하겠지요.

		g =: > 1 { (<'abc'),(<+`-),(<i.2 3),<<<'level 3 box'
		g/ 4 2 1 NB. 어떤 결과가 나올까요? (4 + 2 - 1)은 얼마인가요? 
	5

monad > 와 conjunction &. 가 만나면 쓸모가 많은 관용구 each가 됩니다. u each 는 각 아이템에 u를 적용하고 각각의 결과를 boxing해서 array로 만듭니다. monad u에서 쓸 수 있고 dyad u에도 적용가능합니다.

		each
	&.>
		+: each i. 5
	┌─┬─┬─┬─┬─┐
	│0│2│4│6│8│
	└─┴─┴─┴─┴─┘
		'xyz' , each _2 ]\ 'abcdef'
	┌──┬──┐
	│xa│xb│
	├──┼──┤
	│yc│yd│
	├──┼──┤
	│ze│zf│
	└──┴──┘
	
####= x > y ####=
Larger Than dyad 0 0

dyad < 와 비슷하게 x > y 조건에 대한 불린값을 전달합니다.
###### 정의 ######
x > y ⇔ 1, 만약 x < y 라면.
x > y ⇔ 0, 그밖의 경우.

대소를 비교할 때는 허용오차를 인정합니다. 허용오차는 =(Equal)을 참고하세요. !.(Fit)을 이용해 허용오차를 따로 정할 수 있습니다.

		t =: 2^_44
		(1 + t) > 1		NB. 허용오차는 기본값(t =: 2^_44)
	0
		(1 + t) (>!.0) 1	NB. 허용오차를 !.(Fit)을 이용해 더 엄격하게 적용합니다.
	1
#### <. ####
####= <. y ####=
Floor monad 0

간단한 기호의 역사를 살펴보겠습니다. 초창기 APL에서 Floor 기호는 └y┘ 였습니다. 그러다가 간결하게 └y로 사용되다가 J로 넘어와서 <. y 로 안착됩니다. APL 버전은 기호를 보기만 해도 무슨 뜻인지 알 것 같습니다. 저렇게 노골적인 기호를 보고 있으니 y의 바닥(Floor)을 얼른 찾아주고 싶습니다.

위키피디아에 따르면 가우스가 최초로 사용한 기호는 [y]였다고 힙니다. Iverson이 - 네 그 Iverson입니다 - └y┘를 고안항 이후 수학계에서도 이 기호를 사용한다고 합니다. 좋은 기호는 사람들이 알아보나 봅니다. (http://mathworld.wolfram.com/FloorFunction.html)
###### 정의 ######
<. y 는 y보다 작거나 같은 정수 중 가장 큰 수를 돌려줍니다. <.의 명칭(Floor)가 암시하듯 y의 바닥이 되는 정수를 반환하는 monad입니다.

		<. 2.4
	2
		<. _2.5		NB. 음수일 때는 주의를 기울어야 합니다.
	_3

TODO: y가 복소수인 경우 정수

###### 쓰임새 ######
가장 단순한 쓰임은 정수테스트입니다.

		(= <.) 2.7 4 3r2 _3.4	NB. 정수면 1 아니면 0
	0 1 0 0

반올림에 사용할 수 있습니다. 다른 프로그래밍 언어에서도 오랫동안 사용된 트릭입니다.
	
		<. 0.5 + 2.7 4 3r2 _3.4
	3 4 2 _3
	
###### 조금 더: complex floor ######
복소수일 경우 monad <. 는 복소수와 근접한 complex integer(혹은 Gaussian integer)를 구합니다. Gaussian integer는 실수부와 허수부가 모두 정수인 복소수를 말합니다. Gaussian integer가 아닌 임의의 복소수는 네 개의 근접한 Gaussian integer를 가집니다. 예를 들어 1.2j2.3은 1j2 2j2 1j3 2j3 이렇게 근접한 네 개의 Gaussian integer를 가집니다. 이 네가지 중 어느 것을 1.2j2.3의 complex floor로 정의해야 온당할까요?

여기에는 두가지 규칙이 적용됩니다. 첫번째는 실수부와 허수부 각각에 <.(Floor)를 적용하는 얻은 복소수와의 차이를 실수부와 허수부 각각 구하여 더한 합이 1이하인지 확인하는 것입니다. 예를 들어 1.2j1.3의 실수부와 허수부에 각각 <.(Floor)를 적용한 복소수는 1j1입니다. Gaussian integer 그리드 상에서 1.2j2.3의 왼쪽 하단에 위치한 수입니다. 이 수와 원래 수의 거리를 실수부와 허수부 각각 구해 더합니다. 0.2(실수부에서) + 0.3(허수부에서)< 1이니 1.2j1.3의 complex floor는 1j1입니다.


	1j2 __________ 2j2
	   |          |
	   |          |
	   |          |
	   | *1.2j2.3 |
	    ----------
	1j1            2j1

		 <. 1.2j1.3
	1j1

만일 이 "L-1 norm"이 1보다 크거나 같으면 두번째 규칙을 적용합니다. 예를 들어 1.4j1.7에 첫번째 규칙을 적용하면 0.4 + 0.7 >: 1입니다. 이 경우 두 개의 후보가 있습니다. 실수부만 <.(Floor)를 적용하고 허수부는 >.(Ceiling)을 적용한 1j2가 첫번째 후보입니다. 두번째 후보는 실수부에 >.(Ceiling)을 적용하고 허수부에는 <.(Floor)를 적용한 2j1입니다. 이 경우 실수부와 허수부를 각각 비교해 더 가까운 Gaussain integer를 complex floor로 삼습니다. 거리는 실수부와 허수부 비교합니다. 실수축을 따라 1.4j1.7와 1j2(후보2)의 실수축 거리는 0.6입니다. 허수축을 따라 1.4j1.7와 1j2(후보1)의 허수축 거리는 0.6입니다. 이 경우 후보1과의 거리가 더 가까우므로 1j2(후보1)을 택합니다.

	(후보1)
	1j2 __________ 2j2
	   |          |
	   |  *1.4j1.7|
	   |          |
	   |          |
	    ----------
	1j1            2j1(후보2)

		<. 1.4j1.7
	1j2
	
반대로 1.7j1.4의 경우  2j1(후보2)에 더 가까우니 2j1(후보2)를 complex floor로 삼습니다.

	(후보1)
	 1j2 __________ 2j2
	    |          |
	    |          |
	    |          |
	    |      *1.7j1.4
	     ----------
	 1j1            2j1(후보2)

		<. 1.7j1.4
	2j1


실제 정의에서는 더 '가까운'이 아니라 더 '먼'을 사용합니다. ip는  실수부와 허수부에 각각 <.(Floor)를 적용한 Gaussian integer(위의 예에서 왼쪽 아래)를 리스트로 표현합니다. fp는 ip에서 구한 Gaussian integer의 차이를 구하고, c1은 차이의 합이 1보다 작은지를 확인하며 c2는 실수부와 허수부의 차이를 비교해서 어느 것이 큰지 확인합니다. 그리고 위에 설명한 조건에 맞게 Gaussian integer를 반환합니다.

		 floor=: j./@(ip+(c2>c1),c1+:c2)
		'`c1 c2 fp ip'=:(1:>+/@fp)`(>:/@fp)`(+.-ip)`(<.@+.)

plot을 활용해 complex floor의 분포를 그려봅시다. 45도 기울어진 사각형 타일모양으로 구분되는 것을 관찰할 수 있습니다.
[[:image:]]

		require 'plot numeric'
		
		NB. 구분을 쉽게 하기 위한 색깔을 지정하는 과정입니다.
		NB. 아무리 생각해도 plot의 기본색상은 안 예쁩니다.
		COLORBREWER2_z_ =: 255 247 236,254 232 200,253 212 158,253 187 132,252 141 89,239 101 72,215 48 31,179 0 0,:127 0 0
		
		C=: , j./~ steps _2 2 40
		'dot; pensize 4 ; color COLORBREWER2;aspect 1' plot (]/.~ <.) C
	

###### 조금 더: 나머지 정리######
보통은 정수나 실수를 대상으로 작거나  같은 정수 중 가장 큰 수를 구하는데 사용되지만 사실 monad <.(Floor) 는 나누기 알고리즘에 중요한 역할을 합니다.

a를 b로 나눈 나머지(residue)를 r이라고 합시다. 나머지를 구하는 작업은 넓은 쓰임이 있어 J에서 따로 dyad  | 를 primitive차원에서 지원합니다. 나머지 r을 구하려면 r =: b | a 를 실행하세요.

		7 | 12 NB. 12를 7로 나눈 나머지
	5
	
몫(quotient)도 간단하게 구할 수 있습니다. 바로 몫을 찾을 때 <.(Floor)를 사용합니다. a를 b로 나눌 때 몫은 <. a % b 을 실행하면 찾을 수 있습니다.
	
		<. 12 % 7 NB. 12를 7로 나눈 몫
	1

몫과 나머지 그리고 제수(divisor)가 정해졌으니 이제 이것과 피제수(dividend)와의 관계를 a =  b × q  + r 로 다시 써볼 수 있습니다.
	
		12 = ((<. 12 % 7) * 7) + 7 | 12
	1

좌우변을 정리하여 나머지를 중심으로 다시 정리하면 r = a - b × q 이라는 결과를 얻을 수 있습니다. 한 편으로는 너무 당연해 보이는 식이고 다른 한편으로는 아무 의미 없어 보이는 반복인 것처럼 보이지만 이 식은 중요합니다. 왜냐하면 이제 이 식을 바탕으로 나머지(residue)를 복소수까지 확장하기 때문입니다. dyad |(Residue)는 이 정의를 바탕으로 합니다. 위 식의 우변을 J로 옮기면 'a - b * <. a % b'로 나타나 집니다.

		r =: 4 : 'y - x * <. y % x'
		7 r 12
	5
		1j2 r 2j3
	1j1

[[:TODO:]] 이거 맞는 거유? 해보면 다릅니다. 일단 넘어가고 나중에 하겠습니다.

		1j2 | 2j3
	0j_1
	
위의 정의를 사용하는데 한가지 고려사항이 있습니다. 제수(divisor) b가 0인 경우는 어떻게 다루어야 할까요? 12 mod 0 의 값은 얼마일까요? 아니 얼마면 될까요? (얼마까지 보고 오셨어요?) J는 a mod 0을 a로 정의합니다. 이런 고려까지 포함하려면 위의 user-defined dyad r을 약간 손봐야 합니다.

		r1 =: 4 : 'y - x * <. y % x + 0 = x'
		7 r1 12
	5
		1j2 r1 2j3
	1j1
	
####= x <. y ####=
Lesser of (min) dyad 0 0

dyad < 는 x 와 y의 대소관계의 논리값을 알려줄 뿐입니다. x와 y 중 작은 값이 무엇인지 알려주지는 않습니다. 그래서 준비했습니다. dyad <. 는 x와 y 중 작은 값입니다.
###### 정의 ######
numeric x와 y에 대해, x <. y는 x와 y중 작은 값입니다.

		1.27 <. 2
	1.27
		2j3 <. 2	NB. complex number에 대해서는 정의되어 있지 않고 domain error를 일으킵니다.
	|domain error
		'a' <. 'b'	NB. 문자열에 대해서도 정의되어 있지 않습니다.
	|domain error
	|   'a'    <.'b'

###### 쓰임새 ######
min함수는 일상에서 자주 사용됩니다. J스럽게 dyad <. 를 사용하려면 adverb /와 함께 사용하는 것이 좋습니다. array의 가장 작은 수를 구한다고 하면 아래의 표현을 사용할 수 있습니다.

		<./ ?. 10 $ 10
	4

array의 머리부터 꼬리까지 훑어가며 최소값을 구하려면 adverb /와 adverb \를 함께 사용합시다.

		<./\ 3 2 1 0 _1 0 1 2 _3
	3 2 1 0 _1 _1 _1 _1 _3
	
###### 조금 더 ######


#### <: ####
####= <: y ####=
Decrement monad 0

J의 primitive는 세심하게 정의되어 있습니다. 가까운 primitive는 모습도 비슷합니다. '.'와 ':'을 이용해 서로 가까운 관계임을 쉽게 알 수 있습니다. monad <: 도 이런 경우에 속합니다.  <: 는 <. 와 비슷하게 생겼고 그래서 밀접한 관련이 있습니다. 따로 기억하지 말고 함께 기억하는 것도 좋은 방법입니다. 
###### 정의 ######
<: y ⇔ y - 1 . y에서 1을 제합니다. complex number일 경우 실수부에서 1을 제합니다.

		<: 2
	1
		<: 1j1 
	0j1
	
###### 쓰임새 ######
갯수를 셀 때 하나를 덜거나 더하는 일은 빈번합니다. 이런 경우 dyad + 을 이용해 1을 더하거나 빼는 대신 monad >: 나 monad <: 를 사용하는 것을 고려해보세요. 코드가 짧아지고 작성하는 사람의 의도를 더 명확하게 할 수 있습니다. 물론 취향을 타는 부분일 수도 있겠습니다.
	
###### 연관 primitive ######
monad >: 는 monad <: 와는 반대로 1을 더합니다.

monad -. 는 1 - y를 돌려줍니다. 
####= x <: y ####=
Less Than or Equal

dyad < 가 수학기호 < 와 대응되는 것처럼 dyad <: 는 수학기호 ≤ 와 대응됩니다. dyad <: 는 '작거나 같은지'를 불린값으로 알려줍니다.  
###### 정의 ######
x <: y ⇔ 1, 만약 x ≤ y 라면.
x <: y ⇔ 0, 그밖의 경우.

대소관계를 비교할 때 허용오차가 있습니다. 기본 허용오차는 2^_44입니다. !.(Fit)을 이용해 허용오차를 조정할 수 있습니다.

		t =: 2^_44
		(1 + t) <: 1
	1
		(1 + t) (<:!.0) 1
	0
	
#### >. ####
####= >. y ####=
Ceiling monad 0

monad <. 에 대해 알고 있다면 monad >.를 파악하는 것은 쉬운 일입니다. monad >. 는 <.(Floor)와 대칭되는 primitive입니다. 

참고로 말씀드리면 초창기 APL에서 Ceiling 기호는 ┌x┐ 였습니다. ┌x┐ → ┌x → >.x . 정말 천장을 찾아주고 싶습니다.
###### 정의 ######
>. y 는 y보다 크거나 같은 정수 중 가장 작은 수를 돌려줍니다.
>
		>. 2.4
	3
		>. _2.5
	_2
		>. 2.4 _2.5
	3 _2

###### 조금 더 ######
y보다 크거나 같은 정수 중 가장 작은 수라는 정의는 왠지 친근합니다. 수학시간에 이런 정의를 들은 기억이 아련하게 떠오르지 않으신가요? 기억을 되살려 보니 limsup이 monad >. 와 비슷한 방식으로 정의됩니다. limsup과 liminf는 limsup -x_n ⇔ -liminf x_n 관계를 가지고 있습니다. 혹시 monad >. 와 monad <. 사이에도 이런 관계가 성립하지 않을까요?

		(>.&- = -@<.) 2.4 _2.5	NB. 나루호도(なるほど)!
	1 1

####= x >. y ####=
Larger Of (Max) dyad 0 0

dyad <. 가 있으면 >. 도 있어야 하는 법! Max라는 이름이 알려주듯 x와 y 중 큰 값을 돌려줍니다.
###### 정의 ######
x >. y ⇔ x, 만약 x ≥ y 라면.
x >. y ⇔ y, 그밖의 경우.

###### 쓰임새 ######
array 중 가장 큰 수를 구하려면 adverb / 와 함께 사용합시다. array를 훑어가며 최대값을 구하려면 adverb /와 adverb \ 를 연달아 사용하면 됩니다.

		>./ 4 5 9 8 9 3 5 0
	9
		>./\ 4 5 9 8 9 3 5 0
	4 5 9 9 9 9 9 9
	
#### >: ####
####= >: y ####=
Increment monad 0

monad <: 이 하나를 감한다면, monad >: 는 하나를 더합니다. 비슷한 기호를 사용하는 primitive는 서로 연관이 있다는 걸 다시 한 번 확인합니다.
###### 정의 ######
\>: y ⇔ y + 1 . y에서 1을 더합니다. complex number일 경우 실수부에서 1을 더합니다. >: 의 inverse는 <:입니다. y가 numeric value가 아니라면 에러가 발생합니다.

		>: 2
	3
		>: 1j1 
	2j1
		>: b. _1
	<:
		>: 'Can you increase me?'
	|domain error
	|       >:'Can you increase me?'

###### 쓰임새 ######
`i. 5` 는 0에서 4까지 정수 list를 만듭니다. 1에서 5까지의 정수 리스트를 만드려면 monad `>:` 를 활용하면 가능합니다. 홀수 리스트 만들기에도 활용할 수 있습니다.

		>: i. 5
	1 2 3 4 5
		>: +: i. 5	NB. 1부터 처음 5개 홀수
	1 3 5 7 9

####= x >: y ####=
Larger or Equal dyad 0 0

###### 정의 ######
`x >: y` ⇔ 1, 만약 x ≥ y 라면.
`x >: y` ⇔ 0, 그밖의 경우.

		0 >: i: 2
	1 1 1 0 0
		'a' >: 12	NB. numeric value가 아니면 domain error가 발생합니다.
	|domain error
	|   'a'    >:12

대소관계를 비교할 때 허용오차가 있습니다. 기본 허용오차는 `2^_44`입니다. `!.(Fit)`을 이용해 허용오차를 조정할 수 있습니다.

		t =: 2^_44
		1 >: 1 + t
	1
		1 (>:!.0) 1 + t
	0

#### + ####
##### + y #####
Conjugate monad 0

켤레복소수라는 말은 정말 멋진 용어인 것 같습니다. 신발을 셀 때나 사용할 법한 '켤레'라는 말을 복소수에 적용하다니 말입니다. monad `+`를 이용해 켤레복소수를 구할 수 있습니다.
###### 정의 ######
```+ y``` ⇔ `y`의 켤레복소수.

		+ 2j3 _j_2
	2j_3 _j2
		
		+ 3 NB. 정의대로 실수는 실수 자신입니다.
	3

###### complex number와 관련된 primitive ######
complex number에 관한 primitive가 처음 나왔으니 이번 기회에 complex number와 관련된 primitive를 정리하고 넘어가겠습니다.

		+ 1j2		NB. 켤레복소수를 반환합니다.
	1j_2
		+. 1j2		NB. 실수부와 허수부로 이루어진 리스트를 반환합니다.
	1 2
		9&o. 1j2	NB. 실수부를 구합니다.
	1
		11&o. 1j2	NB. 허수부를 구합니다.
	2
		 *. 1j2		NB. 크기와 각으로 이루어진 리스트를 반환합니다.
	2.23607 1.10715
		10&o. 1j2	NB. 크기(magnitude)를 구합니다.
	2.23607
		12&o. 1j2	NB. 각을 구합니다.
	1.10715

		require 'trig'
		dfr 12&o. 1j2	NB. radian을 각도로 바꾸면 더 쉽게 이해할 수 있습니다.
	63.4349
		_12&o. rad =: 12&o. 1j2 NB. _12&o. 는 12&o. 의 inverse로 radian으로 표현된 각도에 대응하는 unit complex number를 반환합니다.
	0.447214j0.894427
		| 1j2		NB. 크기(magnitude)를 구합니다.
	2.23607
		r. 12&o. 1j2	NB. (r. y) ⇔ (_12&o. y)
	0.447214j0.894427
		r./ *. 1j2	NB. x r. y ⇔ x * r. y 
	1j2
		* 1j2		NB. 복소수 y에 대해 y를 정규화(normalize)합니다.
	0.447214j0.894427
		j./ +. 1j2	NB. x j. y ⇔ x + j. y
	1j2
		1 + j. 2	NB. j. y ⇔ 0j1 * y
	1j2

##### x + y #####
Plus dyad 0 0

따로 설명할 필요가 없는 primitive입니다. 더하기, 빼기, 곱하기, 나누기는 기본 아니겠습니까? 

사칙연산(+ - * %)의 rank는 모두 0 0 0입니다. 연산은 개별값을 대상으로 이루어지는 것이니 rank 0 0 0은 자연스러운 선택입니다. 사실 <(Box)를 제외하고 연산과 관련된 primitive의 rank는 모두  0 0 0입니다. 구체적으로 말하면 <(monad 제외) <. <: > <. >: + +. +: * *. *: - % %: ^ ^. | ! ? ?. j. r. o. 의 rank는 모두  0 0 0입니다. 이들의 공통점은 모두 무언가를 연산하고 계산하는 value-based verb라는 점입니다. rank를 기준으로 primitive를 분류하는 방법은 J언어의 primitive에 대한 큰 그림을 그리는데 도움이 됩니다. 예외도 있지만 일단 무언가 계산하는 value-based(혹은 scalar-based) verb의 rank는 일단 0 0 0이라고 기억하고 예외만 따로 다루는 것도 방법입니다.
 
###### 정의 ######
x + y 는 기본연산의 더하기를 x와 y에 적용합니다. complex number는 실수와 허수부끼리 연산합니다.

		2 + 3
	5
		0j2 + 1j2
	1j4
		1 + 1j2
	2j2

###### 쓰임새 ######
숫자 리스트의 값을 모두 더하려면 dyad + 와 adverb / 을 사용합니다. +/\ 는 running sum을 구합니다.

		+/ >: i. 10
	55
		+/\ >: i. 10
	1 3 6 10 15 21 28 36 45 55

리스트가 불린값이라면 `+/`는 True값을 가지는 원소의 갯수입니다.
	
#### +. ####
##### +. y ####
Real / Imaginary monad 0

J는 복소수가 기본 type이 아닐까 할 정도로 복소수에 대한 지원이 풍부합니다. 복소수의 실수부와 허수부를 구하는 +. y 입니다. 
###### 정의 ######
복소수를 입력값으로 받아 실수부와 허수부로 이루어진 리스트를 돌려줍니다. 실수의 경우 허수부가 0인 복소수처럼 처리합니다.

		y = j./ +. y =: 1j2 NB. y ⇔ j/ +. y
	1
		+. 7 
	7 0

###### 관련 primitive ######
[[:complex number와 관련된 primitive:]]를 참고해주세요.

##### x +. y #####
GCD(Or) dyad 0 0

APL에서 gcd는 ∨ 로 표기되고 lcm은 ^ 로 나타나 집니다. 그런데 이 기호가 낯설지가 않습니다. 왜 논리학에서 쓰는 or과 and기호를 GCD와 LCM에 사용할까요? 어떻게 정의를 확장했는지 이번 기회에 알아봅시다.



###### 정의 ######
x +. y 는 x와 y사이의 최대공약수(Great Common Divisor)를 구합니다.
	
		12 +. 24
	12
		p: 2 4
	5 11
		+./ p: 2 4
	1
		0 +. 0
	0
		0 +. 1
	1
		1 +. 1
	1
		2 +. _108
	2
		1j2 +. 2j3
	1

###### 조금 더 ######
J언어에 대한 자료를 읽다보면 언어의 디자이너가 일관성을 지키기 위해 고심한 흔적을 자주 찾아볼 수 있습니다. GCD와 LCM도 그런 사례에 속합니다. J언어 디자이너는 논리값과 복소수를 대상으로 하는 연산에서도 일관성을 가지며 적용이 가능하도록 일반적인 GCD와 LCM의 정의를 확장합니다. 
(http://www.jsoftware.com/papers/eem/gcd.htm)

m = d x h, h는 정수일 때 m을 d의 배수(multiple)이라고 하며 반대로 d를 m의 약수(divisor)라고 합니다. 당연한 것 같지만 조금 더 살펴보면 재미있는 이야기가 많습니다.

d가 0인 경우는 어떤 일이 일어날까요? 만약 d = 0인 경우 h에 어떤 자연수가 오더라도  0 = 0 x h 관계가 성립합니다. 정의에 의하면 0(좌변)은 0(우변)의 배수이고, 0(우변)은 0(좌변)의 약수(divisor)입니다.

1) 0(좌변)은 0(우변)의 유일한 배수입니다. 0의 배수는 모두 0입니다.
2) 0(우변)은 0(좌변)의 약수이며 다른 수는 0을 약수로 가지지 않습니다.
3) 0는 모든 수의 배수입니다(h가 0일 경우를 생각하세요).
4) 0을 0으로 나눈 몫(quotient)은 정해진 값이 없습니다. (왜냐하면 0 = 0 x h는 모든 h값에 대해 성립하기 때문에  0 ÷ 0은 한가지 값으로 정할 수 없습니다.)

공약수(common divisor)

어떤 d가 두 숫자 h와 k 각각의 약수(divisor)일 때 d를 h와 k의 공약수라고 부릅니다. 이 공약수 중 가장 큰 수를 최대공약수 (GCD - Great Common Divisor)라고 부르겠습니다. 최대공약수를 구하는 효율적인 방법은 어떤 것이 있을까요? 모든 공약수를 구한 다음 그 중 가장 큰 수를 구하는 것은 자원낭비가 너무 큽니다. 어떻게 하면 빨리 최대공약수를 구할까요?

Euclidean Algorithm이 효율적으로 최대공약수를 찾는 방법을 제시해줍니다. 알려진 바에 따르면 이 알고리즘이 알고리즘이라고 부를 수 있는 것 중 가장 오래되었다고 합니다. 알고리즘은 다음과 같은 사실에 기반합니다.

만일 c = a | b라면 a와 b의 공약수(common divisor)는 c와도 공약수인 관계를 가집니다. 220과 284를 예로 살펴봅시다. 220과 284의 공약수는 1 2 4입니다. 284 mod 220은 64이며, 1 2 4는 모두 64의 약수입니다. 
	
		'a b' =: 220 284
   		] c=: a | b
	64
		divisors =: [: */\ 1: , ([ #~ e.)&:q:
		220 divisors 284
	1 2 4

		c = b - a * <. b % a

처음 이걸 읽고 난 후 기분이란 것이 아무 생각이 안 들고 마치 평양냉면을 처음 먹어본 것과 같은 기분이었습니다. 소가 목욕하고 나온 물을 마시는 것처럼 밍밍한 이 육수를 대체 왜 사람들이 좋아하는 것인지 의아함이 들었지만 또 자꾸 먹으니 나름으로 맛이더군요. Euclidean Algorithm도 마찬가지입니다. 한 번 적용하면 이걸 어디에 쓰냐는 의구심이 들지만 이 알고리즘의 진정한 가치는 반복에 있습니다. 평양냉면을 먹는 법을 '배운다'는 생각으로 반복해봅시다.
	
위의 예를 계속해서 사용합니다. 220 | 284의 나머지 연산값 64를 220과 나머지 연산합니다. 64와 220의 공약수는 28과도 공약수입니다. 220과 284의 공약수는 64와도 공약수입니다. 이 두가지를 합하면  그렇다면 28은 220과 284의 공약수의 배수입니다. 이런 과정을 반복해 나머지 연산의 결과가 0이 아닌 경우까지 반복하면 두 수의 최대 공약수를 구할 수 있습니다.

		284 | 220
	220
		220 | 284
	64
		64 | 220
	28
		28 | 64
	8
		8 | 28
	4
		4 | 8
	0

반복되는 패턴이 보이시나요? 패턴이 보이면 이제 verb로 정리할 시간입니다.

		gcd =: 4 : 'if. * y do. y gcd y | x else. x end.' 
	   	220 gcd 284
	4

0 1과 같은 불린값의 GCD는 얼마일까요? 1 = 1 x 1, 0 = 1 x 0로 고쳐 쓸 수 있으니 1이 GCD입니다.

		1 +. 0
	1

0 1의 LCM은 얼마일까요? 0 = 1 x 0, 0 = 0 x 0으로 표현할 수 있느니 0 1의 LCM은 0입니다.

		1 *. 0
	0

논리학에서 쓰는 or과 and기호가 어떻게 dyad +. 와 dyad *. 로 정착할 수 있었던 이유가 설명되었나요?
###### 쓰임새 ######
dyad +. 는 논리연산에서 or 연산자와 동일합니다.

		+. table 0 1 NB. truth table
	┌──┬───┐
	│+.│0 1│
	├──┼───┤
	│0 │0 1│
	│1 │1 1│
	└──┴───┘

#### +: ####
##### +: y #####
Double monad 0

어떤 값의 배수를 구하는 연산은 빈번하게 일어납니다. 그래서 J는 따로 monad +:를 primitive 차원에서 지원합니다.
###### 정의 ######
+: y ⇔ y + y . dyad + 와 monad +: 가 어떤 관련이 있는지 잘 나타납니다.

		+: _2 0 2
	_4 0 4
	
###### 쓰임새 ######
monad +: 와 monad *: 는 tacit 표현을 사용할 때 편리합니다. dyad \* 를 사용해도 가능하지만 monad +:를 사용하면 명시적으로 지정할 수 있기 때문에 사용하지 않을 특별한 이유가 없다면 항상 기본으로 사용하는 것을 권합니다. 
		
		13 : '2 * y'
	2 * ]
		13 : '+: y'
	+:
	
##### x +: y #####
Not-Or dyad 0 0

Not-Or 논리연산자입니다. dyad +. 와 연관이 있는 primitive라는 사실을 닮은 기호를 통해 알 수 있습니다.
###### 정의 ######
`x +: y` ⇔ `-. x +. y` .

		+: table 0 1 NB. truth table
	┌──┬───┐
	│+:│0 1│
	├──┼───┤
	│0 │1 0│
	│1 │0 0│
	└──┴───┘
		
#### * ####
####= monad * ####=
Signum monad 0

어떤 수가 있을 때 간단하게 부호만 알고 싶은 경우가 있습니다. 0보다 작은지 큰지, 아니면 0인지를 알고 싶은 경우가 자주 있습니다. monad * 는 이런 연산을 간단하게 다룰 수 있게 합니다.

Eugene E. McDonnell이라는 이름을 기억합시다. Signum(APL: ×, J: *)과 circular function(APL: ○, J: o.)기호를 제안했습니다. J관련 문서를 읽다보면 항상 보게 될 정겨운 이름입니다. Vector에 연재된 At Play With J는 많은 Jer들에게 통찰과 즐거움을 주는 좋은 글입니다. 그가 제안한 * 기호를 다루며 그에게 큰 고마움을 표합니다. 
###### 정의 ######
* y ⇔ 1, 만약 y > 0 라면.
* y ⇔ 0, 만약 y = 0 라면.
* y ⇔ _1, 만약 y < 0 라면.

y를 복소수까지 확장한 일반적인 경우라면
* y ⇔ 복소수 y에 대해 y를 정규화(normalize)한 값 ⇔ 복소평면에서 원점과 y를 잇는 선과 단위원의 교차점 

		 * _2 _1 0 1 2 1j2
	_1 _1 0 1 1 0.447214j0.894427

대소를 비교할 때는 허용오차를 인정합니다. 허용오차는 Fit(!.)이용해 재지정할 수 있습니다.

		t =: 2^_45
		* 0 + t
	0
		(*!.0) 0 + t
	1

###### 쓰임새 ######
마이너스 인덱스를 사용하면 monad * 를 좀 더 자유롭게 사용할 수 있습니다. _1 인덱스는 array의 가장 마지막 item을 지칭합니다.

		sign =: ;: 'zero plus minus'
		(* _1 1 0 2 _2 0) { sign
	┌─────┬────┬────┬────┬─────┬────┐
	│minus│plus│zero│plus│minus│zero│
	└─────┴────┴────┴────┴─────┴────┘

monad * 와 conjunction @. 은 궁합이 잘 맞습니다. 예를 들어 음수는 x2, 짝수는 ÷2 그리고 0은 1을 더하는 verb를 만들어봅시다. 조건문을 만들고 싶어 근질거리는 손을 잠깐 멈추고 conjunction @. 을 사용해 해결해봅시다. 일단 익숙해지면 이쪽이 더 편하고 읽기도 쉽습니다. 혹시 gerund나 conjunction @.(Agenda)를 아직 사용해보지 않으셨다면 이번 기회에 익혀보세요.

		f =: >:`-:`+:@.* NB. 마이너스 인덱스를 사용하기 위해 음수는 가장 마지막에 위치시킵니다.
		f _1 0 1
	_2 1 0.5

##### x * y #####
Times dyad 0 0

여러분이 짐작하신 곱셈기호 바로 그것입니다. APL에서는 × 기호를 따로 두었지만 J에서는 ASCII 문자 중 *를 사용합니다.
###### 정의 ######
x + y 는 기본연산의 곱하기를 x와 y에 적용합니다. complex number인 경우 통상적인 복소수 곱셈의 정의(ajb × cjd = (ac-bd)j(bc+ad)) 를 따릅니다.
 
		2 * 3
	6
		1j2 * 3j4
	_5j10

###### 조금 더 ######
복소수를 다룰 때는 실수를 다룰 때보다 약간의 에너지가 더 들어갑니다. 아무래도 일상에서 접할 일이 없는 수라는 점과 직관적이지 않은 연산규칙 등이 복소수를 다루는 것을 어색하게 만듭니다. 복소수는 다루기 까탈스럽게 보입니다. 그렇지만 공대생들에게 복소수는 친구와 같습니다. 특히 전자/전기/통신 쪽이 전공이라면 실수따위는 허수부가 0인 복소수에 불과한 것이지요.

J는 복소수를 다루는데 오버헤드가 거의 없습니다. 라이브러리를 사용할 필요도 없고 primitive 차원에서 복소수를 지원하기 합니다. 대부분의 rank-0 verb는 복소수까지 verb의 정의를 확장시켜 놓았습니다. 예를 들어 monad <.(Floor) 나 monad >.(Ceiling)의 경우 complex floor라는 문헌(http://www.jsoftware.com/papers/eem/complexfloor1.htm)에서 어떻게 복소수 차원까지 정의를 확장했는지를 상세하게 설명하고 있습니다.


#### *. ####
##### *. y #####
Length / Angle monad 0

복소수를 위한 monad입니다. complex number를 받아 polar form으로 표현할 때의 magnitude와 angle을 2-element list로 반환합니다.
###### 정의 ######
*. y 는 complex number y의 magnitude와 angle(in radians)값을 list로 반환합니다.

		*. 1j2
	2.23607 1.10715
		| 1j2 NB. magnitude
	2.23607
		10&o. 1j2 NB. magnitude
	2.23607
		12&o. 1j2 NB. angle
	1.10715
 
##### x *. y #####
LCM(And) dyad 0 0

GCD가 있으면 LCM(Least Common Multiple)도 있어야 합니다. 입력이 불린값인 경우 logical operator ^ 과 동일합니다.
###### 정의 ######
x *. y 는 x와 y의 최소공배수를 구합니다. 일반적인 정수에 더해 분수, 불린값 그리고 복소수까지 정의가 확장되어 있습니다.

		12 *. 35
	420
		1r2 *. 2r3
	2
		0 *. 0
	0
		0 *. 1
	0
		1 *. 1
	1
		1j2 *. 2j3
	_4j7
	
###### 쓰임새 ######
dyad *. 는 논리연산에서 and 연산자와 동일합니다.

		*. table values
	┌──┬───┐
	│*.│0 1│
	├──┼───┤
	│0 │0 0│
	│1 │0 1│
	└──┴───┘
	
#### *: ####
####= *: y ####=
Square monad 0

monad +: 와 유사합니다. monad +: 가 y + y를 구하는 반면, monad *: 는 y × y를 구합니다.
###### 정의 ######
> *: y ⇔ y * y .

		(*:`(*~)`:0) 0.7
	0.49 0.49

###### 쓰임새 ######
monad *: 는 tacit 표현을 사용할 때 편리합니다. 예를 들어 \\(x^2 + y^2\\)를 함수로 만들 때 *:을 사용하면 쉽게 만들 수 있습니다.

		require 'plot'
		f =: -&*:
		'noaxes' plot _2 2 ; _2 2 ; 'f'
		
![](http://i.imgur.com/SG1Q1Tw.png)
		
##### x *: y #####
Not-And dyad 0 0

논리연산자입니다. NOT(X AND Y)값을 구합니다.
###### 정의 ######
x *: y ⇔ -. x *. y .

		*: table 0 1 NB. truth table
	┌──┬───┐
	│*:│0 1│
	├──┼───┤
	│0 │1 1│
	│1 │1 0│
	└──┴───┘
	
#### - ####
##### - y #####
Negate monad 0

J를 처음 접하신 분들을 당혹게 하는 것이 한두 가지겠습니까마는 음수를 _(underbar)로 표기하는 것은 다른 프로그래밍 언어에서 보기 힘든 일이니만큼 몹시 당혹스럽습니다. 이런 표기법은 APL의 전통으로 APL의 후손들도 각자 음수와 negate를 구분합니다. APL의 경우 음수를 ¯(high minus)로 표현합니다. 이 전통을 이어 J에서는 음수를 _(under bar)로 표기하고 K언어에서는 띄어쓰기로 음수와 negate를 구분합니다. 예를 들어 -3은 음수이지만, - 3은 negate 3 로 구분됩니다. 

[source](http://archive.vector.org.uk/art10004040)

처음에는 생소하지만 쓰다보면 이런 표기법이 편리합니다. 표기의 일관성을 보장해주고 monad - 를 활용할 수 있게 한다는 것 등 편리한 점이 많습니다. 일상 생활에 사용해도 좋겠습니다.

###### 정의 ######
\- y ⇔ 0 - y .

		- _1 0 1
	1 0 _1
		 - b. _1 NB. monad - 의 inverse는 - 자신입니다.
	-
	
##### x - y #####
Minus dyad 0 0

기본연산 -(빼기) 를 수행합니다.
###### 정의 ######
x - y ⇔  \\(x - y \\) 값을 구합니다. 실수와 복소수까지 정의되어 있습니다.

		 1j2 2j3 0 _1r3 _ - 2j3 0 _1r3 _ 1j2
	_1j_1 2j3 0.333333 __ _j_2

###### 쓰임새 ######
dyad + * 의 경우 right operand와 left operand를 교환해도 결과에 차이가 없습니다. 하지만 dyad % - 의 경우 operand가 바뀌면 결과가 달라집니다. 이런 이유로 operand를 바꾸는 adverb ~ 와 함께 쓰이는 경우가 많습니다. 예를 들어 리스트 원소 간의 차분(difference)을 구할 때는 -~/\ 를 거의 관용구처럼 한 묶음으로 자주 사용합니다.

매년 신생아의 수가 매년 얼마나 늘고 주는지 알아봅시다. 각 연도를 비교해 차분(difference)를 구합시다.
	
		require 'plot'
		
		NB. 1981년부터 2015(p)년 사이 매년 태어난 신생아 
		new_babies =: 867409 848312 769155 674793 655489 636019 623831 633092 639431 649738 709275 730678 715826 721185 715020 691226 668344 634790 614233 634501 554895 492111 490543 472761 435031 448153 493189 465892 444849 470171 471265 484550 436455 435435 438700
		
		diff =: 2 -~/\ new_babies
		COLORBREWER_z_ =: 103   0  31 , 5 48 97
		pd 'new'
		pd 'type bar'
		pd 'color COLORBREWER'
		pd diff
		pd 'show'
	
[[:TODO:]] xlabel 공백문제 처리하는 요령

[source](http://kostat.go.kr/portal/korea/kor_nw/2/2/1/index.board?bmode=download&bSeq=&aSeq=351602&ord=1)
	
#### -. ####
##### -. y #####
Not monad 0

기억을 떠올려 봅시다. - y 는 0 - y 와 동일합니다. monad - 와 점 하나 차이로 다른 monad -. 는 당연히 monad - 와 비슷한 연산을 합니다. -. y 는 1 - y 를 구합니다. 따로 외우려 하지 말고 monad - 와의 유사점을 연상해 기억합시다.
###### 정의 ######
\-. y ⇔ 1 - y .
		
		-. 0 1
	1 0
		-. 0.1 0.2 0.9 1.5
	0.9 0.8 0.1 _0.5

###### 쓰임새 ######
첫번째로 primitive의 명칭이 의미하듯 불린값에 대해 not 연산을 수행합니다. J는 불린값 리스트를 적극적으로 활용하는 편이라 -. 를 사용하고 접할 기회가 자주 있습니다.
	
		numbers =. ?. 10 $ 50
		
		NB. 3의 배수가 아닌 수를 고르시오 - 변수를 사용한 경우
		numbers #~ filter =. -. 0 = 3&| numbers
	44 8 35 16 46 26
	
		NB. 3의 배수가 아닌 수를 고르시오 - 변수를 사용하지 않은 경우
		(#~ 0 -.@= 3&|) numbers
	44 8 35 16 46 26
	
		NB. 3의 배수가 아닌 수를 고르시오 - ~:(not equal)을 사용하는 경우
		NB. 이 경우가 가장 깔끔하기는 하지만 정신줄을 놓고 프로그래밍을 하다보면
		NB. ~: 이 생각나지 않을 때도 있는 거니까요
		(#~ 0 ~: 3&|) numbers

두번째로 complementary probability 를 구할 때 요긴하게 쓰입니다. 어떤 사건 E가 일어날 확률이 \\(P(E)\\)라고 하면 이사건의 여집합 \\(E^c\\)의 확률 \\(P(E^C)\\)는 \\(1 - P(E)\\)입니다. 예를 들어 매해 어떤 사람이 사망할 확률이 q라고 하면 이 사람이 연말까지 살아있을 확률은 -. q 입니다. 실제 예를 들어볼까요?

아래의 숫자는 통계청 발표 2014년 남녀 전체 완전생명표의 생존자 수를 따온 자료입니다. 이 자료를 바탕으로 각 나이별 기대여명을 계산해봅시다.

		l =: 100000 99700 99676 99659 99647 99638 99628 99619 99610 99602 99594 99587 99580 99573 99563 99550 99531 99508 99481 99453 99424 99393 99362 99329 99294 99257 99218 99176 99131 99083 99030 98973 98912 98850 98784 98714 98639 98560 98473 98380 98279 98172 98057 97933 97797 97648 97484 97305 97110 96898 96666 96414 96139 95843 95525 95188 94831 94451 94046 93614 93154 92662 92133 91565 90952 90288 89569 88792 87954 87045 86040 84915 83653 82248 80699 79000 77134 75080 72816 70333 67616 64672 61507 58120 54524 50755 46837 42813 38732 34649 30622 26712 22976 19468 16233 13307 10713 8463 6553 4970 3688
		
		q =: l %~ 2 -/\ l , 0
		
[source](http://kostat.go.kr/portal/korea/kor_nw/2/2/7/index.board?bmode=download&bSeq=&aSeq=350178&ord=2)

##### x -. y #####
#### -: ####
####= -: y ####=
####= x -: y ####=

#### % ####
####= % y ####=
####= x % y ####=
###### 쓰임새 ####=
adverb ~ 와 자주 같이 쓰인다

#### %. ####
##### %. y #####
##### x %. y #####
###### 쓰임새 ######
regression
그리고 roger hui의 코드, project euler에서

#### %: ####
##### %: y #####
##### x %: y #####





#### ! ####
##### ! y #####
Factorial monad 0

위키피디어에 factorial을 검색하면 문화어로 차례곱이라고 한다는 설명이 나옵니다. 문화어가 무엇이지 몰라서 클릭해보니 북한표준어입니다. 위키피디어에 문화어 덕후가 있는 것이 틀림없습니다.

그것과는 별개로 차례곱이라는 말은 이해하기 쉬운 좋은 용어라고 생각합니다.
###### 정의 ######
`! y`는 1에서 y까지의 자연수를 차례로 곱한 값입니다.

* `y`가 음수가 아닌 자연수일 때: `! y` ⇔ `*/ >: i. y`.
		
			! 3 5
		6 120
			NB. 0! 는 1입니다.			
			! 0
		1

* 일반적인 `y`일 때: `! y` ⇔ `Γ(1 + y)`. Γ()는 Gamma Function을 의미합니다. Gamma function은 확률이나 통계에서 자주 볼 수 있습니다. 실수부가 0보다 클 때 복소수 x에 대해 Gamma function의 정의는 \\(\Gamma(t) = \int_{0}^\infty x^{t-1} e^{-x} \, dx \\) 입니다. 이 함수는 negative integer를 제외하고 모두 
			
			NB. 실수 y에 대한 Gamma function
			NB. infinity와 negative infinity를
			NB. 방지하기 위해 (_10, 10) 구간으로
			NB. 제한합니다.
			require 'plot numeric'
			plot domain ; _10 >. 10 <. !@<: domain =: steps _4 4 100

![](http://i.imgur.com/qEPYwwe.png)

`!^:_1`이 정의되어 있습니다. `!^:_1 y`의 결과 `n`은 `y -: ! n`을 만족합니다.

		! 3
	6
		!^:_1 ] 6
	3

###### 쓰임새 ######
Gamma function은 통계 특히 Bayesian statistics에서 약방의 감초처럼 사용됩니다. monad `!`을 확장해 Gamma function으로 사용할 수 있다는 것을 기억하세요.	

Journal of J(앞으로 JoJ라고 지칭합니다)를 아십니까? [Journal of J](http://www.journalofj.com/) J언어와 과학분야의 활용에 대한 온라인 저널입니다. J언어를 다른 사람들이 어떻게 활용하는지 영감을 얻을 수 있는 곳입니다.

JoJ의 표지에 예쁜 그래프가 그려져 있습니다. Gamma function의 surface graph입니다. 이 그래프를 `plot`패키지를 이용해 그려봅시다.
		
		require 'plot numeric'
		gamma =: !@<:
		real =: {.@+.
		x =: steps _3.5 4.5 40
		y =: steps _1 1 40
		z =: real gamma x j./ y

		NB. (_3 , 12) 사이로 제한합니다
		dat =: _3 >. 12 <. z
		NB. default viewpoint 1.6 _2.4 1.5
		'surface;noaxes' plot x ; y ; dat

![](http://i.imgur.com/QhlKLm9.png)
		
		'surface;noaxes;viewpoint _2 _2 1' plot x ; y ; dat

![](http://i.imgur.com/4MMXt5R.png)

##### x ! y #####


#### ? ####
##### ? y #####
Roll monad 0

이름에서 연상할 수 있듯 monad `?`는 주사위를 굴려 숫자를 뽑는 것과 비슷한 일을 합니다. 어떤 범위에서 숫자를 선택할 지 입력값으로 지정합니다.

`? y`는 y로 지정된 범위에서 uniformly distributed된 임의의 값을 뽑습니다.

* `y`가 `0`일 때: (0, 1) 사이 구간에서 임의의 실수를 뽑습니다.

			NB. uniform distribution에서 두 개의 수를 뽑습니다.
			?  0 0
		0.622471 0.324707

			NB. histogram을 그립니다.
			] bins =: }. (% #) i. 10
		0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9

			histogram =: <: @ (#/.~) @ (i.@#@[ , I.)
			'type bar' plot bins histogram ? 1000 $ 0

![](http://i.imgur.com/5Z6ij4D.png)

* `y`가 양의 정수일 때: `i. y` 중의 값 하나를 선택합니다.
			
			NB. 정의에 의하면 y가 1일 때는
			NB. 언제나 0을 반환합니다.
			? 1
		0
			? >: i. 5
		0 0 0 2 2
			NB. histogram으로 확인합니다.
			0 1 2 3 4 histogram ? 1000 $ 5
		205 197 210 179 209
			
			NB. matrix도 가능합니다.
			] m =: ? 3 3 $ 10
		3 4 8
		1 8 0  
		6 3 7

###### 쓰임새 ######
index로 활용하기
boolean list만들기

###### 관련 foreign ######

##### x ? y #####
Deal dyad 0 0

순열(permutation)을 구하는 verb입니다. 사전에서 설명하기를 deal은 '카드를 나누다/돌리다'는 뜻이 있다고 합니다. 사전의 뜻과 dyad `?`을 연관지어 기억합시다.[옥스포드 사전: deal](http://www.oxforddictionaries.com/definition/english/deal)

###### 정의 ######
`x ? y`는 `i. y` 중 x개를 중복없이 선택해 list로 만듭니다. `x`와 `y`가 같은 수일 경우 `i. y`의 임의의 순열(permutation) 하나를 구하는 것과 같습니다. 

		6 ? 9
	8 7 5 0 4 6
		6 ? 6
	4 2 5 1 3 0

###### 쓰임새 ######
permutation과 관련된 연산에 사용됩니다. 임의의 permutation이 필요하다면 dyad `?`와 adverb `~`을 함께 사용해 봅시다.

		?~ 10
	0 2 9 5 8 3 7 1 4 6


카드이야기기 나왔으니 카드를 안 건드리고 갈 수가 없습니다.
카드 한 벌은 4개의 무늬(suit)와 13개의 숫자(denomination)으로 구성되어 있습니다.

		] s =: 4 u: 9827 9830 9829 9824
	♣♦♥♠
		] d =: 'A23456789TJQK'
	A23456789TJQK
		pack =: , { d ; s
		NB. 다섯 장의 카드를 뽑아봅시다.
		> (5 ? 52) { pack
	3♥
	J♠
	6♣
	3♠
	K♠
   
#### ?. ####
##### ?. y #####
Roll / fixed seed monad _

monad `?`와 비슷하지만 fixed random seed와 fixed random number generator(RNG)를 이용합니다.
###### 정의 ######
`?. y`는 `? y` 와 같은 규칙을 따르며 임의의 수를 생성합니다. 고정된 시드값을 가지고 있어 같은 입력에 대해 항상 같은 임의의 수를 생성합니다.
		
		? 0 2 6
	0.0759929 1 0
   		? 0 2 6
	0.582562 1 1
   		?. 0 2 6
	0.038363 0 4
   		?. 0 2 6
	0.038363 0 4

###### 쓰임새 ######
monad `?/`를 사용해 같은 임의의 수를 반복해서 생성할 수 있습니다. 예제를 실행했을 때 같은 결과가 나올 수 있도록 이 책은 가능하다면 의도적으로 `?`대신 `?.`을 사용하고 있습니다.

###### 조금 더 ######
monad `?.`의 verb rank는 `_`(infinity)입니다. 이는 `?.`가 실행될 때마다 RNG(Random Number Generator)가 초기화되기 때문입니다. 만약 rank가 0이라면 한 atom에 대해 실행한 이후 매번 초기화가 되어 모든 임의의 수가 동일하게 됩니다.

		?. 10 $ 6
	0 2 4 0 5 4 4 4 2 1
		?."0 (10 $ 6)
	0 0 0 0 0 0 0 0 0 0

[monad ?. 의 verb rank가 _인 이유](http://code.jsoftware.com/wiki/Vocabulary/querydot)
##### x ?. y #####
Deal / fixed seed dyad 0 0

dyad `?`와 비슷하지만 fixed random seed와 fixed random number generator(RNG)를 이용합니다.
###### 정의 ######
`x ?. y`는 `x ? y` 와 같은 규칙을 따르며 순열을 생성합니다. 고정된 시드값을 가지고 있어 같은 입력에 대해 항상 같은 임의의 수를 생성합니다.

		10 ? 10
	1 7 6 3 2 9 5 8 0 4
		10 ? 10
	1 0 2 9 8 4 6 3 5 7

		NB. 반복된 실행에도 결과가 동일합니다.
		10 ?. 10
	4 3 6 2 9 8 5 1 7 0
		10 ?. 10
	4 3 6 2 9 8 5 1 7 0	
[[:TODO:]]
![](http://i.imgur.com/A4QUvX3.jpg)
#### j. ####

##### j. y #####
Imaginary monad 0

복소수를 나타낼 때 J언어는 j-notation 을 따릅니다. `XjY`에서 j문자를 기준으로 삼아 X는 실수부, Y는 허수부를 가리킵니다. j-notation을 확장해 monad `j.`를 정의합니다.
###### 정의 ######
`j. y` ⇔ `0j1 * y`. `y`에 단위허수 `0j1`을 곱한 결과를 구합니다.

		j. 1j2
	_2j1
		j. 3
	0j3

###### 쓰임새 ######
어떤 수를 복소수로 만들 때 사용합니다. 아미도 가장 유명한 예는 오일러의 공식이 아닐까합니다.
	
		require 'plot numeric'
		NB. e^ix = cosx + i sinx
		'type marker; aspect 1' plot ^@j. steps 0 2p1 10

![](http://i.imgur.com/8BtLFG3.png)

복소평면에서 어떤 수에 단위허수 i를 곱하는 것은 그 수를 원점을 기준으로 시계반대방향으로 90도 회전을 시키는 것과 같습니다.

		12&o. 1j2 
	1.10715
		12&o. j. 1j2
	2.67795
		require 'trig plot'
		NB. Degrees From Radians
		dfr (-~&(12&o.) j.) 1j2
	90
 		'key original, turned ; axes 0 0' plot (,: j.) (0 1j2)

![](http://i.imgur.com/OJEpwJa.png)
##### x j. y #####
Complex dyad 0 0

실수부와 허수부를 조합해 복소수 하나를 만듭니다.
###### 정의 ######
`x j. y` 는 실수부 x와 허수부 y를 결합해 복소수 `xjy`를 만듭니다. `x j. y` ⇔ `x + j. y`.

		1 j. 2
	1j2
		j./~ i. 3 
	0 0j1 0j2
	1 1j1 1j2
	2 2j1 2j2

###### 관련 primitive ######
monad `+.`는 복소수를 실수와 허수부 list로 만듭니다.

		+. 1 j. 2
	1 2


#### r. ####
##### r. y #####
Angle monad 0

radian값을 받아 복소평면 위의 복소수에 대응시키는 verb입니다.
###### 정의 ######
`r. y` ⇔ `^ j. y` 관계를 기지고 있습니다. radian값을 받이 이에 대응되는 단위 복소수를  unit circle위의 점에 대응시킵니다.

		
		require 'plot trig numeric'
		(cos j. sin) 0.25p1
	0.707107j0.707107
		r. 0.25p1
	0.707107j0.707107
		r. 0.25p1
	0.707107j0.707107
		'type marker;aspect 1' plot r. steps 0 2p1 20 

![](http://i.imgur.com/XOzbID4.png)

		
##### x r. y #####
Polar dyad 0 0

monad r. 을 확장해 dyad r.은 x가 magnitude를 지정할 수 있게 합니다.

###### 정의 ######
`x r. y` ⇔ `x * r. y`로 정의되며 `x`는 magnitude `y`는 radian 값으로 이에 해당하는 복소평면의 복소수를 구합니다.

		require 'trig'
		2 r. 0.25p1
	1.41421j1.41421
		
###### 관련 primitive ######
monad `*.`는 복소수의 magnitude와 angle을 list로 만들어 줍니다.

		*. 2 r. 0.25p1
	2 0.785398
		dfr {: *. 2 r. 0.25p1
	45
		
#### ^ ####
##### ^ y #####
Exponential monad 0

^(caret)기호는 여러 프로그래밍 언어에서 거듭제곱 기호로 사용되고 있습니다. TeX에서도 거듭제곱 기호로 사용되고 있어 이런 용도로 사용되는 것을 볼 일이 드물지 않습니다.

따로 밑(base)을 정하지 않고 monad로 사용되는 경우 자연로그의 거듭제곱을 돌려줍니다.
###### 정의 ######
^ y ⇔ e ^ y (e는 자연상수입니다). monad ^ 은 monad ^. 의 inverse verb입니다. 

		^ 1 NB. 자연상수
	2.71828
		^ b. _1
	^.
		^ ^. 2 
	2

###### 쓰임새 ######
monad ^ 쓰임새 중 가장 유명한 것은 Euler's formula(e^ix = cos(x) + i*sin(x))이겠지요. 대체 아무런 상관없어 보이는 지수함수와 사인/코사인 함수사이에 그렇게 놀라운 관계가 있을 수 있을까요? ^@j. 1p1가 얼마안지 확인해보고,  0에서 2π까지의 값을 입력받아 평면에 점을 찍어봅니다.
 
		require 'plot numeric'
		f =: ^@j. NB. monad j. 를 활용
		x: f  1p1 NB. x: 가 없으면 _1에 아주 가까운 수로 표현됩니다
	_1
		'dot ; pensize 4 ; aspect 1' plot f steps 0 2p1 20

[[:plot:]]
####= x ^ y ####= 
Power dyad 0 0

dyad ^ 을 이용해 지수(exponent)뿐 아니라 밑(base)까지 지정해 거듭제곱을 표현할 수 있습니다.
###### 정의 ######
x ^ y 는 x의 거듭제곱을 반환합니다. 일반적인 정의는 x ^ y ⇔ ^y*^.x \(e ^ (y lnx} \) 를 따릅니다. 정수와 실수에 더해 복소수까지 정의되어 있습니다.

		2 ^ 3
	8
		require 'trig plot'
		w =: ^ j. rfd 60
		w ^ i. 7
	1 0.5j0.866025 _0.5j0.866025 _1j3.88578e_16 _0.5j_0.866025 0.5j_0.866025 1j_8.32667e_16
		'dot;pensize 6; aspect 1' plot w ^ i. 7

[[:plot:]]

non-negative 정수 y에 대해 x ^ y 는 */ y # x 와 동일합니다.

		(2 ^ 3) -: (*/ 3 # 2)
	1

몇가지 값들은 따로 정의되어 있습니다.

		 0 ^ 0
	1
		_ ^ 0
	1
		1 ^ _
	1

####= x ^!.p y ####=
Stope Function dyad 0 0

stope은 석탄이나 금을 캐는 채굴장을 말합니다. 곱해지는 인수가 마치 채굴장의 계단처럼 올라가거나 내려가서 이런 이름을 얻었습니다.(Concrete Mathematics Companion)
###### 정의 ######
x ^!.p y ⇔ */ x + p * i. y ⇔ x * (x + p) * (x + 2p) * ... * (x + (y-1)p).

		 4 ^!.1 (3)
	120
		4 * 5 * 6
	120
	
###### 쓰임새 ######
dyad ^!._1 은 따로 falling factorial이라는 이름을 가지고 있습니다. 왜 이런 이름을 가지고 있는지 예제를 통해 확인해 봅시다. falling factorial은 permutation을 밀접한 연관을 가지고 있습니다.

		5 ^!._1 (3)
	60
		5 * 4 * 3
	60	
		 10 ^!._1 (4) NB. 10개 중 4개를 순서를 따지며 선택하는 경우의 수는?
	5040
		10 * 9 * 8 * 7
	5040

( http://www.jsoftware.com/help/dictionary/samp28.htm)


dyad ^!.p 는 확률계산에도 쓰임이 많습니다. 특히 계리(Actuarial Science)하는 분들이 좋아할 만한 기능을 제공합니다. 생존확률(survival probability)라고 불리고 업계에서는 보통 소문자 n_p_x로 표기됩니다. 현재 x살인 사람이 n년 후까지 살아있을 확률을 말합니다. 간단하게 p라고 사용하기도 합니다. 이제 생존확률계싼에 stope function을 이용해 봅시다.

어떤 사람이 올해 안에 사망할 확률이 q라고 하면 연말까지 살아 있을 확률은 p =: -. q 입니다. 업계에 따라 이를 force of mortality, failure probability 혹은 harzard rate이라고 다르게 부릅니다. 간단하게 말해 올해 초까지 살아있었다면 연말 전에 사망할 확률을 말합니다. 예를 들어 각 연도별로 연중에 사망할 확률이 x로 동일하다면 y년 후에 살아있을 확률은 x (-.@[ ^ ]) y 입니다.

		 0.1 (-.@[ ^ ]) i. 6 NB. y년 경과 후 살아있을 확률
	1 0.9 0.81 0.729 0.6561 0.59049

하지만 상수 force of mortality는 너무 단순화한 모형입니다. 보통 일정 나이가 지나면 나이가 들수록 사망확률이 증가합니다. force of mortality도 선형으로 매년 0.01씩 증가한다고 합시다. 이 경우 stope function을 이용하면 쉽게 표현할 수 있습니다.
	
		 0.1 (-.@[ ^!._0.01 ])  i. 6
	1 0.9 0.801 0.70488 0.613246 0.527391
		 1 , */\ -. 0.1 + 0.01 * i. 5 NB. stope function 대신 */\를 사용했습니다.
	1 0.9 0.801 0.70488 0.613246 0.527391

force of mortality가 커지면 처음에는 차이가 크지 않지만 시간이 지나면 큰 차이를 보입니다.

		require 'plot'
		p =: -.@[ ^!.0 _0.01 ]
		'title Survival Probability ;key "constant μ" "increasing μ"' plot |: 0.1 f i.6

![](http://i.imgur.com/IgtfagX.png)

###### 쓰임새: 파티장에 생일이 같은 사람이 있을 확률 ######
파티장에 여러 사람들이 있을 때 366명이상의 사람이 모이면 생일이 같은 두 명이 반드시 존재합니다. 교수님들은 이런 문제를 두고 자명하다고 하시고 증명을 건너 뛰시지요. 혹시 증명을 보고 싶으신 분들은 언젠가 배웠던 비둘기집의 원리를 떠올려 봅시다.

366명이 모이는 파티는 흔하지 않으므로 더 적은 사람이 모였을 때를 논의해 봅시다. 생일이 같은 사람이 있다/없다를 단정적으로 이야기하기는 어려우니 대신 같은 사람이 있을 확률이 얼마인지를 따져봅시다. 예를 들어 생일이 같은 사람이 있을 확률이 1/2을 넘으려면 몇 명의 사람이 파티장에 있어야 할까요? 

이 사람의 생일이 1월 1일인 확률, 저 사람이 생일이 1월 1일인 확률을 따져서 사슴이 장대에 올라 거북이와 두루미 삼천갑자동방삭을 하려고 하니 아 뭔가 복잡합니다. 이럴 때 한번쯤 생각해 볼 접근법이 여집합(complement)을 생각해보는 것입니다. 모든 사람의 생일이 다를 확률을 계산한 다음 이 사건의 여집합을 구하면 생일이 같은 사람이 존재하는 확률을 구할 수 있습니다. 이제 우리가 풀어야 할 문제가 명확하게 정의되었습니다.

> n명의 사람이 있을 때 모든 사람의 생일이 다를 확률을 구하고 그 확률의 complement를 구하라.

모든 사람의 생일이 다를 확률을 계산하는 것은 상대적으로 쉽습니다. 모든 사람에게 1에서 n까지 번호를 부여합시다.

[[:TODO:]]
#### ^. ####
####= ^. y ####=
Natural Log monad 0

지수함수가 있으면 역함수인 자연로그 함수도 필요합니다. ^(caret) 에 점을 추가해 서로 연관있음을 알려주고 있습니다.
###### 정의 ######
^. y ⇔ ln(y). monad ^ 의 inverse입니다.

		3 -: ^. ^ 3 NB. y ⇔ ^. ^ y .
	1
		^. 1
	0

####= x ^. y ####=
Logarithm dyad 0 0

자연상수 이외의 숫자를 밑(base)으로 지정해 로그값을 구할 수 있습니다.
###### 정의 ######
x ^. y ⇔ x를 밑(base)으로 y의 로그값을 계산합니다.

		10 ^. 100 NB. 상용로그
	2

####  | ####
##### | y #####
Magnitude monad 0

다른 프로그래밍 언어에서는 abs()라고 표현하기도 합니다. 실수일 경우 절대값이라고 부르고 복소수로 확장할 경우 magnitude라고 지칭되는 값을 반환합니다.
###### 정의 ######
| y ⇔ %: y * + y . 자신(y)과 켤레복소수(+ y)를 곱(*)한 값의 제곱근(%:)을 돌려줍니다.

		| 2 _2 4j3 _3j4
	2 2 5 5

##### x | y #####
Residue dyad 0 0

나머지 연산자입니다. 보통 a mod n 로 표기되어 a(피제수, dividend)를 n(Modulus, Divisor)로 나눈 나머지를 구합니다. 예를 들어 11 mod 3 은 11을 3으로 나눈 나머지 2값을 돌려줍니다. 하지만 J에서는 a와 n의 위치가 반대입니다. 11 mod 3 을 구하기 위해서는 3 | 11 을 실행해야 합니다.

J언어에서 dyad verb를 실행할 때 x를 control value로 이해하면 편한 경우가 많습니다. 이런 경우 y는 data로 취급됩니다. 예외도 있지만 대체로 이런 규칙을 따르는 편입니다. 혹시 dyad verb를 사용할 때 어떤 것이 x이고 어떤 것이 y가 되어야 할지 헷갈린다면 일단 x를 control value, y를 data로 삼아보세요.
###### 정의 ######
x | y ⇔ y mod x ⇔ y를 x로 나눈 나머지를 구합니다.

음수와 복소수로의 확장은 다음과 같은 정의를 따릅니다.
x | y ⇔ y-x*<. y % x+0=x .

		0 | 17
	17
		_2 | 17
	_1
		1j2 | 2j3
	0j_1

###### 관련 primitive ######
dyad | 는 monad <.(Floor)를 활용하여 정의합니다. 이런 관계가 monad <. 에 자세히 설명되어 있습니다.


=== families of functions ===
#### o. ####
####= o. y ####=
Pi Times  monad  0

수학과 공학의 여러 분야에서 원주율 π값이 필요하기 때문에 프로그래밍 언어는 이 값을 상수로 지원합니다. 예를 들어 자바스크립트와 자바에서는 Math.PI가, 파이썬에서는 math.pi가 π상수입니다. J도 primitive차원에서 π값을 다루는데 다른 프로그래밍 언어와는 약간 다른 접근을 합니다. 상수 대신 monad를 지원하는 것이지요.

참고로 APL도 비슷한 기호를 사용합니다. APL의 ○B 가 J의 o. y와 대응됩니다. 비슷하게 생기지 않았나요? 구두점을 제외하고 말입니다.

###### 정의 ######
o. y는 y times π와 동일합니다. o. y는 π의 y배수를 내놓습니다. o.의 rank가 0이기 때문에 π의 여러 배수를 한꺼번에 만들 수 있습니다.

		o. 2
	6.28319
		o. i. 5
	0 3.14159 6.28319 9.42478 12.5664

###### 쓰임새 ######
π값을 다룰 때 사용할 수 있습니다. 예를 들어 각도를 radian값으로 바꾸려면 degree to radian을 아래처럼 정의합니다. radian to degree는 따로 정의하지 않고 inverse를 이용합니다.

		dtr =: 180 %~ o. NB. degree to radian
		dtr 90
	1.5708
		rtd =: dtr^:_1 NB. radian to degree
		rtd o. 3
	540
		dtr b. _1 NB. inverse를 이용한 rtd의 정의
	[: 0.31830988618379069&* 180 * ]

주파수가 다른 cos함수를 만들어 봅시다. 주파수는 단위시간 당 사건이 일어나는 횟수를 말합니다. 1초를 단위시간으로 하는 경우는 Hz라고 하고 따라서 1Hz는 초당 1회 일어나는 사건을 말합니다. cos함수는 2ㅠ마다 같은 사건이 일어나니 주파수는 1 / 2π입니다. 어떻게 하면 1Hz주파수의 cos함수를 그릴 수 있을까요? 2π를 곱하면 됩니다.
	
		require 'trig plot'
		f =: cos@o.@+:
		'key cos,f' plot (0, o. 2) ; 'cos`f'

![cos 2pi plot](http://i.imgur.com/aMyZJfF.png)
###### 조금 더 ######
p-notation을 이용해 π를 나타낼 수 있습니다. XpY ⇔ X times π ^ Y 이니 π는 1p1, 2π는 2p1로 간단하게 나타낼 수 있습니다. 아니 이런 것까지 외우고 있어야 하실 수도 있지만 써보면 편합니다. 사용을 적극 권합니다.

https://webdocs.cs.ualberta.ca/~smillie/Jpage/Jpage.html
		pi=: [: ": [: <.@o. 10x"_^]
		PiList=: [: pi [: <: [: *: ]
		PiTriangle=: ([: #~ odd) ,/. PiList
		odd=: 1: + 2: * i.
		PiTree=: ([: - [: i. -@]) |."0 1 PiTriangle

#### x o. y ####
Circle Function dyad 0 0

J언어는 primitive차원에서 많은 함수를 지원합니다. 함수마다 이름을 지정하는 것이 번거롭기 때문에 연관이 있는 함수군(functions of family)을 하나의 primitive에 묶어두었습니다. x o. y가 이런 경우입니다. Circle Function이라는 명칭은 이런 사실에서 비롯됩니다. x로 원하는 함수를 지정합니다. 홀수 x값은 odd function에 대응하고 반대로 짝수 x값은 even function에 대응합니다. 음수 x는 역함수와 대응됩니다.

참고로 A○B이 circle function의 APL 기호입니다. o.기호는 생각보다 전통이 깁니다. J가 제공하는 함수군이 더 다양합니다.
###### 정의 ######
x o. y는 left operand 따라 삼각함수(trigonometric function), 쌍곡선함수(hyperbolic function), 그리고 함수들의 역함수 값을 산출합니다.
	
		1 o. 0		NB. sin(0)
	0
		1 o. o. 0.5	NB. sin(π/2)
	1
		2 o. 0		NB. cos(0)
	1
		2 o. o. 0.5	NB. sin(π/2)
	6.12323e_17
		 _1 o. 1	NB. sin^-1(1)
	1.5708
		rtd _1 o. 1	NB. sin^-1(1)라디안
	90
	
x o. y의 모든 함수군은 아래의 테이블과 같습니다.

x o. y 	x 	        		(-x) o. y
 
Sqrt 1-(Sqr y) 	0 	    	Sqrt 1-(Sqr y) 	
Sine y 			1 	    	Arcsine y 	
Cosine y 		2 	    	Arccos y 	
Tangent y 		3 	    	Arctan y 	
Sqrt (Sqr y)+1 	4 	    	Sqrt (Sqr y)-1 	
Sinh y 			5 	    	Arcsinh y 	
Cosh y 			6 	    	Arccosh y
Tanh y 			7 	    	Arctanh y
Sqrt - (1 + Sqr y)      8 	    	- Sqrt - (1 + Sqr y)    
RealPart y 		9 	    	y
Magnitude y 	10 	    	Conjugate (+y)
ImaginaryPart y	11 	    	j. y
AngleOf y 		12 	    	^j. y

###### 쓰임새 ######
dyad로 바로 사용되는 경우는 드물고 보틍은 conjecture &를 이용해 monad 형태로 사용됩니다. 자주 사용되는 cos, sin, tan 함수는 앞자리 번호를 가지고 있어 기억하기 쉽습니다.

		cos =: 2&o.
		cos o. 0.5
	6.12323e_17

9&o.(RealPart)와 11&o.(ImaginaryPart)는 복소수의 실수부와 허수부를 구하는 가장 좋은 방법입니다.

		real =: 9&o.
		img =: 11&.
		real 1j2
	1
		img 1j2
	2

같은 작업을 다른 primitive를 사용하고 싶으면 monad +.를 사용할 수 있습니다. 두가지 중 편한 걸 사용하세요.
 
		'r i' =: +. 1j2
		r
	1
		i
	2

'~addons/math/misc/trig.ijs'에 주요 함수가 이미 정의되어 있습니다. 특별한 일이 아니라면 여러분이 circle functions을 정의하는 필요는 없을 것입니다. 대신 라이브러리를 이용하세요. 위에 언급된 모든 함수군이 적절한 이름을 가지며 이미 정의되어 있습니다. x o. y를 직접 정의할 일이 없지만 가끔 코드 중에 o. 나오는 경우에 놀라지 않으려면 이런 것이 있다는 사실 정도를 알고 있는 것으로 충분합니다. 라이브러리는 require 'math/misc/trig'를 실행하거나 짧은 주소를 이용하는 경우 require 'trig'를 입력하세요.

y는 복소수도 다룰 수 있습니다. 복소수 y가 tangent를 더 살펴볼까요? tangent는 각도에 따라 밑변과 높이의 비를 돌려주는 함수입니다. 복소평면(complex plane) 위의 한 복소수 예를 들어 평면 위에 (3, 4) 좌표에 점이 있다고 한다면, 삼각형의 tangent값은 높이 4 / 밑변 3 = 1.33333입니다. arctan는 반대로 삼각비에서 각도를 찾는 함수입니다. 
	
		require 'trig'
		tan
	3&o.
		tan arctan 4 % 3
	1.33333
	
		dfr arctan 4 % 3	NB. degree from radian
	53.1301
	
TODO: atan2
잘 돌아가는 것처럼 보이지만 사실 한가지 결함이 있습니다. arctan는 (3, -4)와 (-3, 4)를 구별하지 못합니다. 방향이 반대인 두 점의 각도를 마치 동일한 것처럼 다룹니다.

		dfr atan2 3j_4
	_53.1301
		dfr atan2 _3j4
	126.87
	
	[[:image:]]
	
#### =. ####
http://code.jsoftware.com/wiki/Vocabulary/Assignment
#### z-locale ####




### indexing ###
#### { ####
##### { y #####
Catalogue monad 1

J언어를 익히며 가장 어색했던 순간이 monad {를 만났을 때입니다. 짝이 맞지 않은 {라는 건 들어본 적도 상상해 본적도 없는데 여기서 블랙스완을 만났습니다.
###### 정의 ######
{ y 는 y의 곱집합(Cartesian product)을 구합니다. y는 boxed list로 box로 집합을 구분합니다. 

		{ 1 2 ; 3 4 ; 5 6
	┌─────┬─────┐
	│1 3 5│1 3 6│
	├─────┼─────┤
	│1 4 5│1 4 6│
	└─────┴─────┘
	
	┌─────┬─────┐
	│2 3 5│2 3 6│
	├─────┼─────┤
	│2 4 5│2 4 6│
	└─────┴─────┘
	
		 { 'abc' ; 'def'
	┌──┬──┬──┐
	│ad│ae│af│
	├──┼──┼──┤
	│bd│be│bf│
	├──┼──┼──┤
	│cd│ce│cf│
	└──┴──┴──┘

###### 쓰임새 ######
곱집합을 구하는데 편리합니다.

	 	CP =: {@(,&<)
	 	1 2 CP 3 4 5
	┌───┬───┬───┐
	│1 3│1 4│1 5│
	├───┼───┼───┤
	│2 3│2 4│2 5│
	└───┴───┴───┘
	
##### x { y #####
From dyad 0 _

x를 인덱스삼아 y에서 필요한 부분을 추출합니다. SQL 구문의 'select x from y'에 대응되는 primitive입니다.  dyad { 에서 사용되는 인덱스 체계는 adverb }에서도 사용되니 이번 기회에 잘 봐둡시다.
###### 정의 ######
x { y 는 x번째 item을 y에서 선택합니다.

* x가 open일 때: x번째 item을 y에서 가지고 옵니다. x는 `-$y`에서 `<:$y` 사이의 값 중에서 선택가능합니다. 음수 x는 '뒤에서부터' x번째를 의미합니다. J의 인덱스는 0부터 시작합니다.
	
			0 1 _1 _2 { 'abcdefg' NB. 첫번째 두번째 마지막 뒤에서_두번째
		abgf
			] m =: i. 3 2 NB. 3 by 2 행렬
		0 1
		2 3
		4 5
		   	0 1 _1 _2 { m NB. x { y 에서 x는 item을 지칭합니다.
		0 1
		2 3
		4 5
		2 3

* x가 boxed list일 때: 각 axis별로 위치를 정합니다. box 하나가 인덱스 하나입니다.

			(< 2 1) { m NB. 세번째 행, 두번째 열에 위치한 원소.
		5
			(2 1 ; 1 1)
		┌───┬───┐
		│2 1│1 1│
		└───┴───┘
			(2 1 ; 1 1) { m NB. (세번째 행, 두번째 열) 원소 + (두번째 행 두번째 열) 원소
		5 3
		
* x가 level 2 boxed list일 때: 원소를 하나하나 지정하는 대신 각 axis별로 가져올 원소를 한꺼번에 지정합니다.
			
			< 0 1 ; 0 1
		┌─────────┐
		│┌───┬───┐│
		││0 1│0 1││
		│└───┴───┘│
		└─────────┘
			(<0 1 ; 0 1) { m NB. (첫번째 + 두번째) 행의 (첫번째 + 두번째) 열의 원소들
		0 1
		2 3
		
* x가 level 3 boxed list일 때: level 2 boxed list사례를 확장해서 배제할 원소를 각 axis별로 지정합니다. empty box는 모두 포함시킨다는 의미로 해석됩니다.

			< (<<_1) , (<<1)
		┌──────────┐
		│┌────┬───┐│
		││┌──┐│┌─┐││
		│││_1│││1│││
		││└──┘│└─┘││
		│└────┴───┘│
		└──────────┘
			 (< (<<_1) , (<<1)) { m NB. 마지막 행을 제외하고 두번째 열을 제외한 원소들
		0
		2
		
			< (<<'') , (<0)
		┌──────┐
		│┌──┬─┐│
		││┌┐│0││
		│││││ ││
		││└┘│ ││
		│└──┴─┘│
		└──────┘
			(< (<<'') , (<0)) { m NB. 첫번째 열의 원소
		0 2 4
		
###### 쓰임새 ######
boxed x는 필요에 따라 섞어쓸 수 있습니다.
		
		 < (<<0) , (< 1 2) , (< 0) NB. 첫번째 아이템을 제외하고 두번째 세번째 행의 첫번재 열 원소들
	┌───────────┐
	│┌───┬───┬─┐│
	││┌─┐│1 2│0││
	│││0││   │ ││
	││└─┘│   │ ││
	│└───┴───┴─┘│
	└───────────┘
		] mat =: i. 2 3 3
	 0  1  2
	 3  4  5
	 6  7  8
	
	 9 10 11
	12 13 14
	15 16 17
		( < (<<0) , (< 1 2) , (< 0)) { mat
	12 15

conjunction "(Rank) 쓰임새에 대해 이야기하며  Maching Learning Databases에서 데이터를 가져와 처리하는 과정을 소개했습니다. 거기서는 `1&{"1`을 사용해 데이터의 첫번째 열을 따오는 법을 설명했습니다. dyad { 를 배웠으니 rank를 사용하지 말고 인덱스를 이용해 데이터에 직접 접근해 봅시다.

		require 'web/gethttp'
		URL =: 'http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'
		data =: _5 ]\ (LF, ',') cutopen gethttp URL
		
		5 {. data NB. 데이터의 첫 5줄을 출력합니다. 꽃받침 길이 , 꽃받침 너비, 꽃잎 길이, 꽃잎 너비, 강(class)
	┌───┬───┬───┬───┬───────────┐
	│5.1│3.5│1.4│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│4.9│3.0│1.4│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│4.7│3.2│1.3│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│4.6│3.1│1.5│0.2│Iris-setosa│
	├───┼───┼───┼───┼───────────┤
	│5.0│3.6│1.4│0.2│Iris-setosa│
	└───┴───┴───┴───┴───────────┘

이제 꽃받침 너비를 list로 따로 꺼내고 싶습니다. 꽃받침 너비는 두번째 열에 기록되어 있습니다. 첫번째 axis(행)는 모두 가지고 오고 두번째 axis(열)은 두번째 아이템만 가지고 옵시다.
	
		 < (<a:) , (< 1)
	┌──────┐
	│┌──┬─┐│
	││┌┐│1││
	│││││ ││
	││└┘│ ││
	│└──┴─┘│
	└──────┘
		5 {. (< (<a:) , (< 1)) { data NB. 처음 5개의 꽃받침 너비
	┌───┬───┬───┬───┬───┐
	│3.5│3.0│3.2│3.1│3.6│
	└───┴───┴───┴───┴───┘
#### } ####
##### m} y #####
Composite Item(Item Amend) adverb _

형태는 기능을 따른다(Form Follows Function)는 사상이 J에 잘 반영되어 있다는 걸 감안하면  verb `{` 를 닮은 adverb `}`은 서로 비슷한 기능을 가지고 있을 거라는 것을 쉽게 짐작할 수 있습니다. `{`와 `}`은 둘 다 인덱스를 x로 받아 y에 적용하는 verb입니다. `{`은 x위치의 값을 선택하고, `}`는 x위치의 값을 수정합니다.

###### 정의 ######
* m이 numeric일 때: `m} y` 는 y의 item과 같은 shape을 가지는 array를 합성합니다. 합성된 array를 `z`라고 하면`z`의 rank와  m의 rank 그리고 y의 item의 rank는 일치합니다. `j{z` ⇔ `j{(j{m){y`의 관계를 가지고 있습니다.

			NB. rank 5의 list를 합성합니다. 
			NB. 합성 array의 0 2 4번째 atom은 y의 0번째 아이템에서
			NB. 합성 array의 1 3번째 atom은 
			NB. y의 1번째 아이템에서 가져옵니다.
			
			0 1 0 1 0} 'abcde' ,: 'ABCDE' 
		aBcDe
			
			] m =: i. 3 4
		0 1  2  3
		4 5  6  7
		8 9 10 11
			
			NB. rank 4의 list를 합성합니다.
			NB. 0번째 atom은 y의 _2번째 아이템(뒤에서 두번째)에서
			NB. 1번째 atom은 y의 2번째 아이템에서
			NB. 2번째 atom은 y의 0번째 아이템(첫번째)에서
			NB. 3번째 atom은 y의 _1번째 아이템에서(뒤에서 첫번째)에서
			NB. 가져와 합성합니다.
			
			_2 2 0 _1} m 
		4 9 2 1

* m이 gerund일 때: gerund를 y에 적용시킨 결과를 입력으로 받습니다.
	* ```v0`v1`v2} m```일 때: ```v0`v1`v2} m y``` ⇔ ```(v1 y) } v2 y```.
	
				]`?.~@#`+:} i. 4 4 NB. random shuffle
			16 26 4 14	
			
	* ```v1`v2} m```일 때: ```v1`v2} m y``` ⇔ ```(v1 y) } v2 y```.
	
				?.~@#`+:} i. 4 4 NB. random shuffle
			16 26 4 14
	
###### 쓰임새 ######
monad `m}` array의 item을 합성하는데 편리합니다. 다른 방법, 예를 들어 	`m {"0 1&.|: y`와 방법도 있으니 편한데로 사용하면 됩니다.

한가지 기억할 것은 `m`이 Boolean list인 경우 `m} y`를 사용하는데 속도의 개선이 있습니다. `m`이 Boolean list인 경우에는 AIP(Assignment In Place)의 적용을 받아 처리중에 합성된 변수를 새로 복사하지 않고 처리합니다. 아래의 특정한 형식으로 입력받은 코드만 적용됩니다. 

* ```name =: b } x ,: name```
* ```name =: b } name ,: x```

아래의 경우 name의 새로운 복사본을 만들지 않고 처리해 처리속도에 이점이 있습니다. 실제 처리속도의 차이를 확인해볼까요? 큰 차이는 없지만 혹시 이런 작업을 빈번하게 한다면 사용을 고려해볼 필요가 있겠습니다.

	alpha =: 'ABCDE'
	   6!:2 'alpha =: 0 1 0 1 0 } ''abcde'' ,: alpha'
	1.84746e_5

	alpha =: 'ABCDE'
   		6!:2 'alpha =:  0 1 0 1 0 {"0 1&.|: ''abcde'' ,: alpha'
	2.15537e_5	

##### x m} y #####
Amend _ _

이상하게 생기기로는 둘째가라면 서러울 primitive입니다. 짝이 맞지 않는 괄호라니 가당치도 않는 것 같지만 자세히 보면 왜 이런 기호를 선택했는지 이해가 가는 것도 같습니다. dyad `m}` 는 array의 값을 수정합니다. 

###### 정의 ######
* m이 numeric일 때: `x m} y`는 인덱스 m이 지정하는 위치의 y item을 x로 치환한 array를 생성합니다.

			(_ 1j2 1r2) 0 1 _1 } i. 7
		_ 1j2 2 3 4 5 0.5
			('*') 0 1 _1 } 'abcdefg'
		**cdef*

* m이 boxed list일 때: dyad { 의 인덱스 메커니즘을 dyad `m}` 에서 사용할 수 있습니다.
	
			] m =: i. 2 3 4
		 0  1  2  3
		 4  5  6  7
		 8  9 10 11
		
		12 13 14 15
		16 17 18 19
		20 21 22 23

			2p1 (0 1 2 ; 1 1 3) } m
		 0  1       2       3
		 4  5 6.28319       7
		 8  9      10      11
		
		12 13      14      15
		16 17      18 6.28319
		20 21      22      23

		 	1r2 (< (<< 1) , (<0 2) , (<1 3)) } m
		 0 1r2  2 1r2
		 4   5  6   7
		 8 1r2 10 1r2
		
		12  13 14  15
		16  17 18  19
		20  21 22  23

* m이 gerund일 때: gerund를 x와 y에 적용시킨 결과를 입력으로 받습니다.
	* ```v0`v1`v2} m```일 때: ```x v0`v1`v2} m y``` ⇔ ```(x v0 y) (x v1 y) } (x v2 y)```.

###### 쓰임새 ######
예를 들어 아주 큰 array의 마지막 item을 수정한다고 합시다. 기본적으로 J는 array의 입력의 모든 item을 출력 array에 일일이 복사합니다. 수정이 빈번하게 일어나고 array이 크다면 이런 불필요한 복사는 성능을 크게 해합니다. 

		old =: ?. 1e8 $ 10
		NB. 마지막 item을 _1로 치환합니다.
   		(6!:2 , 7!:2) 'new =: _1 (_1}) old'
	0.460932 1.07374e9

`x m} y`은 x로 치환된 새로운 array를 매번 새로 만드는 것을 기본으로 합니다. 이런 사실은 old와 new의 header 포인터를 보면 알 수 있습니다. 모든 noun은 header를 가지고 있습니다. header는 header 포인터로 접근가능한데 따라서 header 포인터가 다르면 다른 noun으로 취급됩니다. 자세한 내용은 memory부분을 참조하세요.

	 	 2 {. viewnoun 'old'
	┌───────────────────────────┬───────────────────────────────┐
	│1743066347616 1744207773792│64 0 1073741744 4 1 100000000 1│
	└───────────────────────────┴───────────────────────────────┘

		2 {. viewnoun 'new'
	┌───────────────────────────┬───────────────────────────────┐
	│1743045691600 1743133966432│64 0 1073741744 4 1 100000000 1│
	└───────────────────────────┴───────────────────────────────┘

header 포인터는 첫번째 box의 두번째 item입니다. 메모리주소를 가리키는 주소로 `old`와 `new`의 header 포인터가 각각 `1744207773792`와 `1743133966432`로 다른 것을 알 수 있습니다.

이것이 기본이지만 J는 특수한 몇가지 구문에 대해서는 `amend in place`라고 지칭되는 작업을 실행합니다. 다른 noun을 만들지 않고 원래 noun의 데이터를 수정해 바로 사용할 수 있습니다.

* `name =: x m} name`
* `name =: x (i}) name`
* `name =: name i}~ x`

위에서 수행한 작업을 다시 해봅시다. 정말 동일한 지를 확인해 볼까요? header pointer가 치환이 끝난 후에도 동일합니다. 
		
		old =: (_1) _1} old
		2 {. viewnoun 'old'
	┌───────────────────────────┬───────────────────────────────┐
	│1743066347616 1744207773792│64 0 1073741744 4 1 100000000 1│
	└───────────────────────────┴───────────────────────────────┘

성능은 어떨까요? 새로 old를 정의한 다음 성능측정을 해봅시다.

		old =: ?. 1e8 $ 10
		(6!:2 , 7!:2) 'old =: (_1) _1} old'
	0.41984 2176 

실행시간이 줄었습니다. 메모리 사용량도 획기적으로 줄었습니다. 가능하면 많이 사용하는 것이 좋겠습니다. 여러분 많이 쓰세요. 
### algebra ###
#### p. ####
##### p. y #####
Roots monad 1

방정식 해찾기를 primitive차원에서 지원합니다.
###### 정의 ######
`p. y` 는 y에 대응하는 다항식(polynomial)을 만족하는 해를 구합니다.

* y가 numeric list일 때(coefficient form): y는 numeric list로 상수항 계수로 시작해 고차항 계수 차례로 입력되어 있습니다. `p. y`는 multiplier와 roots를 각각 boxing한 list를 반환합니다.

 			p. 1 _2 1
		┌─┬───┐
		│1│1 1│
		└─┴───┘
			p. 12 _7 1
		┌─┬───┐
		│1│4 3│
		└─┴───┘
			 p. 23 4 2
		┌─┬──────────────────────┐
		│2│_1j3.24037 _1j_3.24037│
		└─┴──────────────────────┘

* y가 boxed list일 때(multiplier-and-roots form): y는 multiplier와 roots가 각각 boxing된 boxed list입니다. `p. y`는 입력된 multiplier와 roots에 대응되는 다항식의 계수를 반환합니다.

			p. 2 ; 3 4
		24 _14 2

* y가 one-atom(singleton) boxed table일 때(exponent form): 테이블 전체가 하나의 다항식과 대응됩니다. 테이블의 각 행은 coefficient 와 exponent을 item으로 가지고 있으며 exponent에 해당하는 차수의 계수를 표현합니다. `p. y`는 테이블에 대응되는 다항식의 coefficient list를 반환합니다.

			NB. 각 행은 c * y ^ e 에 대응
			] p =: < 12 0 , _7 1 ,: 1 2
		┌────┐
		│12 0│
		│_7 1│
		│ 1 2│
		└────┘
			 p. p
		12 _7 1


p. 의 inverse는 p. 자신입니다.

		p. b. _1
	p.	
		p. 12 _7 1
	┌─┬───┐
	│1│4 3│
	└─┴───┘
		p. p. 12 _7 1
	12 _7 1
   
###### 쓰임새 ######
Cayley-Hamilton theorem은 차차 해가기로 하고 일단 characteristic polynomial을 만들고 해를 풀어보는 것까지.
[[:TODO:]]

[Oh, No, Not Eigenvalues Again!](http://code.jsoftware.com/wiki/Doc/Articles/Play141)
##### x p. y #####
Polynomial dyad 1 0

우선 verb rank를 주의깊게 봐주세요. 왼쪽에 coefficient list가 오는 것이 자연스러울까요? 아니면 오른쪽? 이 질문에 답하실 수 있으시다면 축하드립니다. 여러분은 이제 J로 생각하기에 익숙해지셨습니다.

그리고 혹시 monad p. 를 설명하면서 왜 exponent form이 필요한지 궁금하지 않으셨습니까? dyad p. 는  exponent form을 활용해 dyad p. 를 multinomial까지 확장합니다.

###### 정의 ######
`x p. y`는 x에 대응되는 다항식에 y값을 대입합니다.

* x가 numeric list일 때(coefficient form): `x p. y' ⇔ `+/ c * y ^ i. # c =: x` .

			NB. y^2 + 2*y + 3, where y = 3
		3 2 1 p. 3

* x가 two-atom boxed list일 때(multiplier-and-roots form): `x p. y` ⇔ `m * */ x - r 'm r' =: x`

			NB. 2(y - 2)(y-4), where y = 3
			(2 ; 2 4) p. 3
		_2
			3 p.~ p. 2 ; 2 4
		_2
* x가 one-atom(singleton) boxed table일 때(exponent form): `(<c ,. e) p. <y` ⇔ `c +/ . * e */ . (^~) y`

			NB. a^2 + 4ab + 4b^2, where a = 2, b = 3
			 (<1 4 4 ,. 2 0 , 1 1 ,: 0 2) p. 2 3
		64
			c =: 1 4 4
			e =: _2 ]\ 2 0 1 1 0 2
			c +/ . * e */ . (^~) 2 3 ,: 4 5
		64
			NB. multipoints
			(<1 4 4 ,. 2 0 , 1 1 ,: 0 2) p. 2 3 ; 0 1 ; 1 0
		64 4 1

###### 쓰임새 ######
##### x p.!.s y #####
Stope Polynomial dyad 1 0

conjunction !.(fit)을 이용해 stope polynomial을 정의합니다.
###### 정의 ######
`p.!.s`는 dyad ^ 대신 dyad ^!.s를 사용해 polynomial을 정의합니다.
		
			NB. 1 + x + x(x-1) + x(x-1)(x-2)
			1 1 1 1 (p.!._1) 0 1 2 3
		1 2 5 16
			c +/"1@:*  0 1 2 3 ^!._1"0 1 i. # c =: 1 1 1 1
		1 2 5 16

#### p: ####
##### p: y #####
Primes monad 0

prime number(소수)는 1과 자기 자신만을 약수로 가지는 1보다 큰 자연수를 말합니다. monad `p:`는 약수를 찾아주는 primitive입니다.

prime의 p로 이해하시면 기억하기 편합니다.
###### 정의 ######
`p: y`는 y번째 소수를 반환합니다. 0번째 소수 2부터 시작해 y번째 소수를 구합니다. 
	
		p: 0
	2
		p: i. 10 NB. 처음 10개의 소수
	2 3 5 7 11 13 17 19 23 29

monad `p:` 의 inverse `p^:_1`는 y보다 작은 소수의 개수를 구합니다. 보통 π(y)함수라고 지칭됩니다.

		(,:~ p:^:_1) i. 10
	0 0 0 1 2 2 3 3 4 4
	0 1 2 3 4 5 6 7 8 9

###### 쓰임새 ######
어떤 수 `y`보다 작은 소수를 모두 구하려면 어떻게 해야 할까요? 자주 나올 법한 질문입니다만 머리를 긁적이게 하는 질문이기도 합니다. `p:`와 `p:^:_1`을 연계하면 쉽게 구할 수 있습니다.
			
			NB. 20미만 소수의 개수
			p:^:_1 ] 20
		8
			NB. 20미만의 모든 소수
			p: i. p:^:_1 ] 20 
		2 3 5 7 11 13 17 19
			NB. 좀 더 정돈하면
			i.&.(p:^:_1) 20
		2 3 5 7 11 13 17 19

1에서 60까지 정수의 π(y)함수를 그려봅시다.

			require 'plot'
			integers =: >: i. 60
			'type marker' plot (; p:^:_1) integers

##### x p: y #####
Primes dyad _ _

소수와 관련된 함수군이 정의되어 있습니다. x에 따라 소수와 관련있는 함수 여러 개가 정의됩니다.
###### 정의 ######
* `_4 p: y`: y보다 작은 소수중 가장 큰 소수를 구합니다.
		
			NB. 소수버전의 floor 함수라고 할 수 있습니다.
			 _4 p: 15 7.9
		13 7

* `_1 p: y`: y보다 작은 소수의 개수를 구합니다. monad `p:^:_1`와 동일합니다. 2^31를 넘어서는 숫자는 Miller-Rabin 확률적 알고리즘을 따릅니다.

			_1 p: i. 10
		0 0 0 1 2 2 3 3 4 4

* `0 p: y`: y가 소수가 아니면 1, y가 소수이면 0을 반환합니다. 2^31를 넘어서는 숫자는 Miller-Rabin 확률적 알고리즘을 따릅니다.

			0 p: i. 10
		1 1 0 0 1 0 1 0 1 1
			NB. 소수가 아닌 수 거르기
			(0 p: integers) # integers =: i. 10
		0 1 4 6 8 9

* `1 p: y`: y가 소수이면 0, 소수가 아니면 1을 반환합니다.

			1 p: i. 10
		 0 0 1 1 0 1 0 1 0 0
			NB. 소수 거르기
			(1 p: integers) # integers =: i. 10
		2 3 5 7

* `2 p: y`: y를 인수분해하여 2-rows array를 구합니다. 첫번째 행은 소인수(prime factor), 두번째 행은 소인수의 지수(exponent)를 나타냅니다. dyad `__ q: y`와 동일합니다.
		
			2 p: 24
		2 3
		3 1
			*/ ^/ 2 p: 24
		24
* `3 p: y`: y를 인수분해한 결과를 소인수 list로 표현합니다. monad `q: y`와 동일합니다.


			3 p: 24
		2 2 2 3
			*/ 3 p: 24
		24

* `4 p: y`: y보다 큰 소수중 가장 작은 소수를 구합니다. `_4 p: y`와 대칭됩니다.

			NB. 소수버전의 ceiling 함수라고 할 수 있습니다. 
			4 p: 4.5 7 9
		5 11 11

* `5 p: y`: 오일러 φ 함수(Euler's phi function, 혹은 totient function)입니다. 0부터 y보다 작은 정수 중에 y와 서로소인 정수의 개수를 말합니다.

			NB. 서로소 ⇔ GCD가 1인 관계
			1 = 15 +. integers =: i. 15
		0 1 1 0 1 0 0 1 1 0 0 1 0 1 1
			integers #~ 1 = 15 +. integers =: i. 15
		1 2 4 7 8 11 13 14
			+/ 1 = 15 +. integers =: i. 15
		8
			5 p: 15
		8

###### 쓰임새 ######
[[:TODO:]]
RSA

#### q: ####
##### q: y #####
Prime Factors monad 0

monad p: 가 소수 자체를 다루는 verb였다면 monad q: 는 소인수 분해와 관련있는 verb입니다.

quotient의 q로 이해하시면 기억하기 편합니다. 혹은 p 다음은 q로 기억하셔도 좋습니다.

###### 정의 ######
`q: y`는 y의 소인수의 오름차순 list를 구합니다. `q:`의 inverse는 `*/`입니다. 

		q: 108
	2 2 3 3 3
		*/ q: 108
	108
		q: b. _1
	*/

###### 쓰임새 ######
monad q: 를 이용하여 소인수분해를 편하게 할 수 있습니다. 꽤 큰 수까지 지원하기 때문에 언제나 사용하는데 부담이 없습니다.
		
		NB. 처음 8개의 메르센 소수(Mersenne prime)
		<: 2 ^ x: 2 3 5 7 13 17 19 31
	3 7 31 127 8191 131071 524287 2147483647
		*/ <: 2 ^ x: 2 3 5 7 13 17 19 31
	99937205886963012698207462133
		 q: */ <: 2 ^ x: 2 3 5 7 13 17 19 31
	3 7 31 127 8191 131071 524287 2147483647

monad `q:`를 활용해 [프로젝트 오일러 problem 3](https://projecteuler.net/problem=3)에 도전해보세요. 깜짝 놀랄만큼 짧은 코드로 문제를 풀 수 있습니다. 다른 프로그래밍 언어와 비교하면서 J를 배우는 꿀재미를 느껴봅시다.
 
##### x q: y #####
Prime Exponents dyad 0 0

dyad `p:`와 마찬가지로 소인수분해와 관련된 여러 함수를 모아두었습니다. `x`로 함수를 지정합니다.

###### 정의 ######
* `x`가 양수일 때: `x q: y`는 `y`를 소인수분해할 때 **처음** `x`개 소수의 지수(exponent)를 구합니다. `x`가 `_`(infinity)일 때는 필요한 만큼 `x`를 결정합니다. 

			NB. 처음 두 개의 소수(2 3)의 지수는 3 1입니다.
			2 q: 24
		3 1
			NB. 처음 두 개의 소수(2 3)의 지수는 2 0입니다.
			2 q: 100
		2 0
			NB. 처음 세 개의 소수(2 3 5)의 지수는 2 0 2입니다.
   			3 q: 100
		2 0 2
			NB. infinity x는 필요한 만큼으로 해석됩니다.
			_ q: 100
		2 0 2
		
* `x`가 음수일 때: 2-row 테이블을 생성합니다. 첫번째 행은 **마지막** `|x`개 소수, 두번째 행은 **마지막** `|x`개  소수의 지수를 구합니다. 양수 x와 다르게 zero exponent는 생략됩니다. `__`(negative infinity)는 필요한 만큼으로 해석됩니다.

			NB. 마지막 두 개 소수와 지수
			_2 q: 24
		2 3
		3 1
			NB. 마지막 두 개 소수와 지수
			_2 q: 100
		5
		2
			NB. 마지막 세 개 소수와 지수
			NB. zero-exponent는 생략됩니다.
			_3 q: 100
		2 5
		2 2
			NB. negative infinity x는 필요한 만큼으로 해석됩니다.
			 __ q: 100
		2 5
		2 2

## 기타 ##
"Harder, Better, Faster, Stronger"

Work It

Make It

Do It
Makes Us
Harder
Better
Faster
Stronger
More Than
Hour
Our
Never
Ever
After
Work is
Over [x2]

Work It Harder Make It Better
Do It Faster, Makes Us stronger
More Than Ever Hour After
Our Work Is Never Over [x7]

Work It Harder, Make It
Do It Faster, Makes Us
More Than Ever, Hour
Our Work Is Never Over

Work It Harder Make It Better
Do It Faster, Makes Us Stronger
More Than Ever Hour Af-
Our Work Is Never Over [x3]

Work It Har-Der
M-ake I-t Bet-ter
D-o It Faster Makes Us Stronger
More Than Ever Hour
Our Work Is Never Over

Work It Harder
Do It Faster
More than ever
Our Work Is Never Over

Work It Harder
Make It Better
Do It Faster
Makes Us Stronger
More Than Ever
Hour After
Our Work Is Never Over 
######== mathematics notations ######=
⇔
소
###### color ######

> COLORBREWER_z_ =: 
> 
> 
> 103   0  31 , 178  
> 24  43 , 214  96  77 , 244 165 130 , 253 219 199 , 247 247 247 , 209 229 240 , 146 197 222 , 67 147 195 , 33 102 172 ,: 5  48  97

###### change current directory ######
1!:44 'C:\Users\yunskim\Dropbox\documents\programming_J'
###### wd command syntax ######
http://code.jsoftware.com/wiki/Guides/Window_Driver/Command_Syntax

`'one *two three four'` is parsed as:

	┌───┬──────────────┐
	│one│two three four│
	└───┴──────────────┘

<a name="noun_memory"></a>
###### 메모리 ######
메모리를 열어보거나 바꾸는 일은 일상의 업무에는 불필요하지만 복잡한 프로그래밍 과제에 있어서는 필수적인 경우가 많습니다. J의 메모리 관련 foreign verb를 알아보고 J가 어떻게 메모리를 사용하는지 이해를 구해봅시다. 그리고 파일을 메모리의 일부처럼 확장하는 jmf 라이브러리에 대해 알아봅시다.

조금 복잡하고 새로운 것을 배워야 하는 부담이 있지만 효용만큼은 확실합니다.
####### named noun ######
J의 named noun은 세부분으로 구현됩니다. [source](http://code.jsoftware.com/wiki/Guides/Named_Noun_Internals)

* symbol table entry
* header
* data

symbol table이 header를 가리키고, header는 다시 data를 가리킵니다. 예제를 통해 자세한 내용을 알아봅시다.

새로운 noun을 정의합시다.
	
		] m =: i. 2 3
	0 1 2
	3 4 5

새로운 noun을 정의하면 symbol table entry가 생성됩니다. 새로 만든 noun의 symbol table entry의 포인터를 foreign `15!:6`이용해 찾아봅시다.

		symget =: 15!:6
   		] s =: symget <'m'
	2392449114424

이 entry는 두 개의 포인터를 가집니다.

* 'm' name 를 가리키는 포인터
* header를 가리키는 포인터

`15!:6`을 이용해 구한 sysmbol table entry의 포인터를 이용해 다시 name을 가리키는 포인터, header를 가리키는 포인터를 구해봅시다.

		memr =: 15!:1
   		NB. memr address byte_offset count [,type]
   		NB. type
   		NB. char 2 ; integer 4 ; float 8 ; complex 16
   
   		] 'np hp' =: memr s , 0 , 2 , 4
	2392444652304 2392446144688

header를 참고하는 포인터 hp를 구했으니 이제 header의 내용을 찾아봅시다. header는 integer list입니다. 처음 7개는 차례대로 아래의 항목에 대응되는 정수입니다. 처음 7개의 정수를 fixed header라고 따로 칭합니다.

* offset from header to data
* internal Flags
* reserved size for data - at least large enough to store data
* data type(as in 3!:0)
* reference count
* data length(```*/ $ y``` if dense ; 1 if sparse)
* rank(```#$y```)

마지막으로 shape이 리스트 끝에 추가됩니다.
* shape(list of integers; ```$ y```)

실제로 header에 있는 내용을 따오면 아래와 같습니다.

		] header =: memr hp, 0 9 4
	72 0 168 4 1 6 2 2 3	
   		] fheader =: memr hp, 0 7 4
	72 0 168 4 1 6 2
   		'offset flag size type refcount len rank' =: fheader
   
   		NB. 64비트 시스템: 7 x 8 = 56
   		NB. 32비트 시스템: 7 x 4 = 28
   		] shape =: memr hp, 56 , rank , 4
	2 3
   
이제 header의 내용을 바탕으로 data를 찾아옵시다. 
		
		NB. 혹은 data =: memr (hp + offset), 0 , len, type
   		] data =: memr hp, offset, len , type
	0 1 2 3 4 5

정리하는 의미로 noun의 정보를 알려주는 verb viewnoun을 정의해서 실행해보겠습니다.

	viewnoun =: 3 : 0
	sp =. 15!:6 boxopen y
	'np hp' =. 15!:1 sp, 0 2 4
	fheader =. 15!:1 hp, 0 7 4
	'offset flag size type refcount len rank'=. fheader
	shape =. 15!:1 hp, 56, rank, 4
	data =. 15!:1 hp, offset, len, type
	(np, hp) ; fheader ; shape ; data
	)
		 viewnoun 'm'
	┌───────────────────────────┬────────────────┬───┬───────────┐
	│2392444652304 2392446144688│72 0 168 4 1 6 2│2 3│0 1 2 3 4 5│
	└───────────────────────────┴────────────────┴───┴───────────┘
   
`viewnoun`은 noun의 간단한 메모리 관련 유틸리티입니다. 이 유틸리티를 이용해 몇가지 확인할 점이 있습니다.

noun을 다른 name에 복사하면 어떤 일이 일어날까요?

		n =: m
   		viewnoun 'n'
	┌───────────────────────────┬────────────────┬───┬───────────┐
	│2392444652560 2392446144688│72 0 168 4 2 6 2│2 3│0 1 2 3 4 5│
	└───────────────────────────┴────────────────┴───┴───────────┘
   
noun n의 header 포인터는 noun m의 header 포인터와 동일합니다. symbol table entry의 name 포인터만 차이가 나고 reference counter가 하나 증가한 것을 제외하고 header 포인트는 동일하고 데이터도 변화가 없습니다.

이제 새로 복사한 noun n을 변형시켜 보겠습니다.
	
		n =: n , 7
		viewnoun 'n'
	┌───────────────────────────┬────────────────┬───┬─────────────────┐
	│2392444652560 2392446148016│72 0 168 4 1 9 2│3 3│0 1 2 3 4 5 7 7 7│
	└───────────────────────────┴────────────────┴───┴─────────────────┘

symbol table entry의 name 포인터는 이전과 그대로지만 header 포인터는 달라졌습니다. 그리고 data도 달라졌습니다. 한가지 눈여겨 볼 부분은 size가 168로 동일하다는 점입니다. 미리 여유있게 size를 잡아두어 데이터를 추가하더라도 기존에 잡아둔 사이즈를 넘지 않아 header 포인터를 다시 잡지 않았습니다. 만약 많은 데이터를 추가한다면 header 포인터를 다시 잡습니다. 아래의 예제에서 새로 잡은 포인터를 확인할 수 있습니다.

		n =: n , i. 30 2
		viewnoun 'n'
	┌───────────────────────────┬─────────────────┬────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────...
	│2392444652560 2392446255584│72 0 936 4 1 99 2│33 3│0 1 2 3 4 5 7 7 7 0 1 0 2 3 0 4 5 0 6 7 0 8 9 0 10 11 0 12 13 0 14 15 0 16 17 0 18 19 0 20 21 0 22 23 0 24 25 0 26 27 0 28 29 0 30 31 0 32 33 0 34 35 0 36 37 0 38 39 0 40 41 0 42 43 0 44 45 0 46 47 0 48 4...
	└───────────────────────────┴─────────────────┴────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────...

###### J noun 만들기 ######
지금까지는 noun을 정의하고 정의된 noun의 symbol table entry, header 그리고 data를 차례로 찾아보았습니다. 이제는 이 방향을 거꾸로 짚어보겠습니다. data와 header, symbol table entry를 정의해 noun을 만들어보겠습니다.

주의: 메모리에 직접 접근하는 것은 매우 위험합니다. 꼭 하던 작업을 저장하거나 다른 세션을 열어서 아래의 내용을 진행하세요. 제가 이 부분을 쓰면서 여러 번 Jqt를 꺼트렸다는 사실을 알아주세요. 

비유하자면 C에서 동적메모리를 할당하는 것과 비슷한 점이 있습니다. 우선 해야 할 것은 적당한 메모리를 할당받는 것입니다. foreign `15!:n`에 메모리와 관련된 verb가 정의되어 있습니다. 그 중 `15!:3`을 사용해 메모리를 할당받습니다. foreign 정의를 그대로 쓸 수도 있지만 관용적으로 mema로 사용하는 경우가 많습니다.

		NB. pointer =: 15!:3 data_size_in_bytes
   		mema =: 15!:3
		NB. 6개의 8바이트 데이터
   		] dp =: mema 6 * 8
	2392446528640

이제 할당받은 포인터에 data를 씁니다. foreign `15!:2`를memw로 재명명하여 사용합시다.

		memw =: 15!:2
		NB. (data) memw address, byte_offset, count [, type]
		(i. 6) memw	dp, 0 6 4
		
이제 data를 저장했으니 header를 만들 차례입니다. header를 만드는 foreign verb는 `15!:8`입니다. 바이트 크기를 입력으로 받고 header pointer를 내놓습니다. 

		allochdr =: 15!:8
		hp =: allochdr 5r3 * 6 * 8
		NB. 'offset(from header to data) flag size type refcount len rank' =: fheader
		fheader =: (dp - hp), 0 80 4 1 6 2
		header =: fheader, 2 3
		header memw hp, 0 ,(7 + 2) , 4

이제 마지막 단계로 symbol table entry와 header 포인터를 연결하는 차례입니다. foreign `15!:7`을 `symset`으로 재명명하여 사용합시다.

		symset =: 15!:7
		] mydata =: symset hp
	0 1 2
	3 4 5
		
		 viewnoun 'mydata'
	┌───────────────────────────┬───────────────────┬───┬───────────┐
	│1743066348000 1743066610144│275104 0 80 4 2 6 2│2 3│0 1 2 3 4 5│
	└───────────────────────────┴───────────────────┴───┴───────────┘

이제 mydata를 기존의 방법으로 만든 noun과 차이없이 사용할 수 있습니다.

###### J memory mapped file ######
지금까지 J가 noun의 메모리를 어떻게 다루는지 알아보았습니다. 자체로도 흥미롭고 쓸모가 있는 내용이지만 아무래도 강렬한 한 방이 부족한 것 같습니다. 그래서 준비했습니다. 이제 J memory mapped file에 대해 알아보고 



####### J memery mapped file #######
###### 메모리 관련 foreign ######
stdlib.ijs에 정의

memr ⇔ 15!:1 은  read bytes from memory [as type]
data =: memr address(base address) , byte_offset(number of address location to get to a specific absolute address, count(number of data elements) [,type]

A _1 count reads up to the first 0.

memr and memw type parameter is 2 4 8 or 16 for char integer float or complex. The default is 2. The count parameter is a count of elements of the type. 

In computer engineering and low-level programming (such as assembly language), an offset usually denotes the number of address locations added to a base address in order to get to a specific absolute address. In this (original) meaning of offset, only the basic address unit, usually the 8-bit byte, is used to specify the offset's size. In this context an offset is sometimes called a relative address.

type

Type: the internal type of the noun y , encoded as follows:
2의 배수로 증가

* 1 	boolean
* 2 	literal
* 4 	integer
* 8 	floating point
* 16 	complex
* 32 	boxed
* 64 	extended integer
* 128  	rational
* 1024 	sparse boolean
* 2048 	sparse literal
* 4096 	sparse integer
* 8192 	sparse floating point
* 16384 	sparse complex
* 32768 	sparse boxed
* 65536 	symbol
* 131072  	unicode


foreign `3!:1`의 설명을 참조하세요.

   data=: memr address,byte_offset,count [,type]
#### zig-zag matrix ####
[zig-zag matrix](http://rosettacode.org/wiki/Zig-zag_matrix#J)