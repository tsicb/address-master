# address-master

GitHub Pages上の住所関連ツールから共通利用する住所マスタです。

## Public URLs

- `https://tsicb.github.io/address-master/zipcode_master.json`
- `https://tsicb.github.io/address-master/municipality_master.json`

同じ `tsicb.github.io` 配下のPagesアプリからは、次のルート相対URLでも参照できます。

- `/address-master/zipcode_master.json`
- `/address-master/municipality_master.json`

## zipcode_master.json

郵便番号・町域・自治体コードの対応マスタです。

各レコード:

```json
{
  "pref": "12",
  "city": "12227",
  "address": "舞浜",
  "zip": "2790031"
}
```

- `pref`: 都道府県コード
- `city`: 自治体コード。北海道・東北などでは先頭0を含まない場合があります
- `address`: 郵便番号マスタ上の町域
- `zip`: ハイフンなし7桁郵便番号
- `address` が空文字のレコードは、市区町村の「以下に掲載がない場合」に対応する郵便番号として利用できます

このファイルは `tsicb/tag-zip-labeler` の `zipcode_master.json` から同期します。
同期用Workflow: `.github/workflows/sync-zipcode-master.yml`

## municipality_master.json

都道府県名・市区町村名と標準地域コードの対応マスタです。

構造:

```json
{
  "version": "2026-10-01",
  "prefectures": {
    "01": "北海道",
    "13": "東京都"
  },
  "municipalities": {
    "01101": "札幌市中央区",
    "13101": "千代田区"
  }
}
```

標準地域コードから次の値を導出できます。

例: `01101`

- 標準地域コード: `01101`
- 県コード1: `1`
- 市区町村コード1: `101`
- PrefCityコード: `1101`
- 都道府県: `北海道`
- 市区町村: `札幌市中央区`

## Design policy

各Pagesアプリは起動時にマスタを先読みし、用途別の検索インデックスをブラウザメモリ上に構築します。
CSV/XLSXの行処理中には外部APIへ問い合わせず、ブラウザ内で照合を完結させることを前提としています。
