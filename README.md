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


































