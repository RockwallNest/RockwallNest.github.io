# RockwallNest.github.io
自作のRubyAppなどを公開。

## RubyWithJulia
##### 素朴にRubyでbitFlyerのAPIをたたいてみるAppをつくった...
bitFlyerのAPIをたたいて、Bitcoinのticker情報を取得します。<br>
ticker情報は10だけ表示します。<br>
接頭の時刻はTimeオブジェクトに変換してあり、CSVファイルに出力してグラフ化できるようにしてあります。<br>
末尾の時刻はtimestampそのもので、元データとして残してあります。<br>
RやJuliaなどを使えばグラフ描画はできます。<br>

## Ruby/Ractor
pipe というRactorで生成したchannelを用いて、並列実行するRubyコードを作成した。<br />
```ruby 
pipe = Ractor.new do 
  loop do 
    Ractor.yield Ractor.recv
  end
end
```
##### goroutine-channel処理系と関数オブジェクトに似ているRactor
Ractorのおかげで、Ruby2.0系よりも処理が3倍早くなったということを実感した。<br />
並列性を考えると、現在のRactorの用途では、Go言語のgoroutineの処理系に近いのではないかと思う。<br />
routine同士をpipeというchannelで結び、並列処理を実行する仕組みが似ていると思う。<br />
また、RactorはScalaなどの関数プログラミング言語にも似ている。 <br />
値を生成するgeneratorから値を取り出すextractorを用いる点などが <br />
forEachメソッドを用いたコードと似ていると思う。 <br />
```ruby 
# N: some integer value
gen = Ractor.new pipe do |pipe| 
  (1..N).each do
    n = pipe.take
    Ractor.yield n 
  end
end

extr = (1..N).each do |i|
  r, n = Ractor.select(gen)
  [i, n]
end
# => [1, n1], [2, n2], ...
# rはRactor objectで棄却され、nの値のみが取り出される

```


