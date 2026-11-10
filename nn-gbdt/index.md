<style>
  .markdown-body table {
    display: table !important;
    width: 100% !important;
    max-width: 100%;
    table-layout: fixed;
  }
  .markdown-body th,
  .markdown-body td {
    vertical-align: top;
    word-break: break-word;
  }
  .cmp th:first-child,
  .cmp td:first-child {
    width: 25%;
  }
  .nn-gbdt-charts {
    overflow-x: auto;
  }
  .nn-gbdt-charts-inner {
    width: 908px;
    margin: 0 auto;
  }
  .nn-gbdt-charts-labels {
    display: grid;
    grid-template-columns: 450px 450px;
    gap: 8px;
    font-weight: 600;
    text-align: center;
    margin-bottom: 0.4rem;
  }
  .nn-gbdt-charts iframe {
    display: block;
    width: 908px;
    height: 450px;
    border: none;
  }
</style>

# 大規模NNとGBDTのざっくり比較

## 概要

<table class="cmp">
<thead>
<tr><th>比較項目</th><th>大規模NN</th><th>GBDT</th></tr>
</thead>
<tbody>
<tr><td>主に扱うデータ</td><td>表形式ではないデータ(非構造化データ)。テキスト、画像、音声、動画など。</td><td>表形式データ(構造化データ)。スプレッドシート、Excel、DBテーブル、CSV、TSVなど。</td></tr>
<tr><td>モデルの出力</td><td>人間が五感で理解できる情報（テキスト、画像、音声、動画）や、数値（確率など）。</td><td>数値（予測価格、発生確率、スコアなど）のみ。</td></tr>
<tr>
<td>代表的な使い道</td>
<td>
<ul>
<li>テキストの生成、対話、会話、要約、翻訳、コーディング</li>
<li>画像・映像の生成、加工、認識。自動運転</li>
<li>音楽生成。音声からの文字起こし。</li>
<li>創薬、気象シミュレーション</li>
<li>囲碁・将棋・チェス、対戦型PCゲーム</li>
</ul>
</td>
<td>
<ul>
<li>相場価格の推定・割安商品の発掘（中古車、中古マンション、賃貸物件の家賃、中古PC、金融商品など）</li>
<li>金融サービスの審査（クレジットカード発行、ローン契約、保険加入、融資など）</li>
<li>レコメンド(EC商品やニュース記事の「あなたへのおすすめ」、音楽・動画サイトの「次に再生」候補など)</li>
<li>需要予測(電力需要：発電計画や需給調整。商品需要：欠品防止や食品ロス削減。荷量予測：物流・オペレーション最適化。)</li>
</ul>
</td>
</tr>
<tr><td>AI界での立ち位置</td><td>勾配ブースティングと共に、現代AIを支える二本柱の1つ。非構造化データ学習におけるデファクトスタンダード。</td><td>ニューラルネットワークと共に、現代AIを支える二本柱の1つ。構造化データ学習におけるデファクトスタンダード。</td></tr>
<tr><td>モデルの汎用性</td><td>ただ一つの汎用モデルを構築する世界。<br>非構造化データ（テキスト、画像、音声、動画など）に共通する普遍的な性質を学習するため。</td><td>要件ごとに個別の専用モデルをゼロから構築する世界。<br>表データの構造が業務要件ごとに全く異なり、他への流用が原理的にできないため。</td></tr>
<tr><td>現場の定番ツール</td><td>PyTorch（開発基盤）<br>Hugging Face（モデル/データ管理）<br>DeepSpeed / FSDP（分散並列学習）<br>vLLM（超高速推論エンジン）</td><td>LightGBM（実務の圧倒的デファクト）<br>XGBoost（元祖・安定）<br>CatBoost（カテゴリ変数に強み）</td></tr>
</tbody>
</table>

## コスト目安

