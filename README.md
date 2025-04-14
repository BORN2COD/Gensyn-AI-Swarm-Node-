# Gensyn-AI-Swarm-Node-
This guide helps you easily set up a Gensyn Testnet Swarm Node on your computer, VPS, or rented GPU. Gensyn is a decentralized machine learning network, supported by a16z with $55 million in funding. Follow these step-by-step instructions to join the testnet and help shape the future of AI.

Gensyn is a new machine learning network that has raised $55 million from a16z (Andreessen Horowitz). It provides scalable AI solutions and focuses on decentralized training. You can participate in the Gensyn Testnet Swarm Node by setting up a node on your local computer, a VPS, or rented GPUs. Whether you want to use a CPU-only node or a GPU node for faster processing, this guide will help you with the necessary steps.

By following this guide, you will be able to:
1. Run a Swarm Node on your local PC for testing and development.
2. Set up a Swarm Node on a VPS to help the decentralized network.
3. Use rented GPUs from cloud providers like Hyperbolic to run your node with more power.
⸻

🧰 REQUIREMENTS BEFORE YOU START

Resource	Minimum	Recommended
OS	Ubuntu 20.04+ / MacOS	Ubuntu 22.04 LTS
CPU	4 cores	8+ cores
RAM	8GB	16GB+
GPU	Optional	NVIDIA A100 / RTX 4090 (CUDA support)
Python	>= 3.10	3.11
Disk	20 GB free	SSD for better performance
Internet	Stable	Stable and fast



⸻

✅ STEP 1: CONNECT TO VPS / DEVICE

On VPS:

ssh username@your_vps_ip

Skip this if you’re using your local PC or Mac.

⸻

🔧 STEP 2: INSTALL SYSTEM DEPENDENCIES

🐧 Linux / WSL:

sudo apt update && sudo apt upgrade -y
sudo apt install -y python3 python3-pip python3-venv python3-dev curl wget git lsof nano screen build-essential htop tmux jq make gcc clang autoconf automake pkg-config libssl-dev libleveldb-dev libgbm1 unzip tar

🍎 MacOS (using Homebrew):

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install python node yarn screen git
corepack enable



⸻

🧱 STEP 3: INSTALL NODE.JS, YARN & DOCKER

Install Node.js & Yarn:

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g yarn

Install Docker (Linux):

for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin



⸻

🔄 STEP 4: CLONE RL-SWARM REPOSITORY

git clone https://github.com/gensyn-ai/rl-swarm
cd rl-swarm

Create a screen session to keep your node running in background:

screen -S gensyn



⸻

🐍 STEP 5: SETUP PYTHON VIRTUAL ENVIRONMENT

python3 -m venv .venv
source .venv/bin/activate



⸻

📦 STEP 6: INSTALL JAVASCRIPT DEPENDENCIES

cd modal-login
yarn install
yarn upgrade
yarn add next@latest viem@latest
cd ..



⸻

🚀 STEP 7: RUN THE NODE

./run_rl_swarm.sh

You’ll be prompted with:

Would you like to connect to the Testnet? [Y/n]
→ Enter Y

⸻

🔗 Browser Login Step

A browser window should open automatically. If not, especially on VPS, follow below:

🌐 Make login UI accessible (on VPS only):

sudo apt install ufw -y
sudo ufw allow 3000/tcp
sudo ufw enable
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
cloudflared tunnel --url http://localhost:3000

Then, from your local machine, open the generated cloudflared URL.
→ Login with your email and enter OTP
→ Copy the ORG_ID displayed in terminal
→ When asked about HuggingFace model push → Type N

⸻

🧪 STEP 8: VERIFY & DETACH

To keep your node running:

Detach the screen session:

Ctrl + A → then D

To re-attach anytime:

screen -r gensyn



⸻

🚨 COMMON ISSUES & SOLUTIONS

Issue	Fix
Browser doesn’t open on VPS	Use cloudflared tunnel as shown above
Node crashes due to low specs	Use a GPU server with at least 16GB RAM
Permission errors	Run command with sudo, or ensure you’re in correct directory
Missing packages	Run: sudo apt install -y build-essential python3-dev
GPU not detected	Make sure NVIDIA drivers + CUDA installed (or rent from Hyperbolic)
Port 3000 not accessible	Run: sudo ufw allow 3000/tcp and check with lsof -i :3000
Docker errors	Restart Docker: sudo systemctl restart docker
Python errors	Ensure you’re using source .venv/bin/activate before running commands



⸻

💡 TIPS & NOTES
	•	You don’t need a GPU to participate in the testnet, but GPU nodes earn better.
	•	For GPU rentals, platforms like hyperbolic_labs or RunPod work great.
	•	Keep your node online as long as possible for better uptime and potential rewards.
	•	You can monitor logs inside the screen session or setup logging scripts.

⸻

Want this in PDF / Notion format or need help running on Hyperbolic? Just let me know — I’ll set it up for you.
_____

[Join the Community!](https://x.com/PhaResearcher)

For more detailed troubleshooting, refer to the official docs:
[Gensyn ai](https://github.com/gensyn-ai/rl-swarm?tab=readme-ov-file)

⸻

Happy Coding! 💻
Feel free to reach out if you need help or have any questions.
