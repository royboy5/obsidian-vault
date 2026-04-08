### 🐍 Python AI/LLM Setup

#### 🏗️ Environment
- [ ] **Initialize Git:** `git init`
- [ ] **Ignore Files:** Create `.gitignore` (Add: `venv/`, `.env`, `.ipynb_checkpoints/`, `__pycache__/`)
- [ ] Clone `https://github.com/ed-donner/llm_engineering`
- [ ] install `uv` https://docs.astral.sh/uv/getting-started/installation/
	- [ ] to upgrade - `uv self update`
	- [ ] run `uv --version` to verify install
- [ ] run `uv sync` to install packages
- [ ] add api key to .env (if necessary)
- [ ] 

#### 📦 Dependencies
- [ ] **Upgrade pip:** `pip install --upgrade pip`
- [ ] **Core AI Libs:** `pip install openai langchain python-dotenv`
- [ ] **Notebook Support:** `pip install ipykernel` (Allows running .ipynb files inside VS Code)
- [ ] **Freeze:** `pip freeze > requirements.txt`

#### 🔐 Secrets
- [ ] Create `.env` file for API keys
- [ ] Add `OPENAI_API_KEY=sk-...`
- [ ] Create `.env.example` (copy keys *without* values)

#### 🧪 Verification
- [ ] Create `test_setup.py`
- [ ] Import openai and print "Setup Complete"
- [ ] Run: `python test_setup.py`