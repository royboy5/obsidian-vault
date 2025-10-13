## Day 1
---

### Ollama Setup

1. Download and install Ollama from [https://ollama.com](https://ollama.com/) noting that on a PC you might need to have administrator permissions for the install to work properly
2. On a PC, start a Command prompt / Powershell (Press Win + R, type `cmd`, and press Enter). On a Mac, start a Terminal (Applications > Utilities > Terminal).
3. Run `ollama run llama3.2` or for smaller machines try `ollama run llama3.2:1b` - **please note** steer clear of Meta's latest model llama3.3 because at 70B parameters that's way too large for most home computers!

### LLM Development Environment Setup

#### Install Anaconda environment

- Clone repo https://github.com/ed-donner/llm_engineering
- Install [Anaconda](https://docs.anaconda.com/anaconda/install/)
- In project root, Create the environment: `conda env create -f environment.yml`
	- for windows - use the anaconda powershell **Activate** env using this command: `conda activate llms`
- You should see `(llms)` in your prompt, which indicates you've activated your new environment.
- Start Jupyter Lab `jupyter lab`
- Review SETUP markdown files for troubleshooting

#### Anaconda Alternative

- Run `python --version` to find out which version you are on.
	- Should be 3.11+
	- If you need to install Python or install another version, you can download it here: [https://www.python.org/downloads/](https://www.python.org/downloads/)
- Goto project root
- Create new venv
	- `python -m venv llms`
- Activate the venv
	- windows
		- `llms\Scripts\activate`
	- linux / mac
		- `source llms/bin/activate
- Run `python -m pip install --upgrade pip` followed by `pip install -r requirements.txt`
- Start Jupyter Lab
	- `jupyter lab`


## Day 2
---

### Concepts

- Modals
	- frontier - biggest (i.e, claude, chatgpt, gemini, etc)
	- Open Source (ollama)
	- Closed Source (claude, chatgpt, gemini)
- Tools
- Techniques

### 3 ways to use modals
1. Chat interface
	1. needs subscription
2. cloud APIs
	1. pay per req
	2. langchain
	3. Managed AI Services
3. Direct inference - Self hosted
	1. hugging face
	2. ollama

