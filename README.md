# EC-CUBE4のカスタマイズなど


## 主なファイル構成

- `src/Eccube/Controller` いわゆるController
- `src/Eccube/Form` Formで受け取ったあたいの制御
- `src/Eccube/Form/Type` 実際のFormの個別のパラメーターの制御など プラダウン値の作成など
- `src/Repository` いわゆるRepository(SQLの作成など)
- `src/Resource/template` テンプレート(Twig)
- `html/template` css,jsなどのリソース系ファイル置き場

## ルーティング && URL

- 管理画面はdefaultでは`http://example.com/admin`で、admin部分を任意に変更。(現在は`http://example.com/eccube_admin_dayo/`)
- 独自にルーティングを管理する場所はない
- Controller内の@Routeで管理→おそらくSynfonyの仕様
- `src/Controller`内にHogeControllerなどと書いて任意のメソッドに@Route(〜)で動く

## 追加のクラス

- 独自に追加するControllerやEntityは`app/Customize`直下に作成する。
例 `app/Customize/Controller/HogeController`など


## 検索系大まかなロジック(商品検索を例に)
- formFactoryで検索ロジックを全て制御する
- ベースのロジックは内部で制御。個別に書くのはparameterの設定のみ
- formからsearch用のオブジェクトを取り出し、ProductRepositoryに渡す
- ProductRepositoryでSQL作成 ORMapper,QueryBuilderはDoctrineを使用
- 検索したオブジェクトをpagerに渡す。
- 商品自体は商品企画に基づいているのでproduct_idを取得後,product_classから引っ張る
- ソート後、オブジェクトごとviewに渡す

## view側の値のヘルパー処理など

 - `src/Eccube/Form/Type`で制御

プルダウン例
```
'choice_label' => 'NameWithLevel',
'label' => 'admin.product.category',
'placeholder' => 'common.select__all_products',
'required' => false,
'multiple' => false,
'expanded' => false,
'choices' => $this->categoryRepository->getList(null, true),
'choice_value' => function (Category $Category = null) {
    return $Category ? $Category->getId() : null;
},

```

チェックボックス例

```
->add('status', ProductStatusType::class, [
    'label' => 'admin.product.display_status',
    'multiple' => true,
    'required' => false,
    'expanded' => true,
    'data' => $this->productStatusRepository->findBy(['id' => [
        ProductStatus::DISPLAY_SHOW,
        ProductStatus::DISPLAY_HIDE,
    ]]),
])
->add('stock', ChoiceType::class, [
    'label' => 'admin.product.stock',
    'choices' => [
        'admin.product.stock__in_stock' => ProductStock::IN_STOCK,
        'admin.product.stock__out_of_stock' => ProductStock::OUT_OF_STOCK,
    ],
    'expanded' => true,
    'multiple' => true,
])

```
