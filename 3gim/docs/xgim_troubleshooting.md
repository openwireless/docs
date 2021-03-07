## トラブルシューティング(3GIM/4GIM)

| No | 問題 | 現象 | 対応策 | 補足 |
| --- | --- | --- | --- | --- |
|1|配線|UART(Tx:送信,Rx:受信)、電源およびGNDが正しく理解できていない|･3GIMのピンの#1~#6までを正しく理解して上で配線･接続のこと\\ #1（電源On/Off：任意)、#2(RX)、#3(Tx)、#4(3.3V/5V電源)、#5(3.3~4.2V電源)、#6(GND)|･#5(VCC)で外部電源を利用する場合には、3.7Vリチウムイオン電池を推奨|
|2|応答 レスポンス|･コマンドを送っても返信がない\\ ･正しい応答ではない|･通信モジュールとマイコンボードとの通信またはマイコンボードとPCとの通信において、以下の原因が考えられる\\ ①3GIMの配線が正しくできていない(配線接続確認・はんだ付け不良)\\ ②電源供給に問題がある(電源電圧の確認)\\ ③ UART通信速度の設定が間違っている(設定確認)\\ ④ 初期電源後の待ち時間を考慮不足(15秒以上待機)\\ ⑤プログラムに間違いがある\\ ⑥ArduinoIDEシリアルモニタ画面の改行コードをCR+LFに変更|･応答が正しく表示されない場合の原因は、配線ミスや配線での接触不良等が考えられる\\ ･②の電源供給で,VCCの3.3～4.2Vを間違えるケースが多い。また、電源に十分な電流供給能力があることを確認のこと\\ ･⑥の場合、改行選択メニューで「CRおよびLF」を選択のこと|
|3|エラー頻発|･#=NGが多発\\ ･立ち上げタイミングの問題\\ ･電源供給(電流が小さい)問題|･配線・接続が正しくできていること\\ ･適正なSIMカードの挿入されていること\\ ･正しく電源供給できていること|･RSSI(電波強度測定)やSIMカードのサービス確認 ･$YRや$YSコマンドで確認|
|4|時間の取得|･時間の取得($YT)が間違っている |･正しいSIMカードとアンテナ接続によって正しく設定される\\ ･正しい時間を取得するにはしばらく時間が掛る\\ ･電波状態が悪いところでは正確な時間が取得しにくい|･同上および$YRと$PRで確認\\ ･電波状態の良いところで行う|
|5|SMS送受信|･SMSの送受信ができない\\ ･SMSの応答が無い|･お使いのSIMカードがSMS対応ではない(切替え必要)\\ ･SMSサーバとのやり取りでの不備(何度か読込み必要)|同上|
|6|GPS取得|･GPS取得ができない\\ ･GPS取得に時間が掛る|･GPSアンテナが正しく接続されていること\\ ･GPS電波状態が良い所(PCから離す)で実施のこと\\ ･初期立上げでは数分から10分ほど掛る場合がある\\ ･電源供給が正しくできていること|･一度GPS取得できており、電源がずっと入った状態であれば次からは即時の取得が可能\\ ･AGPS機能を利用することで解決する場合もある|
|7|ネット接続|･Webコマンド群やTCP/IPコマンド群が正しく応答しない|3Gアンテナを正しく接続する\\ ･正しいSIMカードが挿入されていない\\ ･SIMカードの接続不良(再度再挿入などを実施)\\ ･電源供給が正しくできていること|「正しいSIMカードの利用」とは、[[sim|利用実績のあるSIMカードについて]] に掲載されており、Profile設定で正しい番号に設定されている利用をいう|
|8|TCP接続|･最初のconnectTCP()の呼び出し、あるいは最後の通信から概ね30秒以上の時間が経過している場合、connectTCP()の呼び出しにTIMEOUTエラーで失敗する時がある。|ライブラリa3gim2のサンプルスケッチsample_TCPIPにあるように、connectTCP()関数の呼び出しを3回程度までリトライするようにTCP接続処理の実装を行う|この問題は、一定時間通信をしないと3Gのコネクションを一旦切断して消費電力を抑える動作をHL8548-Gが行うことに起因する。|
|9|初期化|Arduinoへスケッチを書き込んだ後、setup()関数の中でstart()を実行しないと3GIMが応答しなくなる場合がある|a3gimライブラリのマニュアルに記載のある通り、setup()関数内での初期化処理では、必ずstart()関数を呼び出すようにします|$YEコマンドによるリセットではなく、a3gim.start()関数を呼び出して、確実に3gimの電源をOFF/ONするようにすること。|
|10|APN設定|$PSコマンドでAPNを設定するときにエラーとなる|$PSコマンドでAPNを設定する場合には、有効なSIMカードと3Gアンテナを正しく装着しておく必要があります| |
|11|動作|3G通信の状態等によっては3GIMからレスポンスが返ってこない場合がある|ライブラリa3gim経由ではなく、直接コマンドを送信して3GIMを利用する場合は、必ずレスポンスの受信処理で一定時間でタイムアウトするようにしてください。レスポンスがない場合は、3GIMがハングアップしている可能性がありますので、PWR_ONピンを使って3GIMの電源をOFF/ONして復帰させてください。| |
|12|動作|$YRコマンド(RSSIの取得)を短時間に何度も繰り返し実行すると、おかしい値が返却される場合がある|RSSIの取得はモデムチップHL8548-G/7539に大きな負荷を掛けますので、RSSIの取得は最低限に抑えてください。| 同じく$PSコマンドも負荷を掛けるため、最低限の実行としてください。|
|13|動作|$TCおよび$TWコマンドでネットワークのエラーが発生した場合、最大60秒レスポンスがない|モデムチップのタイムアウト推奨値に従って、最大60秒間(固定値で変更不可)、相手からのレスポンスを待ちます。|4GIMではこのタイムアウト値を変更する機能を設ける予定です。|