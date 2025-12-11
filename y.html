# 完整 GitHub 專案：chess-endgame-solver

這個檔案包含一個可直接放到 GitHub 的完整專案範例：前端（React）、後端（Node/Express）、Dockerfile 與 docker-compose，以及 README、.gitignore。

> 提示：你可以直接把下面的檔案內容複製到對應檔案，或使用本專案的 zip（若需要我可幫你產生 zip）。

---

## 專案結構

```
chess-endgame-solver/
├─ frontend/
│  ├─ public/
│  │  ├─ index.html
│  │  └─ stockfish.js    # optional: stockfish webworker wrapper (若你要前端分析)
│  ├─ src/
│  │  ├─ App.jsx
│  │  ├─ index.js
│  │  └─ index.css
│  ├─ package.json
│  └─ Dockerfile.frontend
├─ backend/
│  ├─ index.js
│  └─ package.json
├─ .gitignore
├─ docker-compose.yml
├─ Dockerfile.backend
└─ README.md
```

---

## 檔案內容（完整）

### .gitignore

```
node_modules/
frontend/node_modules/
backend/node_modules/
build/
dist/
.tb/
tb/
.env
.DS_Store
*.log
```

---

### README.md

```md
# chess-endgame-solver

一個可部署的西洋棋殘局破解範例專案（前端 React + 後端 Node/Express + Stockfish + Syzygy tablebase 支援）。

## 功能

- 前端提供 FEN 輸入、棋盤預覽、呼叫後端分析 API（`/api/analyze`）與 tablebase probe（`/api/probe`）。
- 後端使用 Stockfish binary（UCI）做分析，並透過 `setoption name SyzygyPath value /tb` 尝试读取 Syzygy tablebases（如果有 mount 到容器）。
- Docker 與 docker-compose 範例，方便在主機或 VPS 上部署。

## 快速啟動（使用 Docker Compose）

1. 把程式碼放到機器上。
2. 若要使用 tablebase，下載 Syzygy tablebase 並放到專案根目錄下的 `tb/`（或其他目錄，調整 `docker-compose.yml`）。
3. 執行：

```bash
docker-compose up --build
```

4. 前端：`http://localhost:8080`，後端 API：`http://localhost:3000/api/...`

## 本地開發

- 前端：
  ```bash
  cd frontend
  npm install
  npm run build   # 或 npm start 開發模式
  ```

- 後端：
  ```bash
  cd backend
  npm install
  node index.js
  ```

## 注意

- Syzygy tablebases 很大（數 GB），不會包含在 repo 中。若需要精確 tablebase 結果，請下載並 mount 到容器中。
- 若把服務公開到公網，請加上認證與 rate limiting。
```

---

### docker-compose.yml

```yaml
version: '3.8'
services:
  backend:
    build:
      context: .
      dockerfile: Dockerfile.backend
    environment:
      - PORT=3000
      - SYZYGY_PATH=/tb
    volumes:
      - ./tb:/tb # 若你下載 syzygy tablebases，把本地 ./tb mount 進容器
    ports:
      - '3000:3000'
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile.frontend
    ports:
      - '8080:80'
    depends_on:
      - backend
