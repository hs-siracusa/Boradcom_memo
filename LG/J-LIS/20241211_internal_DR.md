SRMのよろず相談みたいなやつを海でやっている。
    SRM＋AVI(NSX)で51K


切り替え想定パターン
    DC障害
        1. DR側にレプリケーションデータ(ストレージ機能 or vSR)、基盤、NW切り替え(短時間での切り替えが可能)
        2. バックアップデータ、基盤、NW切り替え(時間かかる、ストレージ機能で複製する、SRM使用不可)

    一部障害(DRするまででもないとき)
        機器の多重化(NW機器、SV)

    データ破損(ディスク障害、ランサム)
        バックアップデータ
            バックアップ保存先?


DR
    DC障害対応の方法でバックアップの方法も変わってくる
        1. DR側にレプリケーションデータ(ストレージ機能 or vSR)、基盤、NW切り替え(短時間での切り替えが可能)
        2. データストア単位のバックアップ製品によるバックアップデータ、基盤、NW切り替え(時間かかる、ストレージ機能で複製する、SRM使用不可)
            DRサイトをどれだけ用意するのかによって決めたほうが良い
            コスト的には変わらない
            台数が多いのであれば1のほうが楽にリストア可能
            台数が少ないのであれば、2でもOK(リストアの作業が発生するため)            

    1．何でレプリケーションして(ストレージミラーリング or vSR)、どう立ち上げるのか(SRM or 手作業) 
        SRMでいいこと→立ち上げる際の順番、スクリプト等
        
    2． バックアップ製品側で紹介


    先に話したほうが良い
        バックアップの機能でどう飛ばすか？をDRで考えないといけないから。
    どれだけのリソースを用意するのか？
        フルのリソースを用意するのか？
        どこまでの範囲をDRの対象にするのか
        DR側は全損した時と想定、ネットワーク的にも。
        →DR側でどれだけリソース用意する？
            難しいと思うので最低でも半分くらい用意するのが一般的
        全損はあんまり起きないんで、N＋2とか、二面用意する。
            リソース拡張とかにも対応できる

    二重投資
        バックアップ製品がDRに対応している場合、そのまま使ったほうが安く済む


バックアップ
    バックアップ手法
        データ領域＋仮想マシン領域
        エージェントエージェントレス
        バックアップ単位 仮想マシン単位 or ストレージ全体
        リストア単位 仮想マシン単位 or ストレージ全体 or ファイル単位 
        vSAN バックアップソフト使って3Tierと同じようにバックアップできますよ
            対応ソフト一覧？

    データ領域→データベース側でバックアップをとっている
        Oracleを引き続き使うのであればDBをバックアップしてもらう
        引き続きDBの機能を使ってバックアップ？
        データ取得単位は？

    一般的なバックアップ製品紹介

    システム領域
        仮想マシン単位でとれますよ
        いまの取得単位は？
            ストレージ全体？VM単位？
            
        ストレージ単位
            仮想マシン数が多いなら良い
            全体でとれるからリソース食わない
            LUNの数だけ設定すればいいので楽
            vSRを使わないのであれば、楽。
            vCenterから見えてないから、SRMの前にストレージ側のリストが必要になってくる
        仮想マシン単位
            ジョブ数が仮想マシン数になるので多くなる
    