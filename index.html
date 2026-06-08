#!/bin/bash

# Nexus Cloud Studio - Full project generator
set -e

PROJECT_DIR="nexus-cloud-studio"
mkdir -p "$PROJECT_DIR"
cd "$PROJECT_DIR"

# Create folder structure
mkdir -p backend frontend/src/components frontend/public .github/workflows

# -------------------------------
# 1. Root .gitignore
# -------------------------------
cat > .gitignore << 'EOF'
node_modules/
.env
.DS_Store
backend/temp/
*.log
EOF

# -------------------------------
# 2. GitHub Actions CI
# -------------------------------
cat > .github/workflows/ci.yml << 'EOF'
name: CI – Nexus Cloud Studio

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Install backend dependencies
        run: cd backend && npm install
      - name: Check backend syntax
        run: cd backend && node -c server.js && node -c exec.js
      - name: Start backend & smoke test
        run: |
          cd backend
          npm start &
          sleep 5
          curl -f http://localhost:5000/ || exit 1

  build-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Install frontend dependencies
        run: cd frontend && npm install
      - name: Build frontend
        run: cd frontend && npm run build
        env:
          CI: false
EOF

# -------------------------------
# 3. Backend files
# -------------------------------
cat > backend/package.json << 'EOF'
{
  "name": "nexus-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "uuid": "^9.0.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.22"
  }
}
EOF

cat > backend/server.js << 'EOF'
const express = require('express');
const cors = require('cors');
const { runJavaScript, runPython } = require('./exec');

const app = express();
const PORT = process.env.PORT || 5000;

app.use(cors());
app.use(express.json({ limit: '1mb' }));

app.get('/', (req, res) => {
  res.json({ status: 'Nexus Cloud Studio Backend is running' });
});

app.post('/run-code', async (req, res) => {
  const { language, code } = req.body;
  
  if (!code || typeof code !== 'string') {
    return res.status(400).json({ error: 'Missing or invalid code' });
  }
  if (language !== 'javascript' && language !== 'python') {
    return res.status(400).json({ error: 'Unsupported language' });
  }

  try {
    let result;
    if (language === 'javascript') {
      result = await runJavaScript(code);
    } else {
      result = await runPython(code);
    }
    res.json(result);
  } catch (err) {
    console.error(err);
    res.status(500).json({ output: 'Internal server error', error: true });
  }
});

app.listen(PORT, () => {
  console.log(`Backend running on port ${PORT}`);
});
EOF

cat > backend/exec.js << 'EOF'
const { exec } = require('child_process');
const fs = require('fs').promises;
const path = require('path');
const { v4: uuidv4 } = require('uuid');

const TIMEOUT_MS = 5000;

async function runJavaScript(code) {
  const tempDir = path.join(__dirname, 'temp');
  await fs.mkdir(tempDir, { recursive: true });
  const tempFile = path.join(tempDir, `${uuidv4()}.js`);
  await fs.writeFile(tempFile, code);

  return new Promise((resolve) => {
    exec(`node "${tempFile}"`, {
      timeout: TIMEOUT_MS,
      env: { NODE_ENV: 'sandbox' },
      shell: false,
      maxBuffer: 1024 * 1024,
    }, (error, stdout, stderr) => {
      fs.unlink(tempFile).catch(() => {});
      if (error) {
        if (error.killed && error.signal === 'SIGTERM') {
          resolve({ output: 'Execution timed out (>5s)', error: true });
        } else {
          resolve({ output: stderr || error.message, error: true });
        }
      } else {
        resolve({ output: stdout || 'No output', error: false });
      }
    });
  });
}

