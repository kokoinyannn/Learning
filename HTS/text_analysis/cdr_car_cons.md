http://www.math.s.chiba-u.ac.jp/~matsu/lisp/emacs-lisp-intro-jp_8.html#SEC96

リストの car とは、単にそのリストの最初の要素のことである。従って、 リスト (rose violet daisy buttercup) の car は rose である。
(car '(rose violet daisy buttercup))

List の cdr はリストの残りの部分である。つまり、cdr 関数は リストの最初の要素を除いた部分を返す。従って、'(rose violet daisy buttercup) というリストの car が rose であるように、この リストの残りの部分、つまり cdr を施して返される値は (violet daisy buttercup) である。
(cdr '(rose violet daisy buttercup))

-------------------------------------------------------------------------------------------------


[ << ]	[ >> ]	 	 	 	 	 	[Top]	[Contents]	[Index]	[ ? ]
7. car, cdr, cons：基本関数
Lisp では car、cdr、そして cons が基本的な関数であ る。cons 関数はリストを作るのに使われ、car 関数と cdr 関数はそれらを分解するのに使われる。

copy-region-as-kill 関数を "walk-through" する時に、cons と共に cdr の二つの変種である setcdr と nthcdr につ いても見るつもりだ。(8.5 copy-region-as-kill).

奇妙な名前の由来	  	
7.1 car と cdr	  	リストの一部を取り出すための関数
7.2 cons	  	リストの構成
7.3 nthcdr	  	cdr を何回もよびだす
7.4 setcar	  	リストの最初の要素の変更
7.5 setcdr	  	リストの残りの要素の変更
7.6 練習問題	  	cons についての練習問題

奇妙な名前の由来
cons 関数の名前は特に非合理的なものではない。この名前は `construct' という単語を略したものである。一方、car と cdr の名前の由来は難解である。car は `Contents of the Address part of the Register' というフレーズの頭文字から来ており、cdr (`could-er' と発音する) は `Contents of the Decrement part of the Register' というフ レーズから来ている。これらのフレーズは、Lisp が開発された頃の極めて初期 のハードウェアの特定の部分に基づくものであるが、単に時代遅れであるという だけでなく、実に25年以上もの間、Lisp に関わる人々にとって全く見当はずれ のものであった。だがしかし、これらの関数をもっと合理的な名前で呼ぼうとし た学者も何人かいたにも関わらず、現在でもこの古い用語が使われている。特に Emacs Lisp のソースコードでもこの用語が使われているので、この入門書でも これらを使うことにしよう。


7.1 car と cdr
リストの car とは、単にそのリストの最初の要素のことである。従って、 リスト (rose violet daisy buttercup) の car は rose である。

もし、この文章を GNU Emacs の Info で読んでいるなら、次を評価してみるこ とでこのことを確認出来る。

 	
(car '(rose violet daisy buttercup))

このＳ式を評価すると、エコー領域に rose と表示されたはずだ。

明らかに car には first という名前の方がふさわしいし、 よくそのように提案されてもいる。

car は最初の要素を除いたりはしない。ただ単にそれが何かを伝えるだ けである。あるリストに car を施した後も、そのリストは元のままであ る。専門用語では、car は「非破壊的 (non-destructive)」であると言っ たりする。この特徴は、後で重要であることが分るだろう。

List の cdr はリストの残りの部分である。つまり、cdr 関数は リストの最初の要素を除いた部分を返す。従って、'(rose violet daisy buttercup) というリストの car が rose であるように、この リストの残りの部分、つまり cdr を施して返される値は (violet daisy buttercup) である。

このことも、次をいつも通り評価してみることで確かめられる。

 	
(cdr '(rose violet daisy buttercup))

これを評価すると、エコー領域に (violet daisy buttercup) と表示さ れたはずだ。

car と同様、cdr もリストから要素を取り除いたりしない---た だ単に二番目以降の要素のリストを返すだけである。

ついでに言っておくと、上の例では花のリストに引用符が付いている。もしこれ が付いていなければ Lisp インタプリタは rose を関数として呼び出し て評価しようとするだろう。この例では、そういうことをしたいのではなかった。

明らかに cdr には rest という名前の方が、ふさわしい。

(ここでちょっと教訓。あなたが新しい関数に名前をつける場合、それについて 極めて注意深く考えないといけない。というのも、あなたは多分、あなたが思う よりずっと長い間、その名前につきまとわれることになるからだ。この文章でこ れらの名前を使っているのは、Emacs Lisp のソースコードでこれらを使ってい るからであり、そうしないとコードを読むのがつらいだろうからである。しかし、 どうかあなた自身はなるべくこういった言葉使いを避けて欲しい。そうすれば後 からやってきた人々から感謝されるだろう。)

car や cdr を (pine fir oak maple) といったシンボル からなるリストに施した時、car によって返される要素はシンボル pine であり、括弧は付いてはいない。pine はこのリストの最初 の要素である。しかしながら、リストの cdr は (fir oak maple) というリストである。それは、次の式をいつものよう に評価してみればすぐに分る。

 	
(car '(pine fir oak maple))

(cdr '(pine fir oak maple))

一方、リストのリストでは、そのリストの最初の要素がそれ自身リストである。 この場合、car は最初のリストとしての要素を返す。例えば次のリストは 三つのサブリストを含んでいる。各々、肉食動物、草食動物、海洋哺乳類の リストである。

 	
(car '((lion tiger cheetah)
       (gazelle antelope zebra)
       (whale dolphin seal)))

この場合、最初の要素、つまり全体のリストの car は肉食動物のリスト (lion tiger cheetah) である。そしてリストの残りは((gazelle antelope zebra) (whale dolphin seal)) である。

 	
(cdr '((lion tiger cheetah)
       (gazelle antelope zebra)
       (whale dolphin seal)))

ここでもう一度、car と cdr が非破壊的であると言っておいた ほうがいいだろう。これらの関数はリストを変化させないのだ。このことはこれ らの関数を使うにあたって極めて重要になる。

また最初の章でアトムについて議論した時に、Lisp では「ある種のアトム、 例えば配列 (array)、は複数の部分に分解出来る。しかし、そのメカニズムはリ ストを分解する時のメカニズムとは違う。リストの操作に関する限り、リストの アトムは分解不可能である。」と書いた。(1.1.1 Lisp のアトム). car 関 数と cdr 関数はリストを分解するのに使われ、Lisp の基本と考えられ ている。しかし、これらの関数は配列を分解したり、部分を取り出したりするの には使えない。従って、配列はリストではなくアトムであると考えられるわけであ る。また、もう一つの基本的な関数である cons も、リストを構築するこ とは出来るが、配列を作ることは出来ない。(配列は、配列を扱う特殊な関数を 使って操作する。詳しくは section `Arrays' in The GNU Emacs Lisp Reference Manual, を参照のこと。)


7.2 cons
cons は、リストを作る関数である。car や cdr とは逆 の働きをするものだと言える。例えば、cons は三つの要素を持つリスト (fir oak maple) から四つの要素を持つリストを作ることが出来る。つ まり、

 	
(cons 'pine '(fir oak maple))

を評価すると、

 	
(pine fir oak maple)

というリストがエコー領域に表示される。このように、cons はリストの 先頭に新しい一つの要素を付け加える。リストに要素を追加するわけである。

cons には、まず要素を付け加えるべきリストが必要になる。 (2) 何も無いところからスタートすることは出来ないの である。もし、リストを作りたいならば、少くともまず初めに空リストを用意す る必要がある。次に挙げるのは、cons を連続して使って花のリストを作 るものである。もし、この文章を Emacs の Info で読んでいるのなら、各々の Ｓ式をいつものように評価することが出来る。表示される値が `=>' の後に書かれている。これを「次のように評価される」と置き換えて読んで欲し い。

 	
(cons 'buttercup ())
     => (buttercup)

(cons 'daisy '(buttercup))
     => (daisy buttercup)

(cons 'violet '(daisy buttercup))
     => (violet daisy buttercup)

(cons 'rose '(violet daisy buttercup))
     => (rose violet daisy buttercup)

最初の例では、空リストが () という形で出ている。そして、 buttercup とこの空リストから新しいリストが作られる。見ればわかる と思うが、空リストは作成されたリストの要素としては出て来ない。 (buttercup) というリストが見えるだけである。空リストは要素として は数えられない。というのも、空リストには要素が一つもないからである。一般 的にいうと、空リストは見えないのである。

二番目の例では (cons 'daisy '(buttercup)) を評価することで daisy を buttercup の前に付け加えて、二つの要素を持つリス トを作っている。三番目の例では daisy と buttercup の前に violet を加えて三つの要素を持つリストを作っている。

7.2.1 リストの長さを調べる: length	  	リストの長さを知る

7.2.1 リストの長さを調べる: length
あるリストがどれだけ多くの要素を持つかを見るには、length という Lisp の関数を使えばよい。次のように感じである。

 	
(length '(buttercup))
     => 1

(length '(daisy buttercup))
     => 2

(length (cons 'violet '(daisy buttercup)))
     => 3

三番目の例では、三つの要素を持つリストを作るために cons 関数が使 われ、その結果出来たリストが length 関数に引数として渡されている。

length 関数を使って、空リストの中の要素の数を数えることも出来る。

 	
(length ())
     => 0

予想した通りだと思うが、空リストの中の要素の数は零個と数えられる。

リストが無い場合にその長さを求めさせてみるのも面白い実験である。つまり、 引数として空リストすら渡さずに length を呼び出すのである。

 	
(length )

やってみれば分るが、これを評価すると、エラーメッセージが表示される。

 	
Wrong number of arguments: #<subr length>, 0

これは引数の数が間違っているという意味のメッセージである。今の場合なら、 本当は一つの引数が渡されなければならない。例えば引数がリストなら、そのリ ストの長さをこの関数が測ることになるはずだったのである。(リストが沢山の 要素を持っていたとしても、一つのリストは一つの引数であるこ とに注意。)

(訳註：因みに length の引数は必ずしもリストでなくともよい。例えば length に文字列を渡すと、その文字列の長さが返る。)

エラーメッセージの中の `#<subr length>' という部分は、関数の名前で ある。これは、特殊な表記法で書かれている。`#<subr' は length が Emacs Lisp ではなく C で書かれたプリミティブな関数であることを表して いる。(`subr' は `subroutine' の略である。) section `What Is a Function?' in The GNU Emacs Lisp Reference Manual, にこの辺りのことについてのより詳しい情報がある。


7.3 nthcdr
nthcdr 関数は、cdr 関数に関連した関数である。これは、リス トの cdr を繰り返し取るものである。

(pine fir oak maple) の cdr を取ると、(fir oak maple) が返される。もしこの返された値にもう一度同じ操作をすると、 (oak maple) が返される。(勿論、リストに何回 cdr を施しても、 元のリストは変化しない。cdr は対象リストを変化させはしないからで ある。従って、cdr の cdr を評価する必要がある。) これを続 ければ、結果として空リストが返る。この場合、() ではなく nil が表示される。

復習のために、以下に cdr の繰り返しの例を書いておく。 `=>' の後に表示される結果が書いてある。

 	
(cdr '(pine fir oak maple))
     => (fir oak maple)

(cdr '(fir oak maple))
     => (oak maple)

(cdr '(oak maple))
     => (maple)

(cdr '(maple))
     => nil

(cdr 'nil)
     => nil

(cdr ())
     => nil

複数の cdr を、間に表示される値を挟まずに続けて書くことも出来る。

 	
(cdr (cdr '(pine fir oak maple)))
     => (oak maple)

この場合は、Lisp インタプリタは最も内側のリストを先に評価する。最も内側 にあるリストには引用符が付いているので、それはそのままリストとして返され、 内側の方の cdr に渡される。この内側の cdr はこのリストの二 番目以降の要素からなるリストを返し、外側の cdr に渡す。これは、元 のリストの三番目以降の要素からなるリストを返すというわけである。この例の ように、cdr 関数を二回続けて使うと、元のリストの最初の二つの要素 を除いたリストが返されることになる。

nthcdr 関数は、cdr 関数を繰り返して使うのと同じことをして くれる。次の例では引数2がリストと一緒に nthcdr 関数に渡され、その 結果最初の二つの要素を除いたリストが返されている。これはまさに cdr を二回繰り返してリストに施したのと同じ結果である。

 	
(nthcdr 2 '(pine fir oak maple))
     => (oak maple)

元の4つの要素を持ったリストを使って、0、1、5などを含むいろいろな引数を nthcdr に与えた場合に何が起こるかを見てみよう。

 	
;; リストはそのまま。
(nthcdr 0 '(pine fir oak maple))    
     => (pine fir oak maple)

;; 最初の要素を除いた残りのコピーを返す。
(nthcdr 1 '(pine fir oak maple))    
     => (fir oak maple)

;; 三つの要素を除いた残りのコピーを返す。
(nthcdr 3 '(pine fir oak maple))    
     => (maple)                

;; 四つ全部を除いた残りを返す。
(nthcdr 4 '(pine fir oak maple))    
     => nil             

;; これも全部を除いた残りを返す。
(nthcdr 5 '(pine fir oak maple))    
     => nil                   

nthcdr も cdr と同様に元のリストを変化させないことに一言触 れておくほうがいいだろう。この関数もまた非破壊的なのである。これは setcar 関数や setcdr 関数とはっきり違っているところである。


7.4 setcar
この二つの名前から連想されるように、setcar と setcdr とい う関数は、リストの car や cdr に新しい値を持たせる。これら は car や cdr が値を変化させないのとは異なり、実際に元のリ ストの値を変化させてしまう。これがどういうことなのかを見る一つの方法は、 実験してみることだろう。まずは setcar 関数から始めてみよう。

最初にリストを作り、setq を使ってある変数の値にそのリストをセット する。動物のリストでやってみよう。

 	
(setq animals '(giraffe antelope tiger lion))

GNU Emacs の Info でこの文章を読んでいるなら、このＳ式をいつものように評 価してみよう。カーソルをＳ式の後に持っていき、C-x C-e とタイプする のである。(私はこう書く時には実際にこの操作を試している。これは計算機環 境にインタプリタが組み込まれていることの利点の一つである。)

変数 animals を評価してみれば、この変数が (giraffe antelope tiger lion) というリストにバインドされていることが確かめられる。

 	
animals
     => (giraffe antelope tiger lion)

別の言い方をすれば、変数 animals はリスト (giraffe antelope tiger lion) を指しているとも言える。

次に、関数 setcar に変数 animals と引用符付きのシンボル hippopotamus の二つ引数を渡して評価してみよう。これは三つの要素を 持つ (setcar animals 'hippopotamus) というリストを書いて、これを いつものように評価することで行うことが出来る。

 	
(setcar animals 'hippopotamus)

このＳ式を評価した後で、変数 animals をもう一度評価してみよう。す ると、動物のリストが変化していることが分る。

 	
animals
     => (hippopotamus antelope tiger lion)

リストの最初の要素 giraffe が hippopotamus に変わっている。

以上のことから分るように、setcar は cons のように新しい要 素を付け加えるのではなく、giraffe を hippopotamus に置き換 えている。即ち、この関数はリストの最初の要素を変更するものなのだ。


7.5 setcdr
setcdr 関数も、setcar 関数と似ているのだが、こちらはリスト の最初の要素ではなく、二番目以降の要素を置き換えるものである。

実際どのように働くかを見るために、まず次のＳ式を評価して、変数の値を家畜 のリストにセットしておく。

 	
(setq domesticated-animals '(horse cow sheep goat))

この時点でこの変数を評価したなら、リスト (horse cow sheep goat) が返されるはずだ。

 	
domesticated-animals
     => (horse cow sheep goat)

次に、setcdr に、このリストを値に持つ変数名とそのリストの cdr の部分を置き換えるリストの二つの引数を与えて、評価してみる。

 	
(setcdr domesticated-animals '(cat dog))

このＳ式を評価すると、リスト (cat dog) がエコー領域に表示される。 これがこの関数によって返される値である。我々にとって興味があるのは副作用 の方であるが、こちらは変数 domesticated-animal を評価してみれば どんなものか理解出来る。

 	
domesticated-animals
     => (horse cat dog)

このリストが (horse cow sheep goat) から (horse cat dog) に変化しているのが分る。リストの cdr が (cow sheep goat) から (cat dog) に置き換わっているのである。


7.6 練習問題
四種類の鳥のリストを幾つかの cons を使ったＳ式を評価することで 作りなさい。また、あるリストにそれ自身を cons した時に、何が起き るかを見なさい。四種類の鳥のリストの最初の要素を魚で置き換えなさい。更に、 そのリストの残りの要素も他の魚で置き換えなさい。


[ << ]	[ >> ]	 	 	 	 	 	[Top]	[Contents]	[Index]	[ ? ]

This document was generated by Matsuda Shigeki on April, 10 2002 using texi2html
