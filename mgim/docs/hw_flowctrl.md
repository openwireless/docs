# MGIM(V4.1)で、ハードウェアフロー制御を使用する手順

1. Arduino IDEのボードマネージャを使って、Arduino Core for SAMD（具体的には、「Arduino SAMD Boards (32-bits ARM Cortext-M0+)」）を最新バージョンの1.8.11に更新する。
2. hl7800ライブラリのサンプルスケッチmonitor_hl7800をMGIMに書き込み、「AT&K3」コマンドを実行する。HL7800からOKが返ってくることを確認すること。
3. Windows PCの場合は、C:\Users\USER_NAME\AppData\Local\Arduino15\packages\arduino\hardware\samd\1.8.11\variants\arduino_zero\ にある variant.cpp を下記のように書き換える。

  - 【書き換え前】 Uart Serial( &sercom5, PIN_SERIAL_RX, PIN_SERIAL_TX, PAD_SERIAL_RX, PAD_SERIAL_TX ) ;
  - 【書き換え後】 Uart Serial( &sercom5, PIN_SERIAL_RX, PIN_SERIAL_TX, PAD_SERIAL_RX, PAD_SERIAL_TX, 24, 23 ) ;

上記の手順で、ハードウェアフロー制御が使えるようになります。
なお、「AT&K3」は、ハードウェアフロー制御を指定するATコマンドで、一度実行すると、電源を切ってもHL7800内のフラッシュメモリに記録されます。

---
2021.11.10 更新
