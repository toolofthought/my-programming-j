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