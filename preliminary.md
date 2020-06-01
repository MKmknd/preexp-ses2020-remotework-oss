
## 感情推定手法の概要

感情分析手法はルールベースの手法と機械学習などモデルベースの
手法が存在する．ルールベースの手法は，あらかじめ肯定的な単語と，
否定的な単語の辞書，及び，文法上のルール（否定文など）を
作成しておき，入力された文章に対してこれらの辞書とルールを適用して，
最終的な感情値を算出する．モデルベースの手法では，機械学習のモデルなどを
感情値が与えられたデータセットを用いて学習し利用する方法である．

有名な感情分析手法には，NLTKにて実装されているHuttoら~\cite{hutto2014vader}が提案した
VADER~\cite{hutto2014vader}がある．VADERはルールベースの感情分析手法であり，
軽量で解釈が容易であるという利点がある．HuttoらはSNSを主眼としてVADERを提案している，
ここで，VADERがソフトウェア開発者とは異なるコミュニティで
作成されているため，その予測精度の信頼性には疑問が残る．
Islamら~\cite{SentiStrength-SE}は，開発者以外のコミュニティで
検証されたSentiStrengthというルールベースの手法を
そのままソフトウェア工学コミュニティに適用する場合に弱点が存在することを
指摘している．特に，コミュニティが異なることによる使用語彙の
差が感情推定精度に大きな影響を与えると述べている．


## ソフトウェア工学ドメインとSNSそれぞれにおける感情分析手法の比較

そこで，ソフトウェア開発者のコミュニティにおける
手法を使用するべきか，その他のドメインで学習されたものを
使用するべきかを，感情分析の予測結果で比較し検討した．
予測結果を比較検討するためのテストデータとして，
Ortuらが提供している手作業で感情値を付与した
課題管理の課題のコメントのデータセット~\cite{ortu2016MSR}を
利用した，このデータセットは，
Group-1から3までデータが存在するが，Ortuらは，
Group-2で提供している感情値が，Group-1のデータセット評価時に，
感情値の評価者間での一致率が高かったことを
指摘している．そして，今回我々が分析対象としてるデータと同じ
課題のコメントを利用している．
これらの点から，Group-2のデータを利用した．
SEドメインにおける感情分析ツールとして，Calefatoら~\cite{Senti4SD}
が提案しているSenti4SDを採用した．
Senti4SDは，スタックオーバーフローの質疑データを元にして，
用語，キーワード，そして，文章の意味を考慮した特徴を生成し，
それを利用して分類を行う手法である．
評価実験より，他のソフトウェアドメイン分野における
感情分析ツールであるSentiStrength-SEよりも精度が良くなることが確認されている．
また，ドキュメントが充実した実装が公開~\cite{[Senti4SD-implementation]}
されており，再利用が容易である．


以下が，VADERとSenti4SDの比較結果である．肯定，否定，
そしてニュートラルな感情値をどれだけ正しく推定できたかを
precision，recallとしてF1で測定している．

```VADERの結果
VADERの結果
--- 肯定 ---
precision: 0.672 (748/(748+365))
recall: 0.977 (748/(748+18))
f1: 0.796 (2*1,496/(2*1,496+365+18))
--- 否定 ---
precision: 0.345 (76/(76+144))
recall: 0.661 (76/(76+39))
f1: 0.454 (2*152/(2*152+144+39))
--- ニュートラル ---
precision: 0.951 (254/(254+13))
recall: 0.353 (254/(254+465))
f1: 0.515 (2*508/(2*508+13+465))
```

```Senti4SDの結果
Senti4SDの結果
--- 肯定 ---
precision: 0.836 (667/(667+131))
recall: 0.871 (667/(667+99))
f1: 0.853 (2*1,334/(2*1,334+131+99))
--- 否定 ---
precision: 0.327 (56/(56+115))
recall: 0.487 (56/(56+59))
f1: 0.392 (2*112/(2*112+115+59))
--- ニュートラル ---
precision: 0.816 (515/(515+116))
recall: 0.716 (515/(515+204))
f1: 0.763 (2*1,030/(2*1,030+116+204))
```

このように，肯定とニュートラルな文章の推定においては
Senti4SDが優れており，否定においてはVADERが優れている結果となった．
よって，ソフトウェアドメインにおいて提案された
Senti4SDの方が2つの感情値においてより良い精度を示したことから，
SNSドメインで提案された手法ではなく，ソフトウェアドメインで
提案された手法を使用することとした．


## ソフトウェアドメインにおける感情分析手法の比較

ソフトウェアドメインにおいて有名な感情分析手法は以下の4つが挙げられる．
- Senti4SD [Senti4SD]
- SentiStrength-SE [SentiStrength-SE]
- SentiSE [SentiSE vs SentiCR]
- SentiCR [SentiCR]


この中で，SentiStrength-SEはベースラインとして使用されており，
予測精度は他の手法より劣る．
SentiSEとSentiCRに関しては実装が公開されているが，ドキュメントが
乏しく使用方法を理解することが難しい~\cite{[SentiCR-implementation], [SentiSE vs SentiCR]}．
SentiSEはSentiCR及びSentiStrength-SEより予測精度が良い\cite{SentiSE vs SentiCR}．
しかし，予測精度を極端に改善しているわけではなかった．
Senti4SDもSentiStrength-SEと比較されているが，同程度の改善
度合いである\cite{Senti4SD}．
以上より，ソフトウェアドメインで提案されている
感情分析手法の中で，Senti4SDを我々の研究で使用した．


## 参考文献

[SentiCR]
Ahmed, T., Bosu, A., Iqbal, A. and Rahimi, S.: SentiCR: A Customized Sentiment Analysis Tool for Code Review Interactions, In Proc. of ASE (2017)

[SentiSE vs SentiCR]
amiangshu/SentiSE, "SentiSE", GitHub (online), available from <https://github.com/amiangshu/SentiSE>, (last accessed 2020-6-1)

[Senti4SD]
Calefato, F., Lanubile, F., Maiorano, F. and Novielli, N.: Sentiment polarity detection for software development, Empir. Soft. Eng., Vol. 23, No. 3, pp. 1352–1382 (2018).

[Senti4SD-implementation]
collab-uniba/Senti4SD, "Senti4SD", GitHub (online), avilable from <https://github.com/collab-uniba/Senti4SD>, (last accessed 2020-6-1)

[hutto2014vader]
Hutto, C. J. and Gilbert, E.: Vader: A parsimonious rulebased model for sentiment analysis of social media text, In Proc. of ICWSM (2014).


[SentiStrength-SE]
Islam, MDR. and Zibran, MF.: Leveraging automated sentiment analysis in software engineering, In Proc. of MSR (2017)

[ortu2016MSR]
Ortu, M., Murgia, A., Destefanis, G., Tourani, P., Tonelli, R., Marchesi, M. and Adams, B.: The emotional side of software developers in JIRA, In Proc. of MSR (2016).
  
[SentiCR-implementation]
senticr/SentiCR, "SentiCR", GitHub (online), available from <https://github.com/senticr/SentiCR/>, (last accessed 2020-6-1)
