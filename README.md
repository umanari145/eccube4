# EC-CUBE4のカスタマイズなど

#　ルーティング

- 独自にルーティングを管理する場所はない
- Controller内の@Routeで管理→おそらくSynfonyの仕様
- src/Controller内にHogeControllerなどと書いて任意のメソッドに@Route(〜)で動く

#　追加のクラス

- 独自に追加するControllerやEntityは`app/Customize`直下に作成する。
例 `app/Customize/Controller/HogeController`など