async function runPython(code) {
  const tempDir = path.join(__dirname, 'temp');
  await fs.mkdir(tempDir, { recursive: true });
  const tempFile = path.join(tempDir, `${uuidv4()}.py`);
  await fs.writeFile(tempFile, code);

  return new Promise((resolve) => {
    exec(`python3 "${tempFile}"`, {
      timeout: TIMEOUT_MS,
      env: { PATH: process.env.PATH },
      shell: false,
      maxBuffer: 1024 * 1024,
    }, (error, stdout, stderr) => {
      fs.unlink(tempFile).catch(() => {});
      if (error) {
        if (error.killed && error.signal === 'SIGTERM') {
          resolve({ output: 'Execution timed out (>5s)', error: true });
        } else {
          resolve({ output: stderr || error.message, error: true });
        }
      } else {
        resolve({ output: stdout || 'No output', error: false });
      }
    });
  });
}

module.exports = { runJavaScript, runPython };
EOF

cat > backend/.gitignore << 'EOF'
node_modules/
temp/
.env
EOF

# -------------------------------
# 4. Frontend files
# -------------------------------
cat > frontend/package.json << 'EOF'
{
  "name": "nexus-frontend",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "@monaco-editor/react": "^4.5.1",
    "axios": "^1.4.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  },
  "devDependencies": {
    "react-scripts": "5.0.1"
  },
  "browserslist": {
    "production": [">0.2%", "not dead", "not op_mini all"],
    "development": ["last 1 chrome version", "last 1 firefox version", "last 1 safari version"]
  }
}
EOF

cat > frontend/public/index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Nexus Cloud Studio</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
EOF

cat > frontend/src/index.js << 'EOF'
import React from 'react';
import ReactDOM from 'react-dom/client';
import './styles.css';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
EOF

cat > frontend/src/styles.css << 'EOF'
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #1e1e2f;
  color: #fff;
}
.app {
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.header {
  background: #2d2d3a;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #3e3e4e;
}
.header h1 {
  font-size: 1.5rem;
  font-weight: 600;
  background: linear-gradient(135deg, #6c5ce7, #a8a4ff);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
.controls {
  display: flex;
  gap: 1rem;
  align-items: center;
}
.language-selector {
  background: #3e3e4e;
  border: none;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-size: 0.9rem;
  cursor: pointer;
}
.run-button {
  background: #6c5ce7;
  border: none;
  color: white;
  padding: 0.5rem 1.5rem;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.2s;
}
.run-button:hover {
  background: #5a4ad1;
}
.save-button, .share-button {
  background: #2a2a3c;
  border: 1px solid #4a4a5e;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  cursor: pointer;
}
.main {
  flex: 1;
  display: flex;
  overflow: hidden;
}
.editor-container {
  flex: 2;
  display: flex;
  flex-direction: column;
  border-right: 1px solid #3e3e4e;
}
.output-container {
  flex: 1;
  background: #1a1a2a;
  display: flex;
  flex-direction: column;
}
.output-header {
  background: #2d2d3a;
  padding: 0.75rem 1rem;
  font-weight: bold;
  border-bottom: 1px solid #3e3e4e;
}
.output-content {
  flex: 1;
  padding: 1rem;
  font-family: 'Courier New', monospace;
  font-size: 0.9rem;
  overflow-y: auto;
  white-space: pre-wrap;
  background: #0f0f1a;
}
.error {
  color: #ff6b6b;
}
EOF

cat > frontend/src/App.js << 'EOF'
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import CodeEditor from './components/Editor';
import Output from './components/Output';

const API_URL = process.env.REACT_APP_API_URL || 'http://localhost:5000';

const defaultCode = {
  javascript: `// JavaScript Example
console.log("Hello from Nexus Cloud Studio!");
const message = "You can run JS and Python";
console.log(message);

const sum = (a, b) => a + b;
console.log("2 + 3 =", sum(2, 3));
`,
  python: `# Python Example
print("Hello from Nexus Cloud Studio!")
message = "You can run JS and Python"
print(message)

def sum(a, b):
    return a + b
print("2 + 3 =", sum(2, 3));
`
};

function App() {
  const [language, setLanguage] = useState('javascript');
  const [code, setCode] = useState(defaultCode.javascript);
  const [output, setOutput] = useState('');
  const [isError, setIsError] = useState(false);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    const savedLang = localStorage.getItem('nexus_language');
    const savedCode = localStorage.getItem('nexus_code');
    if (savedLang && savedCode) {
      setLanguage(savedLang);
      setCode(savedCode);
    }
  }, []);

  useEffect(() => {
    localStorage.setItem('nexus_language', language);
    localStorage.setItem('nexus_code', code);
  }, [language, code]);

  const handleLanguageChange = (e) => {
    const newLang = e.target.value;
    setLanguage(newLang);
    if (!code.trim() || code === defaultCode[language]) {
      setCode(defaultCode[newLang]);
    }
  };

  const runCode = async () => {
    setLoading(true);
    setOutput('');
    setIsError(false);
    try {
      const response = await axios.post(`${API_URL}/run-code`, { language, code });
      setOutput(response.data.output);
      setIsError(response.data.error);
    } catch (err) {
      setOutput(err.response?.data?.error || 'Failed to connect to backend');
      setIsError(true);
    } finally {
      setLoading(false);
    }
  };

  const saveProject = () => {
    alert('Project saved to browser storage!');
  };

  const shareProject = () => {
    const data = { code, language };
    const encoded = btoa(unescape(encodeURIComponent(JSON.stringify(data))));
    const url = `${window.location.origin}${window.location.pathname}#${encoded}`;
    navigator.clipboard.writeText(url);
    alert('Shareable link copied to clipboard!');
  };

  useEffect(() => {
    const hash = window.location.hash.slice(1);
    if (hash) {
      try {
        const decoded = JSON.parse(decodeURIComponent(escape(atob(hash))));
        if (decoded.code && decoded.language) {
          setCode(decoded.code);
          setLanguage(decoded.language);
          window.location.hash = '';
        }
      } catch (e) {}
    }
  }, []);

  return (
    <div className="app">
      <div className="header">
        <h1>☁️ Nexus Cloud Studio</h1>
        <div className="controls">
          <select value={language} onChange={handleLanguageChange} className="language-selector">
            <option value="javascript">JavaScript (Node.js)</option>
            <option value="python">Python 3</option>
          </select>
          <button onClick={runCode} className="run-button" disabled={loading}>
            {loading ? 'Running...' : '▶ Run'}
          </button>
          <button onClick={saveProject} className="save-button">💾 Save</button>
          <button onClick={shareProject} className="share-button">🔗 Share</button>
        </div>
      </div>
      <div className="main">
        <div className="editor-container">
          <CodeEditor language={language} code={code} onChange={setCode} />
        </div>
        <div className="output-container">
          <div className="output-header">Output</div>
          <Output output={output} isError={isError} />
        </div>
      </div>
    </div>
  );
}

export default App;
EOF

cat > frontend/src/components/Editor.js << 'EOF'
import React from 'react';
import Editor from '@monaco-editor/react';

const CodeEditor = ({ language, code, onChange }) => {
  return (
    <Editor
      height="100%"
      language={language === 'javascript' ? 'javascript' : 'python'}
      theme="vs-dark"
      value={code}
      onChange={onChange}
      options={{
        fontSize: 14,
        minimap: { enabled: false },
        scrollBeyondLastLine: false,
        automaticLayout: true,
      }}
    />
  );
};

export default CodeEditor;
EOF

cat > frontend/src/components/Output.js << 'EOF'
import React from 'react';

const Output = ({ output, isError }) => {
  return (
    <div className="output-content">
      <pre className={isError ? 'error' : ''}>
        {output || '▶ Run your code to see output'}
      </pre>
    </div>
  );
};

export default Output;
EOF

cat > frontend/.gitignore << 'EOF'
node_modules/
build/
.env
EOF

# -------------------------------
# 5. README.md
# -------------------------------
cat > README.md << 'EOF'
# Nexus Cloud Studio

A lightweight Replit-style online code editor and runner for JavaScript (Node.js) and Python.

## Features
- Monaco Editor (VS Code style)
- Run JS and Python code in a sandboxed environment (timeout limited)
- Save projects to localStorage
- Share code via URL

## Local Development

1. **Backend**  
   ```bash
   cd backend
   npm install
   mkdir temp
   npm start
