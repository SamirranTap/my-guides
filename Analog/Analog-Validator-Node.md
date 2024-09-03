# 1. Introduction

The Analog project provides a blockchain protocol for utilizing the decentralized ledger of time data. This guide aims to help you deploy and manage a validator node for the Analog blockchain network.

# 2. Prerequisites

Before starting the setup, ensure that your server or machine meets the following requirements:

Operating System: Ubuntu 20.04 LTS or later.

CPU: 4 or more cores.

RAM: At least 16 GB.

Disk Space: SSD with at least 500 GB of free space.

Network: Stable internet connection with at least 100 Mbps bandwidth.

# 3. Setting Up Dependencies
   
## Step 1: Update System Packages

Make sure your system packages are up to date:

```
sudo apt update && sudo apt upgrade -y
```
## Step 2: Install Essential Packages

Install the basic tools required for building and managing the node:

```
sudo apt install -y build-essential git curl jq
```

## Step 3: Install Docker

Docker is needed to containerize the Analog node. Follow these commands to install Docker:

```
# Remove any older versions of Docker
sudo apt remove docker docker-engine docker.io containerd runc

# Install required dependencies
sudo apt-get install -y ca-certificates curl gnupg lsb-release

# Add Docker’s official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up the Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Verify Docker installation
docker --version
```

## Step 4: Install Rust

Rust is needed to build the Analog node. Follow these steps to install Rust:

```
# Install Rust using rustup
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Follow the on-screen instructions to complete the installation.
# You may need to log out and log back in or restart your shell.

# Verify Rust installation
rustc --version
```

# 4. Cloning the Analog Repository

Clone the official Analog repository from GitHub:

```
git clone https://github.com/analog/analog
cd analog
```

# 5. Building the Analog Node
   
Now, let's build the Analog node. This process might take some time depending on your system's resources.

```
cargo build --release
```
After the build process completes, you should find the analog-node binary in the target/release directory.

# 6. Running the Analog Node

To run the node, you need to create a configuration file and set up the environment properly.

## Step 1: Create a Directory for Node Data

Create a directory to store node data and configuration:

```
mkdir -p ~/analog-node-data
```
## Step 2: Generate a Validator Keypair

Generate a new validator keypair using the Analog CLI tool:

```
./target/release/analog-keygen new --outfile ~/analog-node-data/validator-keypair.json
```

This command will create a new keypair and save it in the specified location. Make sure to back up this key securely!

## Step 3: Start the Node

Start the node using the following command:

```
./target/release/analog-node --identity ~/analog-node-data/validator-keypair.json --ledger ~/analog-node-data/ledger --rpc-port 8899
--identity: Path to the validator keypair file.
--ledger: Path where the ledger data will be stored.
--rpc-port: Port for RPC connections.
```

## Step 4: Run the Node in Background

To run the node in the background, you can use tmux or screen:

```
# Install tmux if not already installed
sudo apt install -y tmux

# Start a new tmux session
tmux new -s analog-node

# Inside the tmux session, start the node
./target/release/analog-node --identity ~/analog-node-data/validator-keypair.json --ledger ~/analog-node-data/ledger --rpc-port 8899

# Detach from the tmux session by pressing `Ctrl + b` followed by `d`.
```

To re-attach to the tmux session, use:

```
tmux attach -t analog-node
```

# 7. Monitoring and Maintenance

To ensure your validator is operating correctly, you should regularly monitor its status:

Check logs:

```
tail -f ~/analog-node-data/logs/analog.log
```

Use monitoring tools or set up alerts for resource usage (CPU, memory, disk space).

# 8. Updating the Node

To update your node to the latest version:

Navigate to the Analog repository:

```
cd analog
```
Pull the latest changes:

```
git pull origin main
```

Rebuild the node:

```
cargo build --release
```
Restart the node with the updated binary.

# 9. Security Best Practices

Firewall Configuration: Make sure to only open necessary ports (e.g., 8899 for RPC).

Regular Updates: Keep your system and dependencies up to date.

Backup: Regularly back up your keypair and ledger data.

# 10. Conclusion

Following this guide, you should have a fully functioning Analog validator node running on your server. Ensure you regularly monitor its performance and update the software to remain in sync with the network.

If you encounter any issues or have questions, please consult the official Analog documentation or reach out to the SamirranTap support team.
