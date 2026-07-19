# interval 專案交接說明

> ✅ **程式碼已經推進 <https://github.com/LaiQuan-tech/interval> 的 `main`**(本 session 後續已取得
> interval repo 權限,原本「手動搬運」的步驟不需要了)。
> 此分支 `interval/` 資料夾僅作為開發紀錄備份,請勿把此分支合併進 agec-pitch 的 main。

## 目前狀態

- ✅ 完整應用已在 interval repo main:官網店面 + 會員系統 + `/admin` 電商後台 + AI 報價 + Railway api + CI
- ✅ GitHub Actions CI 已隨 push 自動執行(lint / typecheck / build)
- ⬜ 雲端資源(Supabase / Vercel / Railway)尚未佈建 —— 這個 Claude 環境的網路政策擋掉了
  四個平台的 API(403),需要在你本機跑一次腳本,或開通網路後交給 Claude

## 剩最後一步 — 一鍵佈建(你的電腦上執行)

```bash
git clone https://github.com/LaiQuan-tech/interval && cd interval
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

完成後:**push main → Vercel(網站)+ Railway(API)自動部署**。
後台在 `/admin`(用 ADMIN_EMAIL 登入)。細節見 interval repo 的 `README.md`。

## 也可以全部交給 Claude 遠端完成

到 <https://claude.ai/code> 的環境設定,把網路政策放行
`api.supabase.com`、`api.vercel.com`、`backboard.railway.app`、`api.resend.com`,
然後開新對話說「執行 interval 的佈建」即可(interval repo 已在 session 範圍內)。

## 你本機的 claude design 網站

先 `git pull` interval repo(裡面已有完整應用),再把設計稿的頁面/素材搬進
`web/src/app` 與 `web/public` 後 commit push,或整包丟給 Claude 幫你整合 ——
**不要直接 force push 覆蓋 main**,會把會員/後台/AI 報價整套蓋掉。
