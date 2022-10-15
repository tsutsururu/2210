# 2210

# 1002

#  186A Brick   diff 4

解答遷移 AC

計 01:23

備考

なし



# ARC122B ☆  Insurance   diff 729　　

解答遷移 AC

計 23:00

備考

➀ 思考

さっぱりわからない。実験によってそれぞれ x= Ai/2b となる xで損失が極小になることはわかったので、B問題だし、極小値へのベクトルが平衡になる場所 x が全体を最小化するってことで解答できそうだと判断(全く根拠なし)。あとはN が 偶数の場合だけ、平衡地点が2か所あるのでそれを比較していい方をだせば完了だな AC

* この考えが正しいとしても、Aの中央地点を Ai, Aj とすると、 偶数のときは Ai < x <Aj でベクトルは平衡になるが、どちらかに一致する場合にはむしろ平衡ならないと気が付く。本当にたまたまのACである。ABCと勘違いしたためにできただけ

➁ 解法

Nx - Σmin(2x,Ai) のグラフの傾きを考えると、Nが偶数の場合、例えばN=4のときは A2/2 < x < A3/2 において傾きが0になって極小値をとることがわかる( min()が含まれるためなめらかではないので、A2/2 および A3/2 の前後で傾きが急変する) Nが奇数の場合傾きが0になることはないが、例えば N=3 の場合 A1/2 < x < A2/2 において 傾き -1 , A/2 < x <A3/2  において傾き 1になるので　その中間の A2/2 において 極小値をとる。

