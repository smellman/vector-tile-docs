# vector-tile-docs home
ベクトルタイル、とりわけバイナリベクトルタイルに関する情報がそれほど一般的ではない現在の状況について、説明を試みる。

## 前提
- ウェブにタイル形式で提供されるジオデータは、そのデータセットサイズで大きく分けられる。（1）日本一国以上をカバーするような、データセットサイズ350GB〜数TB程度のデータセット、（2）都道府県をカバーするような数百MB〜5GB程度のデータセット、（3）個別地域の個別主題に対応するような、数十KB〜10MB程度のデータセット。この中間のスケールのデータセットは、あまり存在しない。
- バイナリベクトルタイルについては、早くは数年前に Google が実用化を果たしている。Apple も実用化を果たしている。ただし、彼らの技術は対外的に共有されていない。その状況のもと、Mapbox が OSM データを使用したビジネスの中で、バイナリベクトルタイルのオープンスタンダード vector-tile-spec と、それに準拠したツールを提供開始したのが最近になる。彼らと技術を共有する形で、Mapzen や esri がそれぞれのビジネスの中でバイナリベクトルタイルを採用し始めているところ。当然、それぞれの採用の仕方は、それぞれのビジネスの都合の範囲内で行われている。
- 新規参入者であれば、vector-tile-spec に乗らない理由は特段見当たらない。実際、国土地理院（地球地図国際運営委員会事務局）やマピオンなど国内のプレイヤーも、それぞれのデータを vector-tile-spec で実験的に出し始めているところ（FOSS4G 2016）。

## データセットサイズとタイルの種類
- データの種類の比で言えば、上記（1）と、上記（2）＋（3）は 2:8 といった感じになる。
- データアクセス量の比で言えば、上記（1）と、上記（2）＋（3）は 8:2 といった感じになる。
- バイナリベクトルタイル技術は、上記（1）、（2）、（3）のいずれにも適用可能であるが、上記（3）については、タイル化しないシングル GeoJSON での対応で必要十分であり、上記（2）については、今のところそれほど多くはウェブに出てきていない。よって、バイナリベクトルタイル技術を追求するモチベーションが高いのは、上記（1）のような、大規模・少品種・大アクセスのデータセットの担当者に限られることになる。
- なお、バイナリベクトルタイルを普及するには、上記（2）のニーズを掘り起こす段階でバイナリベクトルタイルが簡単に作れるようにすることが良い方策になるであろう。

## データとスタイルの分離等に起因するクラスタの細分化
- バイナリベクトルタイルでは、画像タイルと異なり、データとスタイルが分離される。ウェブの一般論として、ジオデータの活用推進として、それは良いことである。
- 他方、データとスタイルが分離してしまうことから、新規参入者がとりつきにくくなっていることは事実。HTML から CSS が別れたときのような面倒臭さが発生している。
- また、データを変換する必要があるプレイヤーと、スタイルを調整したいプレイヤーとが分離しがちであることも特徴。OSM などのベースデータのタイルに独自のスタイルをつけたいプレイヤーは、実際にはバイナリベクトルタイル変換をする必要がない。また、データ変換をするプレイヤーは、データ変換で力尽きてしまい、よいスタイルを考える余力がないというところもある。

## スタイル言語の細分化
- バイナリベクトルタイルを WebGL なり Canvas なりで描画する際のスタイル言語については、Mapbox Style, CartoCSS, Tangram の YAML ファイルなど、やや細分化しているところがある。用途によって使い分ける、というのが定説になると思われるが、CartoCSS については、やや落ち目という観測もあり得る。

## 注釈
- バイナリベクトルタイルのフォーマット、特にスタイル付けなどを前提とした属性のパックの仕方、等については十分枯れてはいない。このことが、特にデータ提供者のバイナリベクトルタイル採用を躊躇させているところがある。
- バイナリベクトルタイルが枯れていないことを恐れるプレイヤーの中には、まずは GeoJSON ベクトルタイルから始めようとするプレイヤーがある。GeoJSON ベクトルタイルへの変換は、単純すぎるために決定版のツールに欠けるところがある。本来は、デスクトップ GIS からエクスポートできればよい。
- タイル一般論であるが、ファイルシステムに展開してしまったタイルは、コピーや移動の便宜に欠ける。.mbtiles にパッケージすることが一般的であるが、これが新規参入者からみた障壁を高めている感もある。
- ブラウザの WebGL 対応が従前ではないことも、バイナリベクトルタイルの採用を躊躇させている原因の一つであろう。なお、GeoJSON ベクトルタイルであれば、Leaflet 用の canvas / SVG ベースのエクステンションに安定しているものがある。安定志向かつ小さいデータであれば、GeoJSON ベクトルタイルを採用しがちになる原因であろう。
- ただし、バイナリベクトルタイルではなければ実現できない速度領域というものが、存在する。そこに手を付けざるを得ないプレイヤーのみが、バイナリベクトルタイルに進出しているという状況と理解するのが、早わかりであろう。

## 情報源
- データフォーマットを知りたい場合、 vector-tile-spec を見る。
- Shapefile 等からの変換を行いたい場合、tippecanoe を見る。
- スタイル付けをしたい場合、kosmtik や Mapbox Studio や Tangram Play を使う。
- ブラウザで表示をしたい場合、Mapbox GL JS や Tangram を使う。Tangram は Leaflet の拡張として実装されている。OpenLayers の新しめのものでもサポートされているらしい。

## 作品例
これから充実させていく。
- マピオンベクター https://mapion.github.io/mapion-vector/
- 国土地理院ベクトルタイル提供実験「基盤地図情報（基本情報項目）」ベクトルタイルの一部（佐賀あたり）をタイルワイズにバイナリベクトルタイル変換してホストしてみたもの https://hfu.github.io/8220102/
- 地球地図バイナリベクトルタイル https://hfu.github.io/globalmaps-vt-style/
- 電子国土基本図（住居表示住所）バイナリベクトルタイル https://hfu.github.io/gsi-address-vt/
