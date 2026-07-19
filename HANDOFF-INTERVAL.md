# interval 專案交接說明

> 這個分支(`claude/interval-deployment-ecommerce-y9st93`)的 **`interval/` 資料夾**是一套完整、
> 已通過 build 與手機版 RWD 驗證的全端應用,目標是搬進 <https://github.com/LaiQuan-tech/interval>。
> 與 agec-pitch 簡報本身無關,請勿把此分支合併進 agec-pitch 的 main。

## 為什麼放在這裡?

這個 Claude 雲端工作階段只有 agec-pitch 的推送權限,而且:
- `LaiQuan-tech/interval` repo 目前是**空的**(你本機的 interval 資料夾還沒 push)
- 這個環境的網路政策擋掉了 Supabase / Vercel / Railway / Resend 的 API(403),無法直接遠端佈建

所以完整程式碼先放在這裡,並附上「一鍵佈建腳本」讓部署在你本機 30 秒內完成。

## step 1 — 把程式碼搬進 interval repo(你的電腦上執行)

```bash
git clone --branch claude/interval-deployment-ecommerce-y9st93 \
  https://github.com/LaiQuan-tech/agec-pitch interval-src

mkdir interval-app && cp -R interval-src/interval/. interval-app/
cd interval-app
git init -b main
git add -A
git commit -m "feat: interval 全端應用(官網+會員+電商後台+AI 報價)"
git remote add origin https://github.com/LaiQuan-tech/interval.git
git push -u origin main
```

> 你本機原本的 interval 設計資料夾:先完成上面步驟,再把設計稿的頁面/素材
> 搬進 `web/src/app` 與 `web/public` 後 commit,或整包丟給 Claude 幫你整合。

## step 2 — 一鍵佈建(Supabase + Vercel + Railway + Resend)

```bash
npm install

SUPABASE_ACCESS_TOKEN=sbp_...(你的 token) \
VERCEL_TOKEN=...(你的 token) \
RAILWAY_TOKEN=...(你的 token) \
RESEND_API_KEY=re_...(你的 key) \
ANTHROPIC_API_KEY=sk-ant-...(選填,AI 報價用) \
ADMIN_EMAIL=你的email ADMIN_PASSWORD=一組安全密碼 \
node scripts/provision.mjs
```

前置(各一次):Vercel 裝 [GitHub App](https://vercel.com/account/git)、Railway [連結 GitHub](https://railway.app/account),兩者都授權 `LaiQuan-tech/interval`。

完成後:**push main → Vercel(網站)+ Railway(API)自動部署**,GitHub Actions 自動跑 CI。
後台在 `/admin`(用 ADMIN_EMAIL 登入)。細節見 `interval/README.md`。

## 也可以全部交給 Claude 遠端完成

到 <https://claude.ai/code> 的環境設定:
1. 把 **interval repo 加入環境的 sources**
2. 網路政策允許 `api.supabase.com`、`api.vercel.com`、`backboard.railway.app`、`api.resend.com`
3. 開新對話說「執行 interval 的佈建」即可
