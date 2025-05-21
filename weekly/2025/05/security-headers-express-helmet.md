# 🛡️ セキュリティ強化ヘッダ 基本ノート（Express + Helmet編）

## ✅ 押さえておくべきポイント（6つの主要ヘッダ）

| ヘッダ名                     | 用途                         | 推奨値・例                        | 防げる攻撃・効果                          |
|------------------------------|------------------------------|-----------------------------------|-------------------------------------------|
| Content-Security-Policy      | 読み込み元を制限             | `script-src 'self'`              | XSSの防止                                 |
| X-Content-Type-Options       | MIMEタイプのスニッフィング防止 | `nosniff`                        | 不正なContent-Typeの実行を防ぐ            |
| X-Frame-Options              | iframe埋め込み制限           | `SAMEORIGIN` or `DENY`           | クリックジャッキング防止                  |
| Strict-Transport-Security    | HTTPSを強制                  | `max-age=63072000; includeSubDomains` | HTTPSを強制（HSTS）                   |
| Referrer-Policy              | リファラー制御               | `no-referrer-when-downgrade`     | リファラー情報漏洩の抑止                   |
| Permissions-Policy           | ブラウザ機能の制御           | `geolocation=(), microphone=()`  | カメラ・マイク等の利用制限                |

---

## 🛠 Express.jsでの実装（Helmet使用）

### 📦 必要パッケージのインストール

```bash
yarn add helmet
# または npm
npm install helmet
```

---

### 🧑‍💻 実装サンプル（基本）

```ts
import express from 'express';
import helmet from 'helmet';

const app = express();

// Helmetの全体適用（推奨）
app.use(helmet());

// Content-Security-Policyなどを細かくカスタマイズしたい場合
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      objectSrc: ["'none'"],
      upgradeInsecureRequests: [],
    },
  })
);

// サンプルエンドポイント
app.get('/', (req, res) => {
  res.send('Hello, secure world!');
});

app.listen(3000, () => {
  console.log('App is running on https://localhost:3000');
});
```

---

## 🔍 補足事項

- `helmet()` 単体で使うと、安全なデフォルト設定が有効になる（全ヘッダONに近い）
- `helmet.contentSecurityPolicy()` など個別設定も可能
- 開発中はCSPの厳しさでJSが読み込めなくなることもあるので注意
- HTTPS必須のヘッダ（HSTSなど）はローカル環境では無効化されることもある

---

## 📚 参考リンク

- [Helmet 公式ドキュメント](https://helmetjs.github.io/)
- [MDN - HTTP セキュリティヘッダ](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers)
