## Table of Contents
* [基本方針](#section-0)
  * [概要](#section-0-0)
  * [目的](#section-0-1)
  * [使い方](#section-0-2)
  * [原則](#section-0-3)
* [命名規約](#section-1)
  * [概要](#section-1-0)
  * [クラス・モジュール名](#section-1-1)
* [命名規則](#section-2)
  * [メソッド名](#section-2-0)
      * [自己破壊メソッド](#section-2-0-0)
      * [クラスメソッド](#section-2-0-1)
  * [定数名](#section-2-1)
  * [変数名](#section-2-2)
  * [orm_forのローカル変数名](#section-2-3)
  * [シンボル](#section-2-4)
* [ガイドライン](#section-3)
  * [インデント](#section-3-0)
  * [コメント](#section-3-1)
  * [代入](#section-3-2)
  * [文字列](#section-3-3)
  * [メソッド定義](#section-3-4)
      * [引数](#section-3-4-0)
  * [可変パラメータ](#section-3-5)
  * [ブロック](#section-3-6)
  * [条件分岐](#section-3-7)
  * [繰り返し](#section-3-8)
  * [例外](#section-3-9)
  * [メソッドレベルでのensure](#section-3-10)
  * [ファイル](#section-3-11)
  * [RubyGems](#section-3-12)
  * [メタプログラミング-概要](#section-3-13)
  * [メタプログラミング-method_missing](#section-3-14)
  * [return](#section-3-15)
  * [self](#section-3-16)
  * [Symbol.to_procの活用（Ruby1.9）](#section-3-17)
  * [lambdaとprocの活用 (使い分け)](#section-3-18)
  * [例外のrescueの使い方](#section-3-19)
* [推奨イデオム](#section-4)
  * [options={}パラメータのデフォルト値セット方法](#section-4-0)
  * [FIleのパスを作る記述方法](#section-4-1)
  * [OSネイティブ](#section-4-2)
  * [インスタンス変数の名前](#section-4-3)
* [テスト](#section-5)
  * [概要](#section-5-0)
* [参考文献](#section-6)
  * [一覧](#section-6-0)

##### section-0
## 基本方針
##### section-0-0
### 概要
- レディーに悲しい思いはさせない。
- 食べ物や包丁を武器につかわない。

##### section-0-1
### 目的
- 「読みやすくメンテナンスしやすいコードを書く」ことの価値観をチームで共有できる。
- コーディング規約という共通基盤を通して、プログラマーのスキルアップをはかることができる。

##### section-0-2
### 使い方
- 異臭をはなつ部分は、issueで議論しましょう。決まったものからこちらに移していきましょう。
- 場合によってはこの規約から外れることも時には重要であり、その際はなにかしらの説明を用意するようにしましょう。

##### section-0-3
### 原則
- 見て読みやすく
- 読んで解りやすく
- スコープは適切に
- DRY(Don't Repeat Your Self)
- OCP(Open-Closed Principle)
- 一般的に多く使われているrubyistにまねる
- 変化を受け入れろ！


##### section-1
## 命名規約
##### section-1-0
### 概要
- 名前は単なるラベルではなく、読み手に意味を伝えるべきものである。よって名前は意味のあるものであるべきです。命名する対象の変数のスコープの広さにに比例してわかりやすさ重視にしましょう。

```ruby
#定数
Teinenpi #○
TNP      #×

#クラス/モジュール名
class Joshikousei; end; #○
class Jk; end;          #×

#メソッド名
def joshikousei?; end; #○
def jk?                #×

#インスタンス変数
@joshikousei #○
@jk          #△

#ローカル変数
@girls.each do |g| #○
g.joshikousei?   #○
end
```

名前が長い場合に、入力の手間が増えるという問題があるが、それはIDEやエディタで対応できることなのでここでは問題にせずにいきましょう！

##### section-1-1
### クラス・モジュール名
- クラス、モジュール名は、各単語の一文字めを大文字にし、'_'などの区切り文字は使用しません。
- TPPなどの略語の場合は全て大文字のままにしましょう（省略してるよーって意味が伝わるように）。
- 例外クラスでは特別な事情がない限り末尾に「Error」をつけましょう。
- よほどの意図がない限り、複数名を使用するはやめましょう。

```ruby
#正解
NurseCall
TTPClient
AKBMember
OreOreError

#誤り
Nurse_Call
TPP_CLIENT
AKBMembers
CsvReader
```


##### section-2
## 命名規則
##### section-2-0
### メソッド名
- メソッド名は、全て小文字とし単語の区切りに_をつかいましょう。
- 真偽値を返すメソッド名は、動詞または形容詞に?を付けましょう。
- 破壊的なメソッドと非破壊的なメソッドの両方を提供する場合、破壊的なメソッドには!をつけましょう。
- それ以外で!を使用する場合は、メソッド作成者が利用者に対して、「あぶないぞー！！」っと意思表示したいときにつけましょう。

```ruby
#正解
add_boy
pretty?
noripi?
dance
dance!

#誤り
addBoy
Add_Boy
is_noripi
was_noripi?
```
##### section-2-0-0
#### 自己破壊メソッド

- 例外発生や、自己の状態を書き換える場合は、積極的にエクスクラメーションを使いましょう。

```ruby
#例)
def make_hoge!
  self.foo= "hoge"
end
```
メソッド名には、使うときに注意が必要だということを示すために、末尾に感嘆符をつけることができる。
この命名習慣は、オブジェクト自身を破壊的に書き換えるミューテータメソッドと下のオブジェクトのコピーに変更を加えたものを返すメソッドとを区別するために使われることが多い



##### section-2-0-1
#### クラスメソッド

- 動的にクラスメソッドを追加する等ではなく、あらかじめメソッド名がわかっている（ようは通常のクラスメソッド定義）の場合は、素直に、self.hogeを使いましょう

```ruby
# 推奨
class Hoge
  def self.hoge
  end
end


#クラスメソッドが大量に発生した場合のみ推奨
class Hoge
  class << self
    def hoge
    end
  end
end
```

大量のクラスメソッドがある場合は、毎回selfはだるいので、こちらを使いましょう。また使う場合は、大量のクラスメソッドを一カ所にまとめるという効果もあります。




##### section-2-1
### 定数名
- クラス、モジュール名以外の定数名はすべて大文字とし、単語の区切りに'_'を用いるようにしましょう。

```ruby
#正解
HONKY_TONKY_CRAZY_I_LOVE_YOU
```

##### section-2-2
### 変数名
- 変数名は全て小文字とし、単語の区切りに'_'を用いるようにしましょう。
- スコープが狭いループ変数には、i,j,k という名前、スコープが狭い変数名は、クラス名を省略したものを使用してもオッケー！！！
- 気持ちはとてもわかりますが、hogeとか、fooとかやめときましょう。

```ruby
#(例：eo = ExampleObjext.new

tmp
local_variable
@instance_variable
$global_variable
```

##### section-2-3
### form_forのローカル変数名

- 公式だと _fieldsでAPIリファレンスがかかれている為、profile_fieldsにする。
- form_forの小要素の場合は_fileds, _formなどにする
```ruby
# 悪い例
：f2, f3, p_fp_f

# いい例
profile_fields
```


##### section-2-4
### シンボル
- ハッシュに使用するキー名等は基本的にシンボルにしましょう。
- シンボルは可読性を高め、イミュータブルであるためメモリを節約し実行速度が早くなるなどのメリットがあります。

```ruby
#いい例
excellent = { key: "val" }
good = { :key => "val" }

#悪い例
bad = { "key" => "val" }
```


##### section-3
## ガイドライン
##### section-3-0
### インデント
- インデントは半角スペース２コ。ハードタブの使用は禁止です。あとで本当にきっついです。できる限りエディタでセットしておきましょう。

```ruby
__if girl > 0
____if girl.pefurme? > 0
______puts "ferver!!!!!"
____end
__end
```

##### section-3-1
### コメント
- 書き手は意図が伝わるようなコードを書くようにこころがけましょう。
- コーディング標準や規約に反する場合や、またプログラマー個人の意図がソースに介入している場所に限り、わかりやすいコメントを残すようにしましょう。
- プログラマーの腕のみせどころは、丁寧なコメントを多く書くことではなく、いかにわかりやすいコードを書くかです。よって、コメントを残す場合には「なにを」ではなく「なぜ」を書くようにしましょう。

##### section-3-2
### 代入
- 代入に規制はないが、読みやすさを重視して書きましょう。

```ruby
#正解
a = 'hoge' 
a ||= 'moge' 
a ||= true ? 'foo' : 'bar'

#誤り
a = 1 if a.nil? #×冗長なので×
```


##### section-3-3
### 文字列

-

```ruby
#いい例
"#{i}さん、#{b}さん、#{a}さん"

#いい例)ダブルクオート
%(#{i}さん、#{t}さん、#{o}さん、"ITO")

#悪い例
i + "さん、" + b + ”さん、" + a + "さん"
```



##### section-3-4
### メソッド定義
- メソッドの定義の仮引数リストには括弧を付けるようにしましょう。
- 引数がない場合は、括弧をつけないようにしましょう。

```ruby
#正解
def foo(x, y)
  ・・・
end

#誤り
def foo x, y
  ・・・
end

def foo()
  ・・・
end
```
##### section-3-4-0
#### 引数

- ruby2.1ではキーワード引数をデフォルト値無しで渡せる。2.1以前のバージョンでデフォルト値なしのキーワード引数を定義するとエラーになるのでうっかりやってしまわないようにしましょう。
- ruby2.0ではキーワード引数を使いましょう。
- html_optionsを渡したい場合等、全てがキーワード引数でカバーできるわけではないので、場合によっては使い分けをすること。

```ruby
#2.1
def hoge( msg, limit: , offset: ) 
  p msg
  p limit
  p offset
end


#2.0
def hoge( msg, limit: 10, offset: 10) 
  p msg
  p limit
  p offset
end

#1.9.3
def foo( msg, options=nil)
  config = { limit: 10, offset: 10}.merge options
  p msg
  p config[:limit]
  p config[:offset]
end
```



##### section-3-5
### 可変パラメータ
- 引数の数が呼び出し毎に変わるときなどには、”最後の引数”の前に「*」を付けることによって、引数を(配列として)一度に渡すことができます。
- また、 メソッドから別のメソッドで可変パラメータを呼ぶ場合は＊を付けないと配列が多重化してしまうので必ず引数に＊を付けましょう。

```ruby
def hoge(*aaa)
  p aaa
end

#悪い例
def wrong(*aaa)
  hoge(aaa)
end

#良い例
def ok(*aaa)
  hoge(*aaa)
end
wrong(1,2,3)
#→[[1,2,3]]

ok(1,2,3)
#→[1,2,3]

```


##### section-3-6
### ブロック
- ブロックは基本的にdo・・・endを使用しましょう。ただし１行の時は{}を使いましょう。
- またメソッドチェーンの場合も{}を使用しましょう。

```ruby
#正解
foo(x,y) do
  ・・・
end

akb48.each{|a| a.dance!}

bar.map{|i| i.to_s}.each{|i| puts i}

#誤り
foo(x,y) {
  ・・・
}

foo.map do |i|
  i.to_s
end.each do |i|
  puts i
end
```

##### section-3-7
### 条件分岐
- if式のthenは省略しましょう。
- if !xのような場合は、unlessを使用しましょう。
-  unlessのelseは、ifを使う。unlessは、ifのelseを使う。
- 条件が十分に簡単で、一行でかける場合は、修飾子として使ってもいい。
- 三項演算子も読みやすい範囲で使っていきましょう。

```ruby
#正解
if self.perfume?
  "yes i am."
else
  "no i'm not."
end

# 1行で書けるときのみ
p 'present' unless hoge.blank?

puts "yes i am." if self.perfume?

puts self.perfume? ? "yes i am." : "no i'm not."

#誤り
if self.tashiro? then
  puts "yes i m."
end

unless hoge.blank?
  p 'present'
else
  p 'blank'
end

puts "hoge" if foo && bar && baz && quux

#正解
case bust
when 80
  puts "fum fum"
when 96
  puts "hou hou"
end

#誤り
if bust == 80
  puts "fum fum"
elsif == 96
  puts "hou hou"
end

case bust
when 80 then
  puts "fum fum"
when 96 then
  puts "hou hou"
when 120 then
  puts "haa haa"
end
```

##### section-3-8
### 繰り返し
while の do は省略しましょう。
while !x のような場合は、util x に置き換えましょう。 

```ruby
#正解
while cond
  puts "puts is false"
end

util cond
  puts "cond is false"
end

#誤り
while cond do
  puts "cond is true"
end
```

また、無限ループには loop を使用しましょう。

```ruby
#正解
loop do
  puts "dance!dance!dance!"
end

#誤り
while true
  puts "dance!dance!dance!"
end
```

##### section-3-9
### 例外
- 例外発生の基本方針はそれが「異常事態」かどうかで判断するようにしましょう。
- 明示的に例外を発生させないとプログラムが異常終了してしまう場合に使用しましょう。
- 例えば、ActiveRecord での save!メソッドは使わず、save メソッドを使用しましょう（バリデーションに引っかかるのは例外ではなく、想定された動作なので）。

##### section-3-10
### メソッドレベルでのensure
- 例外処理で後処理が必要な場合、ensureを入れることで例外発生時の処理のし忘れによるエラーを防ぐことができる。
- ensureは例外が発生する・しないにかかわらず必ず実行される。

```ruby
#悪い例（例外が発生しようがしまいが、ファイルが閉じられていないまま処理を抜けてしまっている。）
def hoge
  begin
    file = File.open('sample.txt')
    do_process file  #例外が発生する可能性のある処理
  rescue
    puts "何かが起きました。"
  else
    puts "無事にファイルが開けました。"
  end
end

#いい例（ensureによって、例外の発生に関係なく、きちんとファイルが閉じられている。）
def hoge
  begin
    file = File.open('sample.txt')
    do_process file  #例外が発生する可能性のある処理
  rescue
    puts "何かが起きました。"
  else
    puts "無事にファイルが開けました。"
  ensure
    file.close if file #ここでファイルを閉じる処理を実装できる
  end
end


# また、例外処理がメソッド定義全体にかかる場合はbeginとendを省略できる
def moge
  # メソッドの本体
rescue
  # 例外が発生したときに実行される式
else
  # 例外が発生しなかったときに実行される式
ensure
  # 式が終了する前に必ず実行される式
end

```

##### section-3-11
### ファイル
- ファイルの入出力は特別な事情がない限り、ファイルクローズなどの処理の記述忘れをしないためにも、ブロックを使いましょう。

```ruby
#正解
open("sample.txt", "r") do |f|
  puts f.gets
end

#誤り
begin
  f = open("sample.txt", "r")
  puts f.gets
ensure
  f.close #忘れる可能性あり！！
end
```

##### section-3-12
### RubyGems
- Rails アプリで使用する gems は RAILS_ROOT/Gemfileに記述すること。
- 特に指示がない限りバージョンは明記しておくこと。
- gem 'rails', '3.2.12'

##### section-3-13
### メタプログラミング-概要
- メタプログラミングをおこなう上では、運用/保守に入った際に他人が読んでもわかりやすいことを心がけるようにしましょう。

##### section-3-14
### メタプログラミング-method_missing
- superを使い、親のメソッドミッシングを呼び出すようにしましょう。 

```ruby
#正解
def method_missing(name)
  if @attributes.key? name
    #メソッドとしての処理
  else
    super
  end
end

#誤り
def method_missing(name)
  if @attributes.key? name
    # メソッドとしての処理
  end
  # 例外が発生しないため、わからないよー＞＜
end
```

また、method_missingで追加したメソッドについては、下記の様にどのレベルまでrespond_to?に対応するかは都度チームで決定し拡張しましょう。 ruby def respond_to?(name) @attributes.key?(method) || super end

##### section-3-15
### return

- 必要のあるreturnは書く。（redirect_toとか）
- 必要のないreturnは省略しても良い。

```ruby
#普通のruby

#推奨)
class Gay
  def say
     "I was gay"
  end
end

#非推奨)
class Gay
  def say
     return "I was gay"
  end
end
```

redirect_to の後の return は基本的に書かなくても大丈夫だが、2重に return される時があるので、書いたほうが良い。



##### section-3-16
### self

- インスタンス変数には@をつける。
- インスタンスメソッドにはself.をつける。
- local変数とInstance methodと同じ名前はつけない

```ruby
# 例)
class Hoge
  attr_accessor :title

  def hoge
    #ローカル変数
    title = 'hoge'
    puts title #=> hoge

    #インスタンス変数(attr_accessor)
    self.title = 'moge'
    puts self.title #=> moge
    puts title #=> hoge

    #インスタンス変数(attr_accessor)
    @title = 'foo'
    puts @title #=> foo
    puts self.title #=> foo

    #メソッドの場合
    pp = 'pp'
    pp  #変数ppが評価される
    self.pp #=> oo メソッドppが評価される
  end

  def pp
    puts 'oo'
  end
end
```



##### section-3-17
### Symbol.to_procの活用（Ruby1.9）

- to_proc相当の機能は、Ruby 1.9 ではSymbolクラスへと統合されました。

```ruby
#正解)　どちらも読みやすい
words.map &:upcase
words.map(&:upcase)  

#誤り）わかりずらい（と、いうかエラー）
words.map &:upcase + words.map &:capitalize

#従来）
words.map{|w| w.upcase} + words.map{|w| w.capitalize} 

# 例)だんぜん読みやすくなりました！）
words.map(&:upcase) + words.map(&:capitalize)

#従来
sum = points.inject(0) {|sum, i| sum+i}
#to_sym
sum = points.inject(0, &:+)
```



##### section-3-18
### lambdaとprocの活用 (使い分け)

- procよりもlambdaを積極的に使う
- lambdaリテラルを使う
- 下記の点で、できればlambdaを使う様にしましょう！

１）引数に厳格（ArgmentErrorを投げる）
２）returnがメソッドと同じ感覚で使える（LocalJumpErrorを投げない）

```ruby
#引数の扱いの違い
a = lambda do |x,y|
end

b = proc do |x,y|
end

a.call #アウト（メソッド定義に近い）
b.call #セーフ（よろしくやってくれる）

#returnの違い
def hoge
p = proc { return }
p.call
p 'hoge' #LocalJumpError procのリターンはhogeメソッドから出てしまうため、この行は実行されない。
end

def hoge
p = lambda { return }
p.call
p. 'hoge' #lambdaのリターンはブロックからのリターンのため、この行は実行される。
end

#1.9以降のラムダリテラル
a = ->(x,y) do
end

#x,y=引数 i,n=ローカル変数定義
a = ->(x,y; i=0) do
end

#1.9以降procの呼び出し方法
f.call(x,y) #今まで通り
f[x,y]
f.(x,y)
```



##### section-3-19
### 例外のrescueの使い方

- rescueを使う時は、何をキャッチしているのかを明確にするため、例外オブジェクトを明記する。

```ruby
#正解
def moge
  raise HogeError
rescue HogeError
  puts $!
end

#もっと正解)基本的にはこちらを使用しましょう。
def foo
  raise HogeError
rescue HogeError => ex
  puts ex
end

#誤り
def hoge
  raise HogeError
rescue
  puts $!
end
```




##### section-4
## 推奨イデオム
##### section-4-0
### options={}パラメータのデフォルト値セット方法

- options={}パラメータのデフォルト値セット方法は以下を推奨します。

```ruby
@default_config = {x:1, y:2}

def hoge(options={})
  my_config = @default_config.merge options
end
```



##### section-4-1
### FIleのパスを作る記述方法

- Fileのパスを作る場合は以下を推奨します。

```ruby
#悪い例
path = "#{Rails.root}/tmp/hoge"

#良い例
path = File.join Rails.root, "tmp", "hoge"
```


##### section-4-2
### OSネイティブ

- OSネイティブはなるべく呼ばないようにすることを推奨

```ruby
DIR_PATH="/tmp/hoge"
  #これはだめ
  def init_dir
    `mkdir -p #{DIR_PATH}`
  end


  #推奨
  require 'fileutils'
  def init_dir
      FileUtils.mkdir_p DIR_PATH unless Dir.exist?(DIR_PATH) #無い場合のみ実行
  end
```


##### section-4-3
### インスタンス変数の名前

- 代入側は呼び出したクラスのdowncaseにすることを推奨

```ruby
#悪い例
obj = User.newu = User.new

#いい例
user = User.new
```


##### section-5
## テスト
##### section-5-0
### 概要
- プロダクトコードに書く前に、テストコードを書きましょう。
- テストツールにはRSpecをつかいましょう。
- テストは自動化しましょう。


##### section-6
## 参考文献
##### section-6-0
### 一覧
- プログラミング言語Ruby[ISBN978-4-87311-394-4]
- 達人プログラマー[ISBN4-89471-274-1]
- プログラミング作法[ISBN4-7561-3649-4]
- Kenji Hiranabe, コーディング標準(オリジナル) -- http://objectclub.esm.co.jp/eXtremeProgramming/CodingStd.doc
- Ruby コーディング規約 -- http://shugo.net/ruby-codeconv/codeconv.html



