DeepSeek Coder on MLX-LM (macOS Apple Silicon, 16GB OK)

This guide explains how to install and run DeepSeek Coder 1.3B locally using MLX-LM on a Mac with Apple Silicon (M1/M2/M3) and 16GB RAM. We’ll use pyenv to manage Python, clone and install MLX-LM, and convert HuggingFace models for local inference.

⸻

✅ Prerequisites
	•	Apple Silicon Mac (M1/M2/M3)
	•	At least 16GB RAM
	•	Homebrew

⸻

1. Python Environment Setup (via pyenv)

brew install pyenv
pyenv install 3.11.9
pyenv local 3.11.9

# Enable pyenv in your shell (once or in ~/.zshrc)
eval "$(pyenv init --path)"
eval "$(pyenv init -)"

Verify:

python --version
# Should show: Python 3.11.9


⸻

2. Clone and Install MLX-LM

git clone https://github.com/ml-explore/mlx-lm.git
cd mlx-lm
python -m venv .venv
source .venv/bin/activate
pip install -e .


⸻

3. Convert DeepSeek Model

This downloads and converts the DeepSeek model to MLX format with quantization:

python -m mlx_lm convert \
  --hf-path deepseek-ai/deepseek-coder-1.3b-instruct \
  --mlx-path ~/mlx_models/deepseek-1.3b-chat-mlx \
  -q

If you see a FileNotFoundError about safetensors, use deepseek-coder-1.3b-instruct (not base) which has safetensors.

⸻

4. Run the Chat CLI

python -m mlx_lm chat --model ~/mlx_models/deepseek-1.3b-chat-mlx

This opens a REPL interface with these commands:

'q' to exit
'r' to reset the chat
'h' to show commands


⸻

5. (Optional) Use a Prompt Template

Try this to simulate Codex-style completions:

python -m mlx_lm chat \
  --model ~/mlx_models/deepseek-1.3b-chat-mlx \
  --temp 0.2 \
  --max-tokens 512

Prompt example:

Write a Swift function that recursively finds all files in a directory.


⸻

🧼 Clean Exit Tip

If you see MLX-LM errors after model conversion, ensure:
	•	You used the instruct variant, not base
	•	You use python -m mlx_lm chat (not mlx_lm.chat directly)

⸻

🗂 Related Links
	•	https://huggingface.co/deepseek-ai/deepseek-coder-1.3b-instruct
	•	https://github.com/ml-explore/mlx-lm
