# Week 1: DevContainer Setup Guide for macOS and Windows

This guide will help you set up a consistent development environment using VS Code DevContainers with Docker Desktop on both macOS and Windows.

## Prerequisites

### For macOS Users
1. **macOS Version**: macOS 10.15 (Catalina) or later
2. **Hardware**: Apple Silicon (M1/M2/M3) or Intel processor
3. **Storage**: At least 10GB free space

### For Windows Users
1. **Windows Version**: Windows 10 version 2004+ (Build 19041+) or Windows 11
2. **Hardware**: 64-bit processor with virtualization support
3. **Storage**: At least 15GB free space
4. **WSL2**: Required for best performance

## Step 1: Install Docker Desktop

### macOS Installation
1. Visit [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop/)
2. Download the appropriate version:
   - **Apple Silicon**: Docker Desktop for Mac with Apple Silicon
   - **Intel**: Docker Desktop for Mac with Intel chip
3. Open the downloaded `.dmg` file
4. Drag Docker to your Applications folder
5. Launch Docker from Applications
6. Follow the setup wizard
7. Verify installation:
   ```bash
   docker --version
   docker run hello-world
   ```

### Windows Installation
1. **Enable WSL2** (if not already enabled):
   ```powershell
   # Run as Administrator in PowerShell
   wsl --install
   # Restart your computer when prompted
   ```

2. Visit [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/)
3. Download Docker Desktop for Windows
4. Run the installer as Administrator
5. During installation, ensure these options are checked:
   - Use WSL 2 instead of Hyper-V (recommended)
   - Add shortcut to desktop
6. Restart your computer when prompted
7. Launch Docker Desktop
8. Verify installation:
   ```powershell
   docker --version
   docker run hello-world
   ```

### Docker Desktop Settings
After installation, configure Docker Desktop:

1. Open Docker Desktop settings (gear icon)
2. **Resources** → **Advanced**:
   - CPUs: At least 2 (4 recommended)
   - Memory: At least 4GB (8GB recommended)
   - Disk image size: At least 32GB
3. **General**:
   - ✅ Start Docker Desktop when you log in
   - ✅ Use Docker Compose V2
4. Apply & Restart

## Step 2: Install Visual Studio Code

1. Download VS Code from [code.visualstudio.com](https://code.visualstudio.com/)
2. Install VS Code following the platform-specific installer
3. Launch VS Code

## Step 3: Install Required VS Code Extensions

1. Open VS Code
2. Open Extensions sidebar (`Cmd+Shift+X` on macOS, `Ctrl+Shift+X` on Windows)
3. Install these extensions:
   - **Dev Containers** (ms-vscode-remote.remote-containers)
   - **Docker** (ms-azuretools.vscode-docker)
   - **Remote Development** (ms-vscode-remote.vscode-remote-extensionpack)

## Step 4: Clone the Course Repository

### Using Git Command Line
```bash
# Choose your preferred directory
cd ~/Documents  # or any directory you prefer

# Clone the repository
git clone https://github.com/your-org/container-course.git
cd container-course
```

### Using VS Code
1. Open VS Code
2. Press `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Windows)
3. Type "Git: Clone" and select it
4. Enter the repository URL
5. Choose a local folder
6. Open the cloned repository

## Step 5: Open in DevContainer

### Method 1: Command Palette
1. Open the repository in VS Code
2. Press `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Windows)
3. Type "Dev Containers: Reopen in Container"
4. Select it and wait for the container to build

### Method 2: Notification
1. VS Code may show a notification: "Folder contains a Dev Container configuration"
2. Click "Reopen in Container"

### Method 3: Status Bar
1. Click the green button in the bottom-left corner
2. Select "Reopen in Container"

## Step 6: Verify Your Environment

Once the container is running, verify everything works:

1. **Open a terminal** in VS Code (`Terminal` → `New Terminal`)
2. **Check Docker**:
   ```bash
   docker --version
   docker-compose --version
   ```
3. **Check Python**:
   ```bash
   python --version
   python -m pip list
   ```
4. **Check Git**:
   ```bash
   git --version
   gh --version
   ```
5. **Test Docker functionality**:
   ```bash
   docker run hello-world
   ```

## Step 7: Test GymCTL (Course Exercise Tool)

The devcontainer includes GymCTL for hands-on exercises:

```bash
# Build gymctl if needed
cd /workspaces/container-course
make -C ../gymctl docker-build

# Run gymctl
docker run --rm -it gymctl:latest list

# Or use it directly if installed
gymctl list
gymctl start docker-basics
```

## Troubleshooting

### macOS Specific Issues

#### "Cannot connect to the Docker daemon"
```bash
# Ensure Docker Desktop is running
open -a Docker

# Check Docker daemon status
docker system info
```

#### Apple Silicon Performance Issues
- Enable Rosetta for x86/amd64 emulation in Docker Desktop settings
- Use `--platform linux/arm64` when possible for native performance

### Windows Specific Issues

#### WSL2 Not Working
```powershell
# Check WSL version
wsl --status

# Update WSL
wsl --update

# Set WSL2 as default
wsl --set-default-version 2
```

#### Virtualization Not Enabled
1. Restart and enter BIOS/UEFI
2. Enable virtualization (Intel VT-x or AMD-V)
3. Save and restart

#### Docker Desktop Starting Issues
```powershell
# Restart Docker service
Restart-Service docker

# Or use PowerShell as Administrator
Stop-Service docker
Start-Service docker
```

### Common Issues (Both Platforms)

#### Port Already in Use
```bash
# Find what's using the port (example for port 8080)
# macOS
lsof -i :8080

# Windows PowerShell
netstat -ano | findstr :8080
```

#### Container Fails to Build
```bash
# Clean Docker cache
docker system prune -a

# Rebuild without cache
docker-compose build --no-cache
```

#### VS Code Can't Find Docker
1. Ensure Docker Desktop is running
2. Restart VS Code
3. Check VS Code Docker extension settings

## Environment Details

Your devcontainer includes:
- **Base Image**: Ubuntu 22.04
- **Docker**: Docker-in-Docker support for container exercises
- **Python**: Version 3.11 with common packages
- **Git & GitHub CLI**: For version control
- **VS Code Extensions**: Docker, Python, YAML support
- **Ports**: 8000, 8080, 5000 auto-forwarded

## Quick Commands Reference

### Docker Commands
```bash
# List running containers
docker ps

# List all containers
docker ps -a

# List images
docker images

# Build an image
docker build -t myapp .

# Run a container
docker run -it ubuntu bash

# Stop all containers
docker stop $(docker ps -q)
```

### DevContainer Commands in VS Code
- **Rebuild Container**: `Cmd/Ctrl+Shift+P` → "Dev Containers: Rebuild Container"
- **Reopen Locally**: `Cmd/Ctrl+Shift+P` → "Dev Containers: Reopen Folder Locally"
- **View Logs**: `Cmd/Ctrl+Shift+P` → "Dev Containers: Show Container Log"

## Next Steps

1. ✅ Verify all components are working
2. 📚 Review the Week 1 exercises
3. 🚀 Start with `docker-basics` exercise:
   ```bash
   gymctl start docker-basics
   ```
4. 💡 Join the course Slack/Discord for help

## Getting Help

- **Course Issues**: Create an issue in the course repository
- **Docker Issues**: [Docker Documentation](https://docs.docker.com/)
- **VS Code Issues**: [VS Code DevContainers Docs](https://code.visualstudio.com/docs/devcontainers/containers)
- **Course Support**: Contact your instructor or TA

## Additional Resources

- [Docker Desktop Documentation](https://docs.docker.com/desktop/)
- [VS Code DevContainers Tutorial](https://code.visualstudio.com/docs/devcontainers/tutorial)
- [WSL Documentation (Windows)](https://docs.microsoft.com/en-us/windows/wsl/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

---

**Note**: This environment is specifically configured for the Container Fundamentals course. All necessary tools and configurations are included in the devcontainer, ensuring a consistent experience across all platforms.