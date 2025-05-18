# geth-prysm-node

🖥️ VPS 准备工作
✅ 推荐配置（适用于 Sepolia 测试网）：
系统：Ubuntu 20.04 或 22.04（64位）

CPU：至少 4 核

内存：推荐 8 GB 以上

硬盘：SSD，建议预留 1TB 空间（初期550GB）

🚀 一步步部署指南（VPS端操作）
🔧 Step 1: 登录你的 VPS
bash
复制
编辑
ssh root@your_vps_ip
🧱 Step 2: 安装依赖软件
bash
复制
编辑
sudo apt-get update && sudo apt-get upgrade -y

sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip -y
🐳 Step 3: 安装 Docker 和 Docker Compose
bash
复制
编辑
# 清理旧的 Docker
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove -y $pkg; done

# 添加 Docker 官方源
sudo apt-get install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 安装 Docker 和 Compose 插件
sudo apt update -y && sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# 测试 Docker 是否安装成功
sudo docker run hello-world
sudo systemctl enable docker
sudo systemctl restart docker
📂 Step 4: 创建目录和 JWT 密钥
bash
复制
编辑
mkdir -p /root/ethereum/execution
mkdir -p /root/ethereum/consensus
openssl rand -hex 32 > /root/ethereum/jwt.hex
cat /root/ethereum/jwt.hex  # 确认存在
📝 Step 5: 编写 docker-compose.yml
bash
复制
编辑
cd /root/ethereum
nano docker-compose.yml
