# Simplification Comparison Viewer

tippecanoe のライン単純化の方式によって、河川のラインジオメトリがどう変わるかを地図上で見比べるためのビューアです。
[MapLibre GL JS](https://maplibre.org/) と [PMTiles](https://github.com/protomaps/PMTiles) を使っています。

## ファイル構成

```
viewer/
├── index.html            # ビューア本体
└── pmtiles/
    └── river.pmtiles     # 河川ラインのベクタタイル（z5–14）
```

## レイヤ

`river.pmtiles` には次の3つのレイヤが入っています。いずれも同じ河川データ（`river.geojson`）から、単純化の方式だけを変えて生成したものです。

| レイヤ | 単純化の方式 | tippecanoe のオプション | 表示スタイル |
|---|---|---|---|
| `river-ps` | 単純化なし | `-ps`（`--no-line-simplification`） | 太い灰色（半透明） |
| `river-dp` | Douglas-Peucker | `-S5`（`--simplification=5`） | 赤の破線 |
| `river-vw` | Visvalingam-Whyatt | `-av -S5`（`--visvalingam --simplification=5`） | 青の破線 |

`river-ps` は単純化を行いませんが、タイルの座標精度に丸められる点は他のレイヤと同じです。

## 使い方

PMTiles は HTTP の Range リクエストで読み込むため、`index.html` を `file://` で直接開いても地図に表示されません。
`viewer` ディレクトリで Range リクエストに対応したローカルサーバ（[serve](https://github.com/vercel/serve) など）を起動し、ブラウザで開いてください。

```sh
cd viewer
npx serve
```

ブラウザで http://localhost:3000/ を開きます。

- 地図左上のチェックボックスで、各レイヤの表示・非表示を切り替えられます。複数のレイヤを同時に表示できます。
- 初期表示では `river-ps` だけが表示されます。
- 太い灰色の `river-ps` を一番下に描画しているので、単純化した線（赤・青）が元の線からどれだけずれているかを比べられます。
- 地図の表示位置とズームレベルは URL のハッシュ（`#zoom/lat/lng`）に反映されます。

## クレジット

- 背景地図: © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright)
- 河川データ: © [Overture Maps Foundation](https://overturemaps.org)