<table class="cmp">
<thead>
<tr><th>比較項目</th><th>大規模NN</th><th>GBDT</th></tr>
</thead>
<tbody>
<tr><td>必要なデータの量</td><td>テキストなら新書で500万冊 〜 1億冊超。画像なら数億〜50億枚。音声なら数十万時間（80年分）。</td><td>1,000行 〜 1,000万行の表データ</td></tr>
<tr><td>使うハードウェア</td><td>専用のAIサーバー</td><td>通常の開発用PC</td></tr>
<tr><td>ハードウェアの調達費用</td><td>5000万円 〜 5000億円<br>※一台約5000万円のAIサーバーを複数台(1台 〜 1万台)</td><td>5万円 〜 60万円</td></tr>
<tr><td>必要システムメモリ（RAM）</td><td>2 TB 〜 25 PB</td><td>32 GB 〜 256 GB</td></tr>
<tr><td>必要GPUメモリ（VRAM）</td><td>640 GB 〜 8 PB</td><td>なし（0 GB） 〜 24 GB</td></tr>
<tr><td>学習にかかる時間（1回あたり）</td><td>3日間 〜 90日間</td><td>5分 〜 6時間</td></tr>
<tr><td>学習の電気代（1回あたり）</td><td>3万円 〜 45億円</td><td>5円 〜 500円</td></tr>
<tr><td>予測にかかる時間（1回あたり）</td><td>200ms 〜 5,000ms（0.2秒〜5秒）</td><td>0.5ms 〜 5ms（0.0005秒〜0.005秒）</td></tr>
<tr><td>予測の電気代（1回あたり）</td><td>約0.01円 〜 0.3円</td><td>約0.000001円</td></tr>
</tbody>
</table>

## 仕組み

<table class="cmp">
<thead>
<tr><th>比較項目</th><th>大規模NN</th><th>GBDT</th></tr>
</thead>
<tbody>
<tr><td>学習のイメージ</td><td>特徴量空間内の座標に、画像・自然言語・音声・動画などの情報群をデータ点として配置してから、意味の近いデータ点同士が近くに集まるように、空間を変形させていく。</td><td>特徴量空間内の座標に、特定の表データの各行をデータ点として配置してから、意味の近いデータ点同士が同じエリア内に含まれるように、空間を格子状に細分化していく</td></tr>
<tr><td>空間の扱い方</td><td>パン生地をこねるように、空間をグニャグニャに変形させる。※変形させたものは潜在空間と呼ばれる。</td><td>豆腐をさいの目切りするように、空間をブロックに切り刻んで分割する</td></tr>
<tr><td>完成したモデルのイメージ</td><td>空間を変形させる「こね方の手順（レシピ）」。</td><td>空間を分割する「切り方の手順（レシピ）」と、細分化された各エリアに割り当てられた「予測値」。</td></tr>
<tr><td>推論のイメージ</td><td>まっさらな特徴量空間内の座標に、予測したい情報をデータ点として配置してから、その空間をレシピと全く同じ手順でこねていく。</td><td>まっさらな特徴量空間内の座標に、予測したい情報をデータ点として配置してから、その空間をレシピと全く同じ手順で切っていく。</td></tr>
<tr><td>予測結果</td><td>予測データ点が最終的に着地した「空間の座標」を答えの形に変換した値。</td><td>予測データ点が最終的に属する「空間のエリア」にあらかじめ割り当てられている値。</td></tr>
<tr><td>予測根拠の説明のしやすさ</td><td>特徴量空間をグニャグニャに変形するため、元の特徴量のスケールと意味が破壊され、ブラックボックスになる。</td><td>空間を一切歪めず「元の座標軸に平行な切断」しか行わないため、元の特徴量のスケールと意味が保持され、特徴量ごとの影響度を通じた納得感のある説明が可能。</td></tr>
<tr><td>斜めの関係性(特徴量同士の連動)の捉え方</td><td>空間そのものを自在にこねる（回転・歪曲させる）ことで、複雑な斜めの関係性を空間の柔軟な歪みとしてしなやかに表現できる。</td><td>複雑な斜めの関係性を捉えるには、直交する区切りを階段状に大量に配置しなければならず、空間表現として非効率になる。</td></tr>
<tr><td>トレードオフ</td><td>「空間を回転・変形する自由」を手に入れた代わりに、「元の座標軸の意味(解釈性)」を失った。</td><td>「空間の変形を禁じ、座標軸を守った」代わりに、「斜めの関係の表現効率」を犠牲にした。</td></tr>
<tr><td>必要な数学のレベル</td><td>大学教養課程レベル</td><td>大学教養課程レベル</td></tr>
<tr><td>裏で使われている数学</td><td>線形、非線形写像：空間をこねる操作<br>連鎖律：空間のこね方の調整</td><td>偏微分：各学習データ点ごとの誤差を測る<br>テイラー展開：空間を切る位置の基準を作る</td></tr>
</tbody>
</table>

## 学習のビジュアルイメージ

<div class="nn-gbdt-charts">
  <div class="nn-gbdt-charts-inner">
    <div class="nn-gbdt-charts-labels">
      <div>大規模NN</div>
      <div>GBDT</div>
    </div>
    <iframe src="./charts.html" title="NNとGBDTの学習のビジュアルイメージ" loading="lazy"></iframe>
  </div>
</div>