したがって、x = A[N//2] で損失は最小になる。 ( Nが偶数のときは A[N//2-1] ～ A[N//2] を満たす任意の実数がxとして成立する)


中央値とindex番号のズレに注意



# 182D Wandering  diff 708

解答遷移 AC

計 27:57

備考

➀ 思考

途中の情報が必要なので、塁積和をとって途中の情報を消したりはできない。[ 現在地 , 最高到達点 ] と2次元で保存しながら更新していくのがよいと考えた。あとはどうやってこれらを更新していくかで、現在地から次の現在地まで行くのは累積和で簡単に求まる。よって最高到達点の更新方法だが、これには場所だけでなく、進み方の情報も必要であることに注目した。例えば 現在の最高の進み方が +5 だとしたら現在地 + 5 が最高到達点であれば、最高到達地点を更新する。また、進み方も随時更新され、累積和が最高の進み方を超えれば更新していけばよい。あとは更新の順番に気を付けて実装してAC


実際には20分ぐらいで完了できていたのだが、手計算による検算がうまくいかず提出までに時間がかかってしまった。




# 平衡二分探索木(順序付き集合)　ver python の勉強

C++には存在し、pythonには存在しないデータ構造 std::set のpython版 Sortedsetを扱う

・概要

https://qiita.com/tatyam/items/492c70ac4c955c055602

・関数など

https://github.com/tatyam-prime/SortedSet


簡単に言えば、リストに追加した後、通常よりも高速にsortしてくれるデータ構造である。


S=Sorted(イテラブル)で初期化することで扱える。基本操作はリストと同じ。S[index]で index番号にアクセスして要素を取り出すことができる

・ add(x)

要素の追加。set同様、重複していれば追加されない

・ index(x)

x 未満の要素の数を出力できる。






# ☆ 217C  Cutting Woods  diff 802

➀ 思考

長さ12の木材を位置7で切断したい場合、0,1,..,6  8,9,..,12 の2グループが生成される。これは最初に 0 始まり 長さ12 の木材を持っている状態で、0 始まり長さ7 , 8(7+1) 始まり長さ 5(12-7)の2グループが生成されたと考えられる。したがって、始点と長さを記憶していけばこの問題を解答できると考えたのだが、この次に 3 の位置で分断しようとした場合 位置3 がどのグループに含まれるかわからなければいけない。そのためには例えば始点を管理して二分探索するなどの方法が考えられるが、これにはこの操作を行う前に sort 処理を行わねばならず、全体で O(NlogN) かかるのでとてもクエリ全てをさばききれない。この 位置x がどの始点からはじまるグループに含まれるかのボトルネックを解消するには、おそらくこの処理に適したデータ構造が必要だと感じ、それを知らない今は降参するしかないと判断した。

結局 平衡二分探索木が必要だったので、思考自体は◎


➁ 解法

7の木材の位置3で分断する際、 [0,3] [4,4] と考えたが、実は切れ込みの位置(位置0,7を含む) を管理さえすれば長さを管理しなくても、前後の要素の差でこれが求まる。　例えば 0,3,7 のとき、位置2を含む集合の長さは 3-0 で 3とわかるし、 位置5では 7-3=4 で長さが4とわかる。


③ 入力改善   input = sys.std.readline

仕組みはわからないが、これで提出時間が半分になった。これを宣言するだけであとは通常通りなので、大量の一行入力(for で処理するやつ) の場合は脳死で使った方が良さそう

https://qiita.com/kyuna/items/8ee8916c2f4e36321a1c




#  253c  Max - Min Query      diff  518

解答遷移 AC

計 17:00

備考

➀  思考

query3 に際して、最大値と最小値にアクセスする必要があるが、挿入される値は単調に増加したり減少したりしないので、今持っている値すべての順番を記憶しておかないと、高速なアクセスができない。挿入毎にsortするわけにもいかないので SortSet を使用することにした。しかし今回は同じ要素を個数を含めて管理しなければならず、SortMultiSetを使うのかなと考えたが、値の削除が面倒そうだったので、個数の管理は別でCollectionで行い、空になったらSortsetから削除するという処理を施せばよいと判断。 


➁　別解

https://atcoder.jp/contests/abc253/submissions/32056927

collections.Counter + 優先度付きキュー　で最先端の個数が0だったら、popする処理を繰り返すことで、最小値、最大値に高速にアクセスする。

注意点は２つ。一つは最小値だけでなく、最大値も必要なので 2つのheap構造が必要になること、また最大値をとりだすために、-xを格納しなければいけないこと。2つ目は、heappush(Xmin,値) として、明確に最小値 heap　と最大値 heapのどちらに値を格納するか示さなければならないこと。SortSetの方がはるかに楽



#  ☆  170D  Not Divisible   diff 1033

降参

備考

➀ 思考

Aiについて約数列挙し、Aにその約数が存在するか 積集合をもちいて判定する処理を考えた。最大計算量は 要素が 10^6 ～ 10^6-2.0* 10^5 のN個のときで、積分することでこれを求めると 1.8* 10^8 でギリギリ通せるかと判断して提出 TLE 。。。

なお普通に重複処理をうまく行えなかったようで　WA も出した。➁で示すように逐一積集合を求めずとも、約数全部をまとめて最後に検出すればこのWAは消えそうだ

➁ 解法

ポイントは以下2点

・列挙するなら 約数　より　倍数

A が B　の約数 ⇔  B は A の倍数 であることを利用する。前者は候補列挙に　√ Ai 回ループが必要で、後者は候補列挙に maxA / Ai 回のループが必要である。 ③でも述べるが、N要素における合計値 つまり積分した値は桁が1つ異なる程度に、後者の方が高速で処理できてしまう。 

・　重複処理

例えば A= 2 8 の場合、 8 のみが条件を満たすためには 列挙する倍数を 2Ai,3Ai...にする必要がある。Aiを含めてしまうとすべての要素が条件を満たしてしまうためである。 

しかしA= 2 2 8 の場合では2を含めないといけない。これを解決するために個数を管理して、最後の検出の際に、倍数候補に含まれない　かつ 重複する要素がないものを数えるようにしたらよいだろう。これならば A= 2,3,3,15,15 に対して 2は候補に含まれずただ一つなので、条件をみたし、3 は候補に含まれないが重複するので省かれる。15は候補に含まれるので数えらない。これで完了できる


③ log N   vs  √ N の積分

後者は結局 N ^ (3/2) になるため N= 10^6 とすると 10^9になるが、 logN はたかだか 10倍する程度なので処理性能が全く異なる。




# 1003  

# アルゴ式 データ構造

# 9-1  二分木の通りがけ順

備考

通りがけ順の定義通りに出力できるようにしたdfsを実装するだけ。具体的には子の情報を index番号か -1 で持つ。例えば [2,-1] で　左側に頂点 2の子を持ち、右側には子を持たないことを示すことにする。あとはふつうにdfsしつつ、右側の子方向に探索を始める前に自分自身の番号を保存すれば完了



# 9-2  二分探索木への挿入 (1)

解答遷移 WA WA AC 

計  09:19

備考

完全にsortされているものだと思っていたが、ルールに従い木を構築し、行きがけ順で出力すれば完了できる

・構築ルール

根頂点から順番に見ていき、
・挿入したいキーが今いる頂点のキー以下である場合は左側の子頂点へ進む
・挿入したいキーが今いる頂点のキーより大きい場合は右側の子頂点へ進む
という動作を繰り返します。
進む先の子頂点が存在しなくなったとき、
その場所に、挿入したいキーをもつ頂点を挿入します。



# 9-3 二分探索木への挿入 (2)






# 222A   Four Digits   diff 5

解答遷移　AC

計 01:08

備考

なし

# 221A Seismic magnitude scales   diff 10

解答遷移　AC

計 01:39

備考

なし

# 222A Find Multiple   diff 14

解答遷移 AC

計 05:19

備考

➀ 解法

B//C * C が B以下の最大のCの倍数なのでこれが A以上であれば出力し、そうでなければ-1 


# 068C  Cat Snuke and a Voyage  diff 609

解答遷移 AC

計 06:39

備考

➀ 思考

bfsで最短距離を調べ上げて、最後にNの最短距離が2かどうか判定




# 106C  To Infinity   diff 441

解答遷移 AC

計　12:47

備考

➀ 思考

5000兆 = 5* 10^18 。　2^60 ≓ 10^18 より 1以外の数がSの先頭にあれば K の値によらず その値が答えになる。したがって考慮すすべきは K が S の先頭に存在する1を示すか否かである。 これはSの先頭に存在する 1　の数をカウントしてその数が K 以上ならば 1が答えになり、それ以外であれば1の次に先頭に存在する数が答えになる処理で完了できると判断。

➁ 改善

K番目までSを前から全探索できないから、カウントするかとなったが、S全部をみる途中にK番目が存在すればそこまで見る処理はできるの。具体的には range(len(Sstr),K) とすればよい。



# 122C  Get AC   diff 700

解答遷移 WA AC

計 30:26

備考

➀　思考

クエリにO(1)で解答しなければいけないので、始点と終点にアクセスするだけでその区間におけるACの数を求めたい。よって尺取り法などではなく累積和を使うのが良さそうだと判断した。あとはSを前から順に探索し、文字列を構成していく。ACになればその地点の個数を+1して、それ以外なら文字列を空にしていく処理を行うことで完了できると判断。WA  →  何が間違っているのか見当もつかないし、愚直解も思いつかないので、ひたすらサンプル作ってテスト　→  運よくたまたますぐにカウントが正しくできていないケースを発見。 文字列が ACにならなければ空にしていたがこれでは AAC　がうまく処理できていなかったのだ。これを修正してAC



➁ 改善

Sの i文字目とi+1文字目で AC になっているか確認すればよい。



③ input.stdin.readline

![image](https://user-images.githubusercontent.com/109026838/193501818-3288c982-2503-4df5-a0fb-dedf78c61137.png)

文字列の場合だけそのまま読み込むと改行文字が含まれてしまうので修正が必要




#  ☆ 261D  Flipping and Bonus  diff 801

降参

備考

➀ 思考

dpだな。ただお金がでかすぎて2次元リストつくれない。それさえできればカウント数を追っていけば解けるのに。。。代案も見つからず降参



➁ 模範思考

i回目投げたとき Z 円持っていたとする。　すると i+1 回目に 裏が出れば Z 円のままであり、表が出ればZ + Xi+1 円になり、さらにここまでの連続表数 k がボーナス値ならばさらに Bk 円追加される。これがこの問題で生じる事象の遷移のすべてである。したがって、遷移を追うためには、1,今何回目なのか 2,何円持っているのか 3,何連続なのか の3つの情報が必要になる。ここで例えば 1,2 を行と列に、3を値としてdpすることも可能だが、サイズの制約で実装できない。そこで 1,3 を行と列にすれば実装できるなと判断して解答する

このdpにおける基本の思考の流れ、特に遷移状態を追うために何の情報が必要なのか把握することができていない。dpっぽいなー　→ こういうのいっつも回数お金の2次元リスト作ればいけるからそうしよー、とやってるから解けなかった。



# 248C Dice sum  2回目   diff  748

解答遷移 AC

備考

➀　思考

数列を構成する数列を前から順番に見ていくと 、i+1番目の数字は 1～M でかつ その数で総和を更新しても Kより大きくならない条件を満たすものである。したがって 1,　今前から何番目か 2,総和はいくつか　の情報を保持すれば遷移状態を追えることになる。また、総和、つまりK は K <= NM < 2500 なのでdpするための2次元リストを作ることは可能である。



# ☆ 197C ORXOR  diff 809

降参

備考

➀　思考

制約的にbit探索しそう。ただ、区間が2つとは限らないから違うのかな？　→ 最適化すると必ず2区間になるんだろうか？ 実験　→ １の偶奇で分類できそうだけど、複雑すぎてすべて追うのはきついし 2区間が最適かどうか結論づかない。 → 区間の数は最大でも N だからこれを全探索できないか？　→ でも 2つ以上箱作ってわけてくのきついしわかんねーな　降参

➁　解法

Aの要素間に仕切りを入れることで区間を表現すれば、仕切りを入れるかいれないかでbit全探索できる


③　反省

bit全探索は 2択の全探索　だから2区間が最適だと決めつけ、そうなることを示そうとし、時間も思考を浪費してしまった。いざ区間の数を探索しようとしたころには体力と思考力がなくなって十分な考察が行えず、解答もできなかった。


また仕切りで区間を表現することは便利なので反射的に思い付けるようにしたい


補足)

![image](https://user-images.githubusercontent.com/109026838/193567642-d6737281-325c-4271-824b-391519c9a19d.png)

この問題でも ^ と | を要素間に挿入し、 ^ ならば区間を区切り、 | ならば同じ区間とみなせばよい


# アルゴ式  dp 

# 1 最大和問題

解答遷移 AC

計 02:49

備考

➀　思考

bit全探索したいけど無理。 dpするにしても最大総和は目盛り的に無理だしな。。とりあえず実験。　→  普通に負の数無視すれば絵絵だけやん。実装　→　if 処理するのめんどいし、足して総和を更新しなかったら足さないをmaxで処理しよう　AC

* 結果的に意図せず dp になった

# 2 ナップサック問題

解答遷移 WA AC

計 15:34

備考

➀ 思考

dpしそうだし遷移状態を見る。 i+1番目の品物を選べるか否かは、i番目までの総和に依存する。また選んだ時の価値はi番目までの価値の総和とvi+1 になる。したがって、1,何番目か 2,それまでの重さの総和はいくつか、3,それまでの価値の総和はいくつか　の3つ情報で遷移状態を完全に把握出来る。価値の最大値を求めるので価値を要素にして更新していくのが良さそう。重さの最大値サイズのリストも作れるし。

* WA は選ばない処理を 選ぶ処理のif分の中に入れてしまったために生じた。選ばない方は一番先に処理してしまうのが良い。



# 3 部分和問題

解答遷移 AC

計 05:27

備考

➀　思考

bit全探索したいけど無理。実験 →　i+1 番目の処理は、i番目までの遷移に関係なく、選ぶ選ばないになってるな。1, 何番目か 2,それまでの総和　の2情報があれば遷移をすべて把握できるとわかる。求めるべきは総和Mになることがあるかどうかの存在確認なので、ある総和となる選び方があるかどうかFlagで管理するのが良さそう。Mもメモリを心配することのない小さな値である。



# 4 部分和数え上げ問題

解答遷移  AC

計 05:50

備考

全問と同じで、それに加えてその総和となる選び方の数も追いたいので、選び方の数を要素にすればよいと判断


# 5 最小個数部分和問題  

計 09:14

備考

baseは全問と同じ。あとは選んだ整数の数も把握したいので、選んだ数を要素で管理し、min処理すれば最小値がわかる。min理なので初期値はありえないでかい数を設定する


# 6  K 個以内部分和問題

備考

個数を管理し、K以下ならYes 


# 7 ☆  個数制限付き部分和問題

備考

➀　思考

遷移状態はこれまでと同じ。異なるのは同じ要素を重複して選べることだけだから、dp[i][j] が Trueなら dp[i+1][j+a[i]* k (<= b[i])] もTrueにする処理で良さそう。ただ計算量がとても気になる。例えば a=1 , b=10000 の場合は、True の要素 1つに対して 10000回消費するので、最高で O(NMb)になってTLEしてまうと思われる。→ 途中で一度でも Mになる選び方があればそこで打ち止めれば削減できそうだが、微妙。一応通すか　→ AC

➁　模範解答

dp[i][j] = i 番目までの整数の中から選んで総和を j とするときに含まれる A[i] の個数 とすることでこの問題を解答できる。jを前から探索して、 dp[i][j-a]<b なら まだ aを選ぶことができるので dp[i][j] = dp[i][j-a]+1 とできる。さらにdp[i+1][j]=0 になる。なぜなら i+1 番目までにおいて、a[i+1]を0個選んで総和 j にすることが確定しているからである。

* この問題ではそれぞれの個数にのみ制限があるので、このように解答できるが、もし個数の総数にも制限があったら例えば 個数の総数を管理するもう一つのdpを作成して同時に操作していくことで、一応解答できそう。つまり、Q6 とQ7を並行してやっていくイメージ



# 二分探索　勉強

単調性を持つ、または境界で真偽が2分する対象について、ある条件を満たす常に満たす範囲や境界の探索を行うアルゴリズム。

https://qiita.com/drken/items/97e37dd6143e33a64c8c#4-%E4%B8%80%E8%88%AC%E5%8C%96%E3%81%97%E3%81%9F%E4%BA%8C%E5%88%86%E6%8E%A2%E7%B4%A2%E3%81%AE%E9%81%A9%E7%94%A8%E4%BE%8B

探索範囲を (ng,ok] として mid = (ok+ng)/2　が 条件を満たす範囲に含まれるなら ok を更新し、そうでないなら ng を更新して、境界へ向かって条件を満たす範囲と、満たさない範囲を広げていく。最終的に境界を見つけることができれば、例えば条件を満たす最小値なら右端から広げてきた ok の値がこれを示し、条件を満たす最大値なら、左側から広げた ok の値がこれを示す。


https://www.forcia.com/blog/001434.html



# アルゴ式 二分探索

2-1 方程式を解く

解答遷移 AC

計 13:41


# 2-2 貯金 (1)

解答遷移 AC

計 12:27



# 2-3 最小の添え字

解答遷移 AC

計 05:40

備考

➀　思考

二分探索して A > B を満たすok 範囲 と満たさないng範囲を狭めていき、最終的に差が1になった瞬間全探索が終わり、ok範囲の最小値とはokそのものなのでそれを出力して完了

なお初期値は (ng,ok] のようにokは含めて、ngは含めないのがよいとみたので、ok=n-1,ng=-1とした。

![image](https://user-images.githubusercontent.com/109026838/193878360-993f6567-b1d2-4551-ac9f-9f195dee05bc.png)

****** 注意 *****

こんな雑に初期値を定めてはいけない

ok は条件を必ず満たす範囲であり、ngは条件を満たさない範囲である。たとえば A=[1] b=100　の場合 ok= 0 ( N-1 ) とはならない。なぜならA[0] = 1 > 100 とはならないからである。正しくは　ok=N ng=-1 とすべき。(ただしこの問題では A[-1]>b なので実は ok=N-1 としてもよい)



# 2-4 小さい数の個数

解答遷移  AC

計 09:36

備考

➀　解法

基本的には 2-3 と同じアルゴリズムであるが、 A < B の範囲を求めたいので、okを左側に設定する。また、Aのすべてが n より大きい、つまり条件を満たす範囲が存在しない場合を考慮して okの初期値は 配列外の -1 に設定する必要があることに注意。  


# 2-5 和が K 以上のペア

解答遷移 AC

計 07:17

備考

➀　思考

組合せの全探索は O(N^2)なので無理。Aiを固定すると、K-Ai 以上の要素の個数を求める問題となり、これはAを二分探索すれば解決するので、O(NlogN)で処理可能


# 2-6 重さは何番目？

解答遷移 AC

計 02:59

備考

copyして二分探索するだけ

# 2-7 貯金 (2)

解答遷移 AC

計 04:53

備考

➀　思考

n日後の貯金額は (n+1)n/2 でこれが初めてXを超えるようなnを求めたい。→ 二分探索で行けると判断。


# 2-8 ひもを切る

解答遷移 AC

計 12:37

備考

➀　思考

長さ t で分断するとすると、ひもL1からは L1//t 本、L2からは L2//t 本　作ることができる。この合計がK以上だった場合ok 範囲を広げる二分探索で tの最大値を求められると判断


# 2-9  九九の表 (1)

備考

➀　解法

・二分探索

ok=0,ng=N+1 として、数列 i,2i,3i,...において　Kがどの位置に存在するか二分探索すると　K以下の個数はok と求まる。

・ 数式変換

(i+1) * (j+1) <=K より i <= K//(j+1) -1  したがって min(i,N-1)+1　である行に含まれるK以下の個数が求まる。



# 2-10 九九の表 (2)










# 1004

# 224A Tiers diff 6

解答遷移 AC

計 01:23

備考

➀ 補足

name="tsuru" のとき name[-1000:]="tsuru" である。おそらく末尾と違って先頭は、0より前にアクセスしてもエラーにならず0にアクセスできるようだ。



# 225A  Distinct Strings  diff 12

解答遷移　AC

計 03:15

備考

➀ itertools.permuration

順列を生成。引数にはイテラブルとそこからいくつ取り出すかの情報が必要。返り値はその並び方。また、文字列も与えることができ、この問題はこの性質を利用することで簡単にとける


# 056C  Go Home  diff 731

解答遷移 AC

計 16:49

備考

➀　思考

dp。1, 時間　2,現在地 の2情報があれば遷移をすべて把握できる。しかし現在地をすべて管理するサイズのリストは作れない。 ほかに遷移に関わる情報を探す　　→  なし！！！　dp じゃないってこと？　実験　→ 時刻 t における最高到達点を Xt とすると、tまでに -Xt ～ Xt までのすべての地点に到達するルートがあることを確認。したがって、Xに最速で到達する時間とは、X<Xt を初めて満たす t となる。これは最高到達点の累積和を作成し、2分探索すればすぐに求まる。



# 245D  Polynomial division   diff 815

解答遷移 AC

計 42:03

備考

➀　思考

ややこしいから全部候補調べて、一致するの答える方針で行きたい。　→ B は最大 100項あり、係数は　-100 ～ 100 まで取りうるので 全探索する場合 約 O( 200^100 )かかるので絶対無理。実験してみる　→　はじめに B0が確定して、　次に ( C1 - B0 * A1 )  // A0 で B1 が求まる。　次は ( C2 - B0 * A2 - B1 * A1 ) // A0 にて B2 が確定する。　このように Bの係数は前から順に確定していき、その確定した値を利用して後の値も順に決まっていく。したがって Bを前から順に求めていけばよいと判断。 また、Bk を決定する際に B0 * Ak , B1 * Ak-1 ,....をいちいち計算するのは面倒なので、これらを Bi (0<= i <k) が確定した時点で随時計算してリストCC の　CCk　にでも累積しておくことで (Ck - CCk) //A0 で　O(1) 処理できると考え、これを実装してAC


➁　改善案 

そもそも Ck の 値 は Bk 計算時にしか使用しないので、CCなど作らなくても C の値を更新していっても全く問題ない。



# 　☆ 125D   Flipping Signs   diff 833

解答遷移 AC

計 1時間 over 絶対に本番で出てたら解けてない

備考

➀　思考

よくわからないので実験。　→ 前から順に決めていくと i+2 番目 に Ai+2 , -Ai+2 を選択することで Ai+2 が確定して i+1 番目までの総和も確定することがわかった。よって、1, 前から何番目か　2, i-2 番目までの総和   3,i+1 番目 が Ai+1 , -Ai+1 のどちらか　の3つの情報によってすべての遷移を把握できることがわかった。→ しかしAiにあわせたサイズのリストを作成することは不可能である。ここで例えば、 10(総和) - 4 - -8 の選び方と、 -10 - 4 - 8 の選び方は以降同じ遷移をし、必ず後者の総和が大きくなるので、この時点で前者を捨てることが可能なのですべての総和を記録する必要がないとわかる。よって行には番号、列にはAiの正負(つまりサイズ2) を割り振ったリストで要素(総和)を更新するdpを行えばよいと判断。

ここまでの考察にも時間がかったが、さらにここから index番号と、遷移の連動がうまくできず死ぬほど時間をかけてなんとかACできた


➁　考え方整理

まず i+2 番目に Ai+2 , -Ai+2 を選択した時点で i+1番目までの総和は確定するので、 -1ずらして考え、i+1番目の選択で i番目までの総和が決定すると考えてよい。するとdpにおける、i行目の処理は Ai+1 を選択し、i番目までの総和を確定する処理であることを理解しやすい。また、2番目,N番目を決定する際の処理が、3 ～ N-1 のmax処理と同様に行えないことに注意。


③ 別解

- が偶数ならば必ず全て正の数にでき、奇数なら任意の一か所を選んでそれ以外すべてを正の数にできる性質を利用




# 1005

# 218A    Weather Forecast  diff 7

解答遷移 AC

計 01:08

備考

なし

# 219 AtCoder Quiz 2  diff 6

解答遷移 AC

計 02:31

備考

なし

# 230C  X drawing  diff 566

解答遷移 AC

計 35:38

備考

➀ 思考

よくわからないので、実験　→ 1の操作で (1,1) ベクトル方向&逆方向のマスが、2の操作で(-1,1)ベクトル&逆ベクトル方向が塗られる。これは素直にkを探索することで求められる。あとは出力範囲の該当マスのみに対して操作したくて、全範囲だとA,Bが大きすぎる時ヤバいし無駄だなので。　→ A+kとしてありえる範囲を探索し、随時そのkを用いた B+k (B-K)が範囲内に存在するか確認してそうなら塗る操作が良さそう。　→ a+kを探索しているのでB+kの存在確認をするときに Aを引くことを忘れずに行ってAC

➁　別解

P<=i<=Q , R<=j<=S を探索した場合、操作１で塗られるマスでは、i=A+k j=B+k が成り立つことになる。ここからkを消去すると i-j = A-B となるので、A=B に一致するi,jのマスを塗ればよい。操作2の条件でも同様に i+j = A+B で考える。なお、minやmaxはつまり、1 ～ N 以下であることを保証するものなので、より狭い i,j を探索している状況では気にしなくてよい



#　☆ 160D  Line++  diff 879

解答遷移 WA AC

計 1:42:36

備考

➀　思考

普通の経路での最短距離と、XY連結経路での最短経路を比較すれば最小距離を得られると考えた。制約的にも素直に始点を全探索して毎回bfsしても間に合うので。　→  しかし、iとi+1がつながっているという表現につられたのか、頂点は前から順に探索するだけでよく、戻ることはないと考え一度始点になった頂点を探索しないbfs をしたり、戻らないので、X-1とXはつながっていない方がいいいいと考えて分断したりと、随所で誤りを起こし、サンプルと合わず苦戦した。　2-3-4...-7 (X=3,Y=7) などでは4-7間の最短距離は 4→3→7　と辿る場合で頂点３に戻る必要があるので戻ることもあるし、分断もゆるされない。

➁　別解

1-2-..-3(X)..-7(Y)...-10 の経路において、1から10へ行くには 1, 素直に1-2-...10と辿る 2, 1-X-Y-10 と辿る2つの方法がとれる。前者の経路は距離 10-1=9 であり、後者の経路は |10-Y| + |1-X| である。したがって、始点と終点を全探索しこれらを比較して最短距離を管理できれば bfs することなくこの問題は解答できる。




# ☆ 154D Dice in Line  diff 485

諦め

備考

➀　思考

よくわからないので、実験　→  めちゃめちゃ煩雑だが期待値の和は求められる。ただ区間の期待値はわからない。一応 累積和の様に x番目の期待値から y-1 番目の期待値 を引くことで、y-x区間の期待値に一致しそうな気がしたので、保証はないがそれで解答することにした。　→ TLE  → 自分の案だと期待値の合計の分子まで記憶する必要があり、大きすぎる数字を生成する可能性があるのでTLEしたのだろう。 →　改善案として、区間がずれるたびにすべての期待値を引いていく処理を考えたが、これではループが増えてO(NK)になって結局TLEしてしまう。ここで降参 


➁　解法

![image](https://user-images.githubusercontent.com/109026838/194077941-2ef99a73-93c7-4bea-a14b-d188e864c5db.png)

https://manabitimes.jp/math/698

独立な確率の和の期待値は、期待値の和に一致する性質を利用。尺取り法で連続k区間の和を求めて最大値を更新して完了できる　




# 1006

# 216A Signed Difficulty  diff 17

解答遷移 AC

計 03:36

備考

なし

# 217A Lexicographic Order  diff 21

解答遷移 AC

計 02:02

備考

文字列どうしは比較できる


# 232D Weak Takahashi  diff 539

解答遷移 WA AC

計 49:44

備考

➀　思考

dfsして、seenの合計で一瞬だな。→ 戻ることができないんのでこれではだめ。→ だったらbfsで最短経路を記憶して、その最大値求めた良さそう。　→ WA →本当になぜ WA なのか不明なのでサンプル作って確認することに　→ なんと処理自体は全く正しいが、max(max(dist))で正しく最大距離が出力されていないだけだった。ので仕方なく、全探索してans更新することに


➁ 多次元リスト max 

numpy のようにはならない。

https://qiita.com/recuraki/items/d4a2e5d5accdbb90321e



# 111C  /\/\/\/  diff 826

解答遷移 AC

計 58:01

備考

➀　思考

偶数番目と奇数番目の出現個数を別々で管理すれば簡単に解答できそうだと考えた。問題は最頻値の要素が一致した場合であり、しばらく考えて、これはどちらかの2番目の最頻値のうち大きい方の要素をその代表値として、もう一方は1番目の最頻値の要素を代表にすればよいとわかった　→ ここですべての要素が同じ場合の処理を施していなかったことにサンプルテストで気づき、修正してAC

正直サンプルが不親切だったら、それまでにとても時間がかかったこともあってAcできていなかったかもしれない。


➁　改善

どちらかの要素がすべて同じ場合の条件分岐が面倒なので、[0,0]を追加すればよい



# 212D  Querying Multiset   diff 775

解答遷移 WA AC

計 11:49 + 05:00

備考

➀ 思考

優先度付きキュー使えば解決しそう。query2だけめんどいけど、累積する値を記憶しといて、出力時に足せば良さそう　→ WA → よく考えたら 12 , 13 , 22 , 13  の順に操作したら中身は 3 4 5 になるが、自分の処理では最後に累積した後に追加したボールにまで累積値を追加してしまう。これを回避するために追加するたびに、その時点の累積値を引いた値で追加しておけば順序も正しいし、最後に現時点での累積値を追加できるのでこれで良いと判断。AC




# 141D Powerful Discount Tickets   diff 823

解答遷移 AC

計 40:36

備考

➀　思考

二分探索しそうー(というか使いたいだけか) 。ただ商品ごとに割引券使う枚数変わるのでしんどそう。　→ 素直にdpするか。 1, 前から何番目か、 2, 割引券残数 3, i番目までの総和 がわかれば遷移状態を完全に把握できる。行列サイズは (前から何番目、残数)にするのが良さそう。M<10^5 で目盛り的にもよい。 → ただ i番目に残数m の場合 dp[i+1][m],dp[i+1][m+1],...dp[i+1][M]まで更新する必要があり、これではO(NM^2)で処理不可能。　→ 貪欲法か？　→ 一番お得なのは X -　X//2 が最大のもの、すなわち 最大のXに割引券を使用するのが最適である。 したがって、優先度付きキューで常にO(1)で最大値X を取得できるようにして、X//2 を追加していけば解答できると判断。AC

➁ 注意点

最大値を取得するために、負値を格納する必要があるが、// はガウス、つまり負の範囲では小数点切上げになってしまうので、int()にするか、正の数に直して演算しなければいけない。intは小数絡むし、正数変換する方が良いか。



# 061C Big Array  diff 808

解答遷移 AC

計 09:46

備考

➀　思考

SortedSet か　優先度付きキューで管理すれば良さそうだなー →  個数も必要だから優先度付きキューにしよう。あと既に存在する要素は、個数を追加したいな　→ それならその要素にアクセスしたいが何番目に存在するかわからないから不便だな。　→　普通に 1～ 10^5 までの個数を管理すればいいだけやん！　→ 前からremを更新していって初めて0以下になる場所を出力して完了



# ☆ 193D Poker  diff 866

降参

備考

➀　思考

普通に#としてありえる候補を全探索すれば解けそう → サンプル１の場合 10/25 になって 4/9にならない。何がおかしいのか30分考えてもわからず降参

➁　解法

例えば、5と5を配る確率は、2/25 * 1/24 になる。5と9では (2/25)^2 になるので25通りの選び方は同様に確からしいわけではないのだ。したがって、条件を満たす選び方が10通りあるので、10/25 にはならないというわけである。

これまでは選んで戻したりして、全体の確立が変らず常に同様に確からしい確率の範囲で解答できる問題にしか出会っていなかったが、以降注意したい。




# 189D Logical Expression diff 769

解答遷移 AC

計 30:19

備考

➀　思考

bit全探索したいけど無理。dpでいけそう。1, 前から何番目か 2,xi+1  3,yi の情報ですべての遷移を把握可能。 → x,y の2種類が存在すると2次元では管理しにくいので 3次元に拡張し、 axis=0 で　xi+1の種類で分類し、axis=1 に前から何番目か axis=2 に yi の個数で管理することにして最終的に dp[0][-1][0]+dp[1][-1][0] を出力してAC。


➁　別解

xi+1 = True かつ Si = OR の場合、yiの値に関わらず yi+1 = True になる。これを満たすのは 2^i 個。Xi+1 = False の場合は yi=True の必要があるので Si = OR の場合、yi+1となる並び方の総数は合計で 2^i + (yi=True の総数) となる。 Si = AND の場合は xi+1 かつ yi = True でなくてはいけないので、総数は (yi=True の総数となる)。

つまり S[i] = OR の場合 yi=True の個数を 2^(i+1)個増やしていくだけでこの問題を解答できる。ANDの場合は増えないので何も処理する必要がない



# 1007

# 214A New Generation ABC diff 4

解答遷移 AC

計 01:25

備考

なし


# 215A  Your First Judge   diff 7 

解答遷移 AC

計 01:17

備考

なし



# 250C Adjacent Swaps diff 517

解答遷移 AC

計 55:13

備考

➀　思考

ふつうに入れ替えれば良さそう。→ 操作後の任意のボールの位置を把握できていないといけないことに気が付く。これはボールの位置を管理するリストを作成し、入替で1増やしたり減らしたりすればよい。 → サンプルで確認。合わない　→ 入替先のボールは位置リスト内の前後に存在するわけではないことに気が付く。入替先のボール何なのかはindex()などで得ないといけないが、これではO(N^2)かかる。→ そこで ある位置に存在するボールが何かを管理するリストも追加で作成すればよいことに気が付く。これで完了


# 169B Multiplication2 diff 352

解答遷移 AC

計 07:53

備考

➀ 巨大数計算量

N ( 10進数表記桁数 n10 ,2進数表記桁数 n2) 
 

・四則演算

掛け算なら O( n10 log n10 )

![image](https://user-images.githubusercontent.com/109026838/194547231-ce83926b-abca-45b4-b513-3eedd1070325.png)

https://qiita.com/square1001/items/1aa12e04934b6e749962#2-2-%E5%A4%A7%E3%81%8D%E3%81%AA%E6%95%B0%E3%81%AE%E8%B6%B3%E3%81%97%E7%AE%97

・累乗

O(log2 n2)

![image](https://user-images.githubusercontent.com/109026838/194549272-e3433336-fceb-43ec-a821-33b5daeeae92.png)


したがって例えば 2^(10^7) を素直に計算するのは難しい(238A 07) が、この問題の様に 10^18 * 10^18 程度なら18 log 18 なので余裕で処理可能


➁　解法

➀で述べたように 10^18 までの計算程度なら余裕で処理可能なので、普通に掛け算を行って、10^18を超えれば -1 出力する処理で解答可能。ただし、10^18を超えたあとに0が登場する場合が例外になってしまうので、昇順にするなどの工夫が必要



# 139D ModSum  diff 397

解答遷移 AC

計 11:14

備考

➀　思考

制約が強すぎて、探索すらできない。とりあえず実験しよう。 → 1,2,3,4 ... N それぞれについて最後のN以外は -1 した値が余りになる可能性があり、かつ 2,3,4,5...N-1,1 とすればすべて最大の余りにできることがわかった。また、NになにXを割り当てても、最大でもあまりはX-1 にしかならず、これは X-1 にXを割り当てた場合と変わらないので、Nに1以外を割り振ってより得をすることもない(現状維持が最大)。 したがって、 2,3,4,5...N-1,1と割り振って余りが 1,2,3,...N-1,1 となる場合が最大値となる。


# 1008

# 212A Alloy diff 5

解答遷移 AC

計 03:44

備考

なし


# 213A Bitwise Exclusive Or diff 33

解答遷移 AC

計 01:58

備考

➀　XOR 演算

A XOR B = C  ⇔  A XOR C = B ⇔ B XOR C = A 


# 255C ±1 Operation 1   diff 574

解答遷移 AC

計 29:55

備考

➀　思考

As = A , Ae = A+D(N-1) (数列の末端)とする。 

等差数列をすべて管理できれば二分探索で近傍にアクセスして差をとればいいのだが、Nがでかすぎて無理。　→ 
等差数列の値は必ず Dでわった余りがAであることに注目して X%D との差をとればよいことを思いついた。あとは Xが数列の範囲に存在しない場合だけ別で処理すれば完了!! → サンプル3 があわない。ここでようやく各値が負の範囲で動けることに気づく。→ D<0 の場合だけ割り算が成立しないし面倒すぎるので、これをうまく処理できないか考察　→ 数列をひっくり返せば、つまりAs = Ae , Ae = As とすれば D=-D としてDを正の範囲で処理できることに気づく。これで完了

➁ MOD 演算の基本

A>X や A<0 を考慮して (A%D-X%D)%D としてが、最終的にmodをとるなら、個別でとる必要はない。


③ 二分探索

リストを作ることはできなくても二分探索はできる。例えば、1 3 5 7 9 11 の数列に対して  ok=0 ng=6 (項数 6) として 1+mid* 2 (公差) <= K とすれば何項目まで K以下なのか求めることができる。


# 255D ±1 Operation 2  diff 788

解答遷移　AC

計 35:19

備考

➀ 思考

各クエリに対して、各要素に何回操作が必要か求めるのは無理。→ 足りない分をベクトル演算できそうだと考えた。例えば 6 11 2 5 5 に対して、 X=20 なら 14 9 18 15 15 回ずつ必要だが、各要素どうしのベクトル列 0 5 -4 -1 -1 のそれぞれについて 20-6 =14 との差が 14 9 18 15 15 に一致することを利用できないかということである。確かに X<=2 , 11<=X の範囲であればベクトルの符号が全要素同じなので、このような演算で回数に一致させられるが、 2<X<11 の範囲ではこの演算では回数に一致させられない。　→ 2<X<11 に注目。数列をソートすると二部探索でき、その境界前後でXに一致させるのに + する必要があるのか、 - する必要があるのか変化することがわかった。したがって、境界前後の総和に対してそれぞれの個数分 X をかけた値との差が求める回数になると気づいた。区間和は累積和でO(1)処理して完了



# ☆ 263D  Left Right Operation   diff 1016

降参

備考

➀　思考

累積和をとって、どこまでで変換すればいいか探索する方針を考えたが、うまくいかず降参した


➁　解法

Aを前から探索して、i番目までの総和の最小値を求めることにする。例えば 1,100,1,100...　,L=10 の場合、1番目までの総和は 1 or 10 なので  1。 2番目までの総和 は 1番目までの総和 + 100 or 2L で 20 と求まっていく。末端からの操作も同様に考え、N番目から k 番目までの総和の最小値を求めていき、最終的に 先頭から l 番目までの総和の最小値と 末端から l+1番目までの総和の最小値において、lを1～Nまで動かしたときの最小値が答えになる。

③ dp

Aを前から探索して、-1番目が RならR、Ai-1ならAi またはR、LならL,Ai,R と求まるので、i, 前から何番目か 2,i番目までの総和 3,i番目が L,R,Aiのいづれか　の情報で遷移を完全に把握できる。これで最小値を求める

④　反省

1,100,1,200,1.... の場合に 100 と 200 のところまでは進めたいから、、、と初めから条件を設定して、それを満たせるように処理を考えるのはナンセンス。

困ったらとりあえず前から探索する。 




# 272

# D Root M Leaper

備考

➀　平方数列挙

x^2 + y^2 = M を満たす非負整数(x,y)の組みを求める。

0<= x <= M の範囲において、ok=0 ng >M と設定して求めた mid の2乗 が M-m 以下であれば ok を、そうでんばければngを更新していく。これによって ok^2 <= M-m になるが ok^2 -= M-m であれば x^2 + y^2 = Mを満たす(x,y) = (m,ok) が成立する。 

また、例えば 1^2 + 8^2 = 65 であるが、 4^2 + 7^2 = 65でもあるので、一組見つかっても終了してはいけない。m^2>M になって初めてbreak可能


https://yatt.hatenablog.jp/entry/20130128/1359370204


➁ WA

(1,1)から移動できない場合の処理がうまくできていなかったことが原因。しかしこれだけで WAが10個モデルとは思えずメインの処理に理由があると思い込んで時間を迎えてしまった。一つでもサンプルを与えていれば見つかっただけに悔しい。



# 1009

# 210A Cabbages  diff 19

解答遷移 WA AC

計 05:02 + 05:00

備考

1, A>N の場合を忘れない

2, WA食らう方が痛いので、かっこつけてmin,maxで条件分岐なくすより、素直に全網羅したほうが脳死で間違えずかける気がする



# 211A Blood Pressure   diff 6

解答遷移 Ac

計 01:11

備考

なし



# 256D Union of Interval  diff 546

解答遷移 WA AC

計 38:32 + 05:00

備考

➀　思考

最終的な連続区間の始点と終点がわかれば解答できる。これは、L,R を区別するために X[L]は+1,L[R]は-1 して、初めて0を超えた場所を始点、その後カウンターが初めて0になった場所を終点とすればよいことに気づいたが、WA → なにが間違っているかわからず、もたもたサンプルを作成した後、カウンターをX[i]の値で増減していないことに気づいて修正 AC

➁ 修正 と imos法  * 類題 183D

カウンターをわざわざ作成しなくとも、累積和をとれば簡単に始点と終点がわかる。


③ サンプル作成

まず f.write()で書き込めるのは文字列だけ。

書き込み時、print(f.write(文字列))とすると、適当な数字がターミナルに出力されてしまう。printしなくとも書き込みは成立する。全く不要





# 221D Online games  diff 832  2回目

解答遷移 AC

計 30:43

備考

➀ 思考

ログイン時に +1 して、ログアウト時に -1 して時間を前から探索すれば解答できる(imos法)。しかし、これを管理するリストを作るにはメモリが足りないし、そもそも探索することもできない。　→  現在何人ログインしているかを持っておきながら、ログイン、ログアウト時のみを探索すればO(2N)で人数を把握できそうだと判断。ログイン時とログアウト時が被ってもその間の時間は0なので、人数カウンターをその分インクリメントしても問題ないことに注目。ログイン、ログアウトをともにソートして、前から探索　、探索のたびに前回の探索時の時間から現在時刻を引いたものが現在人数から変化せずログインされ続けた時間になる。その後、現在時刻を更新し、ログインなら人数を増やし、ログアウトなら人数を減らす処理で完了


➁　別解

collection.Counterでログイン、ログアウト時を管理する。これによって 2Nのリストから、2N以下のカウンターにメモリが削減されるし(重複時)、ログインか、ログアウトかをカウンターの要素として記憶できるので、処理も簡単になる。


# 158D String Formation  diff 610   類題 199C 

解答遷移 AC

計 13:36

備考

➀ 思考

ボトルネックは文字の反転であるが、ほかの操作をいじることで擬似的な反転を実現させたいな　→ T=1 になるたびに反転flagを入れ替えて、T=2の場合に反転フラグが立っていたら、文字列が反転していると考えて挿入位置を入れかえればよい。また、反転フラグの入れ替えはXOR演算でflag^=1とすれば　1なら0に、0なら1に場合分けせずに統一できる。さらに、挿入位置の入れ替えも、(quuery[1]-1) ^ flag とすれば、flagが立っていない場合にそのまま query[1]-1の値が得られ、falgが立っていれば0なら1が、1なら0が得られる。


# 199C IPFL  2回目 diff 436

解答遷移 AC

計 12:45

備考

➀　思考

疑似的な入替で高速化したい。→ 入替flagで状態を管理して、入れ替えている状態は index番号が +N %2N であることを利用し、AとBを入れかえればよいな


# 258C Rotation  diff 419

解答遷移 AC

計 05:49

備考

➀　思考

取り出すindex番号を、現在のindex番号に合わせられたら O(1)で処理できるな。→ t=1 のxを累積すると、(x-1-tmp)%N が取り出す位置になることに気づいて完了



# 168A ∴ (Therefore) diff 12

コード作成なし

備考

➀ 1の位の出力

1, %10 

2, 文字列 S で受け取り、S[-1]



➁ 分類方法

1, 数字 → リスト内検索 ex) s in [0,1,6,8]

2, 文字列 → 文字列内検索 ex) s in "0168"  * 一桁でない場合では注意が必要




# 1010 

# 158B Count Balls  diff 123

解答遷移 AC

計 03:08

備考

なし



# 188D Sunuke Prime  diff 933

解答遷移 AC

計 53:53 + α (スタート押し忘れ)

備考

➀ 思考

imos法使いたいけど、制約的に無理だから、221D方針で行こうと考えた　→　しかし1日しかサービスを利用しない場合など ex)  a a c   のような場合で うまく処理できないことに気づいたので、開始日を 2a 終了日を 2b+1 と開始と終了を同日でも区別できるように調整した。また、切り上げ演算すればこの調整があっても正確に継続日数を出力出来ると判断した。　→　サンプル 2,3が微妙にあわない。　適当なサンプルでデバック　→ 例えば 1 2 6 , 1 100 10,  3 4 7 では、1 , 4 , 5 , 8, 200 の時間になるが、 4 と5 の間は日の間であるので、この区間では料金は発生しないはずである。しかし自分の処理では 100円のサービスを 1日することになってしまう。これを回避するために、探索時に前回時点が終了時だったら、それを+1することで全部丸く収まってACできた。

➁　改善

方針自体は完全に同じだが、開始日を -1 すれば時間軸を調整せずに済む。これはi日目の始まりを i-1 ,終わりを i　とすることと同義であり、これによって同日であっても終了時から開始時を引くことで1日を出力できるようになるし、自分の実装で問題になった日の間もなくなる。



# 235D Multiply and Rotate  diff 862

解答遷移 AC

計 38:43

備考

➀　思考

1, 何番目か 2,i番目の末尾の数　ですべての遷移を把握で気はするが、番号の定義がわからないし、そもそも何回目まで見れば良いのかわからずdpではなさそうだと判断。→ 入力された数字を操作した後に出力される数字の桁数は入力数字の桁数以上であること(小さくはならないこと)、N<10^6 であることから、素直にbfsで探索できそうだと判断。

➁ 注意点

文字列が絡む条件分岐には最新の注意が必要。Xが10で割り切れないとは

1, X%10!=0

2,str(X)[-1]!= "0"  ( !=0 ではない)


# 130D Enough Array  diff 865

解答遷移 AC

計 12:06

備考

➀　思考

負の値がなく単調増加する連続区間だから尺取り法使いそうだな。でもK "以上" か.. → 一度でもK以上になったらそれ以降の要素分答えに追加する処理で問題なく完了できそうだとわかり完了

# 066B ss    diff   384

解答遷移 AC

計 06:17

備考

➀ 思考

普通に毎回末端2文字削除して条件を満たすかどうか判定すれば良さそう。2文字の場合は2文字減らさずに答えが1になるので、減らし続けて2文字になった場合にこの処理を行えるような実装をして完了




# 1012 

# 208A Roalling Dice   diff 28

解答遷移 WA AC

計 06:30 + 05:00

備考

dp的に考えればわかるが、A回ふった場合 A　～ 6A の範囲の任意の整数をとりうる。


# 209A  Counting  diff 5

解答遷移 AC

計 01:22

備考

なし


# 161D  Lunlun Number  diff 991

解答遷移 AC

計 37:58

備考

➀ 思考

実験によってi+1桁であって条件を満たす数のうち先頭がxのものは x + (i桁であって先頭が x-1,x,x+1 のいづれかである条件を満たす数) として表せることがわかったので、1, 何桁目か 2, 先頭の数字 3, 個数 この3つの情報で遷移を完全に把握できると判断した。　→ 1を行に、2を列にすればよさそうだが、問題は何桁目まで調べればいいかわからないことである。 K<= 10^5であり、実験によって 10^4桁目まで行く前に K 番目の数は見つかると判断したので(証明などはない)、K番目まで条件を満たす数を全探索する方針に。→ while ループで随時dpにappendしていき、最新の行を取り出して操作していくことにした。また、2桁目以降0始まりで条件を満たす個数をカウントこそしないが管理する必要があったので、0～10までの長さ11のリストを用意することにした。これによって 0 始まりの場合だけ別で処理すれば、頭が9であっても前の行の -1,+0,+1 のインデックスにアクセスして個数を累積する処理でまとめられるようにした。ただし10は更新しない → ここまでで個数を完全に管理できるようになったが、K番目の具体的な数を求める必要があるので、
個数に加えて、条件を満たす数そのものも記憶する処理を追加した。具体的には、dpの要素として[ 個数 ,[ 条件を満たす数の文字列]] とした。　ただし9始まりの場合 8,9,10の個数を累積することは許されるが、文字列の更新時は　10に格納された意味のない文字列を反映させたくないので、10の初期値を[0,""]として,
文字列を更新するのは空でないときという処理を追加することで完了できた。


➁　別解

1,9を前から順に探索し、1を取り出して10,11,12 と作り、2を取り出して21,22,23と作り、これをK個に到達するまで繰り返すことで解答できる。正整数は取り出した数をXとすると、10X+X%10 +(-1,+0,+1) となる。X%10=0 or 9の場合に注意


# 1013

# 206A Maxi_Buying   diff 5

解答遷移 AC

計 02:57

備考

なし

# 207A Repression  diff 5

解答遷移 AC

計 01:20

備考

なし


# 145D Knight   diff 1099

解答遷移 WA 降参

計 1時間 over

備考

➀ 思考

1, 何回目か 2, 到達個数 ですべての遷移を把握できるのでdpしようと考えた。2次元空間に対応すべく axis=0で回数を管理しようと考えたが、そうなるとO((10^6)^3) になるのでできない。　→　次にbfsで(X,Y)までの経路を知らべ用途考えたが、こちらもTLEすると判断した。最終的に bfs + dp の複合合わせ技で突破しようとしたが、やはり厳しい　→ ここで冷静に状況を整理するため前から実験。すると (1,2) ,(2,1) ベクトルの個数は(X,Y)で一位に定まることを発見した。それぞれを a回、b回とすると答えは a+b C a になる。combではTLEになりそうだったので逆元の考えを思い出して解答。サンプル２からマスの合計は3の倍数である必要があるのでこれを満たさないものをあらかじめ取り除く処理で完了。。。かと思ったが2WA 。結局原因がわからず降参

➁　解法

解法は完璧 (300,3)など 3の倍数かついけないマスがある。a,bの探索時、すなおにX,Yのどちらにもあてはまる条件処理を施せば回避できた。

③ 逆元計算

nCm は 1～mまで探索して m*(n-i+1)%MOD を計算すればよい。


# 264D  "redocta".swap(i,i+1)  diff 414

解答遷移 AC

計 37:51

備考

➀　思考









272D

メモ dx,dyは√M以下なので全探索可能。さらにNを超えるとマス内部を移動できないことに気づけば探索範囲は√Nになる



# 1015

# 174A   Air Conditioner   diff 7

解答遷移 AC

計 01:02

備考

なし



# 174B  Distance  diff 43

解答遷移 AC

計 02:51

備考

距離が D　なので D^2  を比較する。


# ☆ 152D Handstand2  diff 1045

降参 → AC

計 1時間over(本番では絶対間に合わない)

備考

➀　思考

N以下の正整数をすべて探索し、その数の末尾から始まり先頭で終わる数を桁数ごとに求めることができると判断。最高でも6桁なので　O(6N)　となり十分高速。　

しかし場合分けが非常に煩雑で、死ぬほど時間がかかったうえに、一度諦めた後再度考えてAcできた。

➁　模範思考

この問題で重要になるのは、先頭と末尾である。したがって、A=1,11,101,111,...1001,1011,.... がすべて同じものとして管理できれば数え上げが簡単になりそう。→　直接 (先頭,末尾)の状態で管理すればよい。その後N以下の整数を全探索して、それらについて(末尾,先頭)の個数が条件を満たすBの個数に一致することを利用して数え上げる。

# 172D

解答遷移 AC

計 46:21




# 116C Grand Garden

解答遷移 AC

計 17:38

備考

➁　別解

https://yamakasa.net/atcoder-abc-116-c/




# 133C Remainder Minimization 2019  diff 592

解答遷移 WA AC

計 13:37

備考

➀　思考

[L,R] に2019が含まれていれば簡単。これはL//2019 と R//2019 が一致しなければ必ず2019をまたぐと考えて判定することが可能。含まれる場合は。L,Rを2019の余りに変換し、2019=3* 673 から 3と673が...と考えようとしたがとてもめんどくさそうだと感じた。ここで2019までならi,jを二重ループで探索できることに気づいてこれを実行しAC

* なお 1WAは for j in range(L+1,R+1)としてしまったことが原因。正しくは i+1 である


# 258D Trophy

解答遷移 AC

計 13:00


# 145C Average Length  

解答遷移 AC

計 09:59


# 269C 

解答遷移 AC

計 17:11


# 201C 

解答遷移 WA AC

計 32:21


# 112D

解答遷移 AC

計 12:50












