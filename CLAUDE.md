# レキサク プロジェクトメモ

## Supabase

- **プロジェクトID**: `sunxpjbdvidftukiutxg`
- **URL**: `https://sunxpjbdvidftukiutxg.supabase.co`
- **DB更新はSupabase MCPツールで行う**
  - anon キーは READ のみ（RLS により WRITE 不可）
  - DB の INSERT / UPDATE / DELETE は `mcp__bfe9f6e5-f2ba-4118-93c1-94b6f248b478__execute_sql` を使う
  - `project_id`: `sunxpjbdvidftukiutxg`

## 主なテーブル

| テーブル | 用途 |
|----------|------|
| `eras` | 時代マスタ（code, name_ja, sort_order, avg_age, avg_life 等） |
| `spots` | スポット一覧 |
| `spot_eras` | スポット × 時代の中間テーブル |
| `spot_tags` | スポット × タグの中間テーブル |
| `tags` | タグマスタ |
| `prefectures` | 都道府県マスタ |

## eras テーブルの code 一覧（sort_order 順）

| code | name_ja |
|------|---------|
| `geological` | 地質時代 |
| `paleolithic` | 旧石器 |
| `jomon` | 縄文 |
| `yayoi` | 弥生 |
| `kofun` | 古墳 |
| `nara` | 奈良 |
| `heian` | 平安 |
| `kamakura` | 鎌倉 |
| `muromachi` | 室町 |
| `edo` | 江戸 |
| `meiji` | 明治 |
| `taisho` | 大正 |
| `showa_early` | 昭和前期 |
| `showa_mid` | 昭和中期 |
| `showa_late` | 昭和後期 |
| `heisei` | 平成 |
| `gendai` | 令和 |

## スポット評価ルール

- Google の星評価 **4.0 以上** → レキサク評価 **3**
- Google の星評価 **4.0 未満** → レキサク評価 **2**

## スポット追加時の必須入力フィールド

施設を追加する際は、以下のフィールドをすべて埋めること（フロントエンドで表示される項目）:

| フィールド | 内容 |
|------------|------|
| `name` | 施設名 |
| `name_kana` | 施設名ふりがな |
| `description` | 施設説明（80文字程度） |
| `address` | 住所 |
| `prefecture_id` | 都道府県ID |
| `latitude` / `longitude` | 緯度・経度 |
| `access_info` | アクセス方法（交通手段・所要時間） |
| `open_hours` | 開館時間・休館日 |
| `admission_fee` | 入館料（無料の場合は「無料」と記載） |
| `is_free` | 無料かどうか（boolean） |
| `website_url` | 公式サイトURL |
| `star_rating` | レキサク評価（上記評価ルール参照） |
| `status` | `true`（公開） |

## フロントエンド

- メインページ: `index.html`
- 管理画面: `admin.html`（パスワード: rekisaku2025）
- 15財閥ページ: `zaibatsu.html`
- 15財閥リンクは明治以降の時代のみ表示（`zaibatsu-bar` div を JS で show/hide）

## スポットカード表示ルール

- 関東外（東京・神奈川・千葉・埼玉・茨城・栃木・群馬以外）のスポットは背景を赤（`outside-kanto` クラス）にする
  - 関東外判定: `s.prefectures` が null、または `KANTO_PREFS`（上記7都県）に含まれない場合
