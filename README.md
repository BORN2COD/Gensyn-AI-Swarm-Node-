# GENSYN SWARM NODE COMPLETE SETUP GUIDE ✨

## INTRODUCTION
Gensyn is a decentralized machine learning network that rewards compute providers for contributing their resources to train AI models. It recently raised $55M from Andreessen Horowitz (@a16z), and it operates through testnet Swarm nodes which you can run on:

- Your local PC (Mac/Linux)
- A cloud VPS (e.g., Hetzner, Contabo)
- Rented GPU providers like @hyperbolic_labs

> **IMPORTANT:** Low-spec machines are not supported and may crash during setup. You need:
> - Minimum 16 GB RAM
> - Python >= 3.10
> - Optionally: RTX 3090 / 4090 / A100 / H100 for GPU mode

---

## PART 1: SYSTEM SETUP 🚀

### 1.1 UPDATE SYSTEM
```bash
sudo apt update && sudo apt upgrade -y
```

### 1.2 INSTALL ESSENTIAL TOOLS
```bash
sudo apt install -y curl git wget nano tmux htop nvme-cli lz4 jq make gcc clang \
build-essential autoconf automake pkg-config libssl-dev libleveldb-dev \
libgbm1 bsdmainutils ncdu unzip tar
```

### 1.3 INSTALL PYTHON & PIP
```bash
sudo apt install -y python3 python3-pip python3-venv python3-dev
python3 --version
```

### 1.4 INSTALL NODE.JS, NPM & YARN
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
node -v && npm -v

sudo npm install -g yarn
yarn --version
```

### 1.5 INSTALL DOCKER (Optional - Future Feature)
```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

---

## PART 2: CLONE & START SWARM NODE 🚗

### 2.1 CLONE THE RL-SWARM REPO
```bash
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm
```

### 2.2 CREATE A SCREEN SESSION (VPS RECOMMENDED)
```bash
sudo apt install screen -y
screen -S gensyn
```

### 2.3 SET UP PYTHON ENVIRONMENT
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2.4 INSTALL MODAL LOGIN DEPENDENCIES
```bash
cd modal-login
yarn install
yarn upgrade && yarn add next@latest && yarn add viem@latest
cd ..
```

### 2.5 RUN THE NODE
```bash
./run_rl_swarm.sh
```

### 2.6 TESTNET & LOGIN PROMPTS
- You will be asked: `Would you like to connect to the Testnet? [Y/n]` ➔ Press **Y**
- A browser window should pop up ➔ Login with your email and OTP.
  - If no browser: Visit http://localhost:3000 manually.

Once logged in, note down your **ORG_ID** and press **N** for the Hugging Face upload question.

> ✅ Node is now running and logs will appear.

To detach screen:
```bash
Ctrl + A then press D
```
To re-attach:
```bash
screen -r gensyn
```

---

## PART 3: VPS ACCESS TO LOGIN PAGE 🌐

### 3.1 ENABLE PORT 3000 & TUNNEL WITH CLOUDFLARED
```bash
sudo apt install ufw -y
sudo ufw allow 3000/tcp
sudo ufw enable

wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
cloudflared --version

cloudflared tunnel --url http://localhost:3000
```
Copy the public URL and open it in your browser to complete login.

---

## PART 4: TROUBLESHOOTING ⚖️

### 4.1 MAC OOM ERRORS (MEMORY ISSUES)
```bash
nano ~/.zshrc
```
Paste:
```bash
export PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.0
export PYTORCH_ENABLE_MPS_FALLBACK=1
```
Then:
```bash
source ~/.zshrc
```

### 4.2 GPU CONFIG TUNING
- Edit the YAML config file:
```bash
nano ~/rl-swarm/hivemind_exp/configs/gpu/grpo-qwen-2.5-0.5b-deepseek-r1.yaml
# OR on Mac:
nano ~/rl-swarm/hivemind_exp/configs/mac/grpo-qwen-2.5-0.5b-deepseek-r1.yaml
```
Change:
```yaml
vllm_gpu_memory_utilization: 0.2
```
to:
```yaml
vllm_gpu_memory_utilization: 0.4
```

Ctrl+X → Y → Enter to save

Then restart terminal and node.

### 4.3 SAVING `swarm.pem` FOR FUTURE
```bash
scp USERNAME@IP:~/rl-swarm/swarm.pem ~/swarm.pem
```

### 4.4 START NEXT DAY
```bash
cd rl-swarm
source .venv/bin/activate
./run_rl_swarm.sh
```

---

## PART 5: OPTIONAL - HUGGINGFACE ACCESS TOKEN 🧡
1. Sign up on [Hugging Face](https://huggingface.co/)
2. Go to Settings > Access Tokens
3. Create a **Write Token** and save it securely

---

## LINKS & RESOURCES 🔗
- Gensyn GitHub: https://github.com/gensyn-ai/rl-swarm
- Troubleshooting Docs: https://github.com/gensyn-ai/rl-swarm/tree/brian-address-cpu-only-crashes?tab=readme-ov-file#troubleshooting
- Telegram: https://t.me/cryptogg

---

## DONE ✅
Congratulations, your Gensyn Swarm Node is now up and running!

> If you encounter any issues:
> - Check the logs: `screen -r gensyn`
> - Re-run setup with correct config
> - Contact support or raise a GitHub issue

[Join the Community!](https://x.com/PhaResearcher)

For more detailed troubleshooting, refer to the official docs:
[Gensyn ai](https://github.com/gensyn-ai/rl-swarm?tab=readme-ov-file)

Happy contributing to decentralized AI! 🤖