```

---

### Dockerfile.backend

```dockerfile
FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y nodejs npm curl ca-certificates git build-essential && rm -rf /var/lib/apt/lists/*

# 安裝 stockfish（預設 apt 的版本）
RUN apt-get update && apt-get install -y stockfish && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY backend/package.json ./
RUN npm install --production
COPY backend ./
EXPOSE 3000
CMD [ "node", "index.js" ]
```

---

### Dockerfile.frontend

```dockerfile
FROM node:18 as build
WORKDIR /app
COPY frontend/package.json ./
RUN npm install
COPY frontend ./
RUN npm run build

FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

### backend/package.json

```json
{
  "name": "chess-endgame-backend",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

### backend/index.js

```js
// backend/index.js
const express = require('express');
const { spawn } = require('child_process');
const path = require('path');
const app = express();
const port = process.env.PORT || 3000;

const STOCKFISH_PATH = process.env.STOCKFISH_PATH || 'stockfish';
const SYZYGY_PATH = process.env.SYZYGY_PATH || '/tb';

function runStockfish(commands, onLine, onClose) {
  const proc = spawn(STOCKFISH_PATH, []);
  proc.stdout.setEncoding('utf8');
  proc.stderr.setEncoding('utf8');

  proc.stdout.on('data', (data) => {
    data.toString().split(/
?
/).filter(Boolean).forEach(onLine);
  });
  proc.stderr.on('data', (d) => console.error('stockfish stderr:', d.toString()));
  proc.on('close', (code) => onClose && onClose(code));

  // feed commands
  commands.forEach((c) => proc.stdin.write(c + '
'));
  // ensure we quit after some time
  setTimeout(() => {
    try { proc.stdin.write('quit
'); } catch (e) {}
  }, 20000);
}

app.get('/api/analyze', (req, res) => {
  const fen = req.query.fen;
  if (!fen) return res.status(400).json({ error: 'missing fen' });

  const lines = [];
  const commands = [
    'uci',
    `setoption name SyzygyPath value ${SYZYGY_PATH}`,
    `position fen ${fen}`,
    'go movetime 800'
  ];

  let sent = false;
  runStockfish(commands, (line) => {
    lines.push(line);
    if (line.startsWith('bestmove') && !sent) {
      sent = true;
      res.json({ raw: lines.join('
') });
    }
  }, (code) => {
    if (!sent) res.json({ raw: lines.join('
'), code });
  });
});

app.get('/api/probe', (req, res) => {
  const fen = req.query.fen;
  if (!fen) return res.status(400).json({ error: 'missing fen' });

  const lines = [];
  const commands = [
    'uci',
    `setoption name SyzygyPath value ${SYZYGY_PATH}`,
    `position fen ${fen}`,
    // depth 1 是為了讓 engine 嘗試讀取 tablebase info
    'go depth 1'
  ];

  let sent = false;
  runStockfish(commands, (line) => {
    lines.push(line);
    // 有些 stockfish build 會在 info 裡面輸出 tb/wdl/dtz
    if ((/dtz|tb|tablebase|WDL/i).test(line) && !sent) {
      // 盡快回傳初步資料
      sent = true;
      res.json({ hint: 'tablebase-info-line', line, raw: lines.join('
') });
    }
    if (line.startsWith('bestmove') && !sent) {
      sent = true;
      res.json({ raw: lines.join('
') });
    }
  }, (code) => {
    if (!sent) res.json({ raw: lines.join('
'), code });
  });
});

// serve frontend build when present
app.use(express.static(path.join(__dirname, '..', 'frontend', 'build')));
app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, '..', 'frontend', 'build', 'index.html'));
});

app.listen(port, () => console.log(`Backend listening on ${port}`));
```

---

### frontend/package.json

```json
{
  "name": "chess-endgame-frontend",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "chess.js": "1.0.0-rc.2"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
```

> 注意：如果你偏好 Vite 或 Next.js 可自行替換；此範例以常見 create-react-app（react-scripts）流程為例。

---

### frontend/public/index.html

```html
<!doctype html>
<html lang="zh-Hant">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Chess Endgame Solver</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

---

### frontend/public/stockfish.js

```
// 佔位檔案：若你要前端使用 stockfish webworker，請放置 worker 檔或 wasm wrapper
// 可參考 https://github.com/niklasf/stockfish.wasm 或其他 stockfish.js 專案

// 這裡不包含完整的 worker 實作；後端已能提供分析 API（推薦在 server 端跑）。
```

---

### frontend/src/index.js

```js
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';
import './index.css';

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

---

### frontend/src/index.css

```css
body { font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; margin:0; padding:0; }
.btn { background:#0ea5e9; color:white; padding:.5rem 1rem; border-radius:6px; }
textarea { font-family: monospace; }
```

---

### frontend/src/App.jsx

```jsx
import React, { useState, useEffect } from 'react';
import { Chess } from 'chess.js';

export default function App() {
  const [fen, setFen] = useState('8/8/8/8/8/8/8/8 w - - 0 1');
  const [chess, setChess] = useState(new Chess());
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    const c = new Chess();
    if (c.load(fen)) setChess(c);
  }, [fen]);

  async function callApi(path) {
    setLoading(true);
    setResult(null);
    try {
      const res = await fetch(`/api/${path}?fen=${encodeURIComponent(fen)}`);
      const j = await res.json();
      setResult(j);
    } catch (err) {
      setResult({ error: String(err) });
    } finally {
      setLoading(false);
    }
  }

  function sample1() {
    // KQ v KR
    setFen('8/8/8/8/8/8/4k3/4KQ2 w - - 0 1');
  }

  function renderBoard() {
    const board = chess.board();
    return (
      <div style={{ display: 'grid', gridTemplateColumns: 'repeat(8, 48px)', border: '2px solid #333', width: 48*8 }}>
        {board.map((rank, ry) =>
          rank.map((sq, rx) => {
            const dark = (rx + ry) % 2 === 1;
            const piece = sq ? (sq.color === 'w' ? sq.type.toUpperCase() : sq.type) : null;
            const unicode = piece ? pieceUnicode(piece) : '';
            return (
              <div key={`${rx}-${ry}`} title={`${rx}-${ry}`} style={{ width:48, height:48, display:'flex', alignItems:'center', justifyContent:'center', background: dark ? '#6b7280' : '#f3f4f6' }}>
                <span style={{ fontSize: '24px' }}>{unicode}</span>
              </div>
            );
          })
        )}
      </div>
    );
  }

  function pieceUnicode(piece) {
    const map = { p:'♟', r:'♜', n:'♞', b:'♝', q:'♛', k:'♚', P:'♙', R:'♖', N:'♘', B:'♗', Q:'♕', K:'♔' };
    return map[piece] || '';
  }

  return (
    <div style={{ padding:24 }}>
      <h1>西洋棋殘局破解</h1>
      <div style={{ display:'flex', gap:24 }}>
        <div>
          <div>FEN</div>
          <textarea value={fen} onChange={(e) => setFen(e.target.value)} style={{ width:400, height:120 }} />
          <div style={{ marginTop:8, display:'flex', gap:8 }}>
            <button className="btn" onClick={() => callApi('analyze')} disabled={loading}>呼叫後端分析</button>
            <button className="btn" onClick={() => callApi('probe')} disabled={loading}>查 Tablebase</button>
            <button className="btn" onClick={sample1}>範例</button>
          </div>

          <div style={{ marginTop:12 }}>
            <div style={{ fontWeight:600 }}>結果</div>
            <pre style={{ height:200, overflow:'auto', background:'#f3f4f6', padding:8 }}>{result ? JSON.stringify(result, null, 2) : '(尚無)'}</pre>
          </div>
        </div>

        <div>
          <div style={{ marginBottom:8 }}>棋盤預覽</div>
          {renderBoard()}
        </div>
      </div>

      <div style={{ marginTop:18, color:'#6b7280' }}>
        <div>備註：後端分析使用系統的 Stockfish binary（或你指定的 STOCKFISH_PATH）。若要更高精度的 tablebase probe，請下載 Syzygy tablebases 並 mount 到容器的 /tb。</div>
      </div>
    </div>
  );
}
```

---

## 後續我可以幫你做

- 把前端換成更漂亮的互動棋盤（drag & drop、move animations、變化樹）
- 把 backend 的 probe 改成直接呼叫 syzygy probe binary（C 程式）並回傳結構化 WDL/DTZ
- 產生一個可下載的 zip，或直接幫你把 repo push 到你的 GitHub（你需提供 repo 名稱與權限）

---

如果你要我直接**產生 zip**（把上述所有檔案打包）或**把專案推到 GitHub**，回覆告訴我你要哪一項：

- 回覆 `zip` → 我會把專案打包並上傳給你（提供下載連結）。
- 回覆 `github` → 告訴我你的 GitHub 使用者名稱與 repo 名稱（例如 `youruser/chess-endgame-solver`），我會給你完整的 `git` 指令與必要的步驟說明（或若你授權我，我可以協助自動化推送）。


*以上程式碼為可直接上手的完整範例；若你希望我把 Stockfish 改為特定版本、或直接整合 syzygy probe binary，請告訴我我就立刻替你改寫。*
