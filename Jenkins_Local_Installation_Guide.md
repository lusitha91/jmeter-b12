# Jenkins Local Installation Guide

Jenkins is a popular open-source automation server that helps teams build, test, and deploy applications. This guide covers various methods to install Jenkins locally on your machine.

## Table of Contents
- [System Requirements](#system-requirements)
- [Installation Methods](#installation-methods)
  - [1. Docker Installation (Recommended)](#1-docker-installation-recommended)
  - [2. Native Package Installation](#2-native-package-installation)
  - [3. WAR File Installation](#3-war-file-installation)
  - [4. Package Manager Installation](#4-package-manager-installation)
- [Post-Installation Setup](#post-installation-setup)
- [Troubleshooting](#troubleshooting)
- [Comparison of Methods](#comparison-of-methods)

## System Requirements

### Minimum Hardware Requirements
- **RAM**: 4 GB+ (8 GB recommended)
- **Disk Space**: 10 GB+ free space
- **CPU**: 2+ cores recommended

### Software Requirements
- **Java**: Java 17 or Java 21 (LTS versions)
  - Jenkins 2.401+ requires Java 11 minimum
  - Jenkins 2.463+ requires Java 17 minimum
- **Operating System**: Windows, macOS, Linux, or Unix-like systems

---

## Installation Methods

### 1. Docker Installation (Recommended)

Docker is the easiest and most portable way to run Jenkins locally.

#### Prerequisites
- Docker Desktop installed and running
- Basic knowledge of Docker commands

#### Steps

1. **Pull the official Jenkins Docker image**:
   ```bash
   docker pull jenkins/jenkins:lts
   ```

2. **Create a Docker volume for Jenkins data**:
   ```bash
   docker volume create jenkins_home
   ```

3. **Run Jenkins container**:
   ```bash
   docker run -d \
     --name jenkins \
     -p 8080:8080 \
     -p 50000:50000 \
     -v jenkins_home:/var/jenkins_home \
     jenkins/jenkins:lts
   ```

4. **Access Jenkins**:
   - Open browser and navigate to `http://localhost:8080`

5. **Get initial admin password**:
   ```bash
   docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
   ```

#### Pros
- ✅ Easy to install and remove
- ✅ Isolated environment
- ✅ Consistent across different operating systems
- ✅ Easy to upgrade/downgrade versions
- ✅ No conflicts with system Java

#### Cons
- ❌ Requires Docker knowledge
- ❌ Additional resource overhead
- ❌ May have networking complexity for advanced setups

---

### 2. Native Package Installation

Install Jenkins directly on your operating system using official installers.

#### Windows Installation

1. **Download Jenkins**:
   - Visit [jenkins.io](https://www.jenkins.io/download/)
   - Download the Windows installer (.msi file)

2. **Install Jenkins**:
   - Run the downloaded .msi file
   - Follow the installation wizard
   - Jenkins will install as a Windows service

3. **Access Jenkins**:
   - Jenkins automatically starts after installation
   - Open browser to `http://localhost:8080`

4. **Find initial password**:
   - Location: `C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword`

#### macOS Installation

1. **Download Jenkins**:
   - Download the macOS installer (.pkg file) from jenkins.io

2. **Install Jenkins**:
   - Run the .pkg file
   - Follow installation prompts
   - Jenkins installs to `/Applications/Jenkins`

3. **Start Jenkins**:
   ```bash
   sudo launchctl start org.jenkins-ci
   ```

4. **Access Jenkins**:
   - Browser: `http://localhost:8080`
   - Password location: `/Users/Shared/Jenkins/Home/secrets/initialAdminPassword`

#### Linux Installation (Ubuntu/Debian)

1. **Update package index**:
   ```bash
   sudo apt update
   ```

2. **Install Java**:
   ```bash
   sudo apt install openjdk-17-jdk
   ```

3. **Add Jenkins repository**:
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
     /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
     https://pkg.jenkins.io/debian binary/ | sudo tee \
     /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```

4. **Install Jenkins**:
   ```bash
   sudo apt update
   sudo apt install jenkins
   ```

5. **Start Jenkins**:
   ```bash
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```

6. **Access Jenkins**:
   - Browser: `http://localhost:8080`
   - Password: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

#### Pros
- ✅ Native performance
- ✅ Integrated with system services
- ✅ Direct file system access
- ✅ Official support and updates

#### Cons
- ❌ Platform-specific installation steps
- ❌ May conflict with system Java
- ❌ Harder to clean up completely
- ❌ System-wide installation

---

### 3. WAR File Installation

Run Jenkins directly using the executable WAR file.

#### Prerequisites
- Java 17 or 21 installed and configured
- Command line access

#### Steps

1. **Download Jenkins WAR file**:
   - Visit [jenkins.io/download](https://www.jenkins.io/download/)
   - Download `jenkins.war`

2. **Create Jenkins directory**:
   ```bash
   mkdir ~/jenkins
   cd ~/jenkins
   ```

3. **Run Jenkins**:
   ```bash
   java -jar jenkins.war
   ```

4. **Custom configuration** (optional):
   ```bash
   java -Djenkins.install.runSetupWizard=false \
        -DJENKINS_HOME=~/jenkins_home \
        -jar jenkins.war --httpPort=8080
   ```

5. **Access Jenkins**:
   - Browser: `http://localhost:8080`
   - Password location: `~/.jenkins/secrets/initialAdminPassword`

#### Pros
- ✅ Simple and lightweight
- ✅ Easy to configure custom options
- ✅ Portable across systems
- ✅ Easy to stop (Ctrl+C)
- ✅ Multiple versions can coexist

#### Cons
- ❌ Manual Java management
- ❌ No automatic startup
- ❌ Command line dependent
- ❌ Manual updates required

---

### 4. Package Manager Installation

Use system package managers for streamlined installation.

#### Homebrew (macOS)

1. **Install Homebrew** (if not installed):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Install Jenkins**:
   ```bash
   brew install jenkins-lts
   ```

3. **Start Jenkins**:
   ```bash
   brew services start jenkins-lts
   ```

4. **Access Jenkins**:
   - Browser: `http://localhost:8080`

#### Chocolatey (Windows)

1. **Install Chocolatey** (if not installed):
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   ```

2. **Install Jenkins**:
   ```powershell
   choco install jenkins
   ```

3. **Access Jenkins**:
   - Jenkins starts automatically as service
   - Browser: `http://localhost:8080`

#### APT (Ubuntu/Debian)
See Linux installation in [Native Package Installation](#linux-installation-ubuntudebian)

#### YUM/DNF (RHEL/CentOS/Fedora)

1. **Add Jenkins repository**:
   ```bash
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io-2023.key
   ```

2. **Install Jenkins**:
   ```bash
   sudo yum install java-17-openjdk jenkins
   # or for newer systems:
   sudo dnf install java-17-openjdk jenkins
   ```

3. **Start Jenkins**:
   ```bash
   sudo systemctl start jenkins
   ```

#### Pros
- ✅ Automated dependency management
- ✅ Easy updates
- ✅ Integration with system package management
- ✅ Familiar workflow for developers

#### Cons
- ❌ Package manager specific
- ❌ May not have latest version immediately
- ❌ Less control over installation options

---

## Post-Installation Setup

After installing Jenkins using any method:

1. **Initial Setup Wizard**:
   - Navigate to `http://localhost:8080`
   - Enter the initial admin password
   - Choose "Install suggested plugins" or "Select plugins to install"
   - Create first admin user
   - Configure Jenkins URL

2. **Essential Plugins** (if not installed):
   - Git plugin
   - Pipeline plugin
   - Blue Ocean (modern UI)
   - Build Pipeline plugin

3. **Configure Tools**:
   - Go to "Manage Jenkins" → "Global Tool Configuration"
   - Configure JDK, Git, Maven, etc.

4. **Security Configuration**:
   - Enable security features
   - Configure user authentication
   - Set up authorization strategy

---

## Troubleshooting

### Common Issues

#### Port 8080 Already in Use
```bash
# Find process using port 8080
netstat -ano | findstr :8080  # Windows
lsof -i :8080                 # macOS/Linux

# Use different port
java -jar jenkins.war --httpPort=9090
# or for Docker:
docker run -p 9090:8080 jenkins/jenkins:lts
```

#### Java Version Issues
```bash
# Check Java version
java -version

# Install correct Java version (Ubuntu/Debian)
sudo apt install openjdk-17-jdk

# Set JAVA_HOME (if needed)
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
```

#### Permission Issues (Linux/macOS)
```bash
# Fix Jenkins home directory permissions
sudo chown -R jenkins:jenkins /var/lib/jenkins

# Or for WAR file installation
sudo chmod -R 755 ~/.jenkins
```

#### Memory Issues
```bash
# Increase memory allocation
java -Xmx2g -jar jenkins.war

# For Docker
docker run -e JAVA_OPTS="-Xmx2g" jenkins/jenkins:lts
```

### Getting Help
- **Documentation**: [jenkins.io/doc](https://www.jenkins.io/doc/)
- **Community**: [community.jenkins.io](https://community.jenkins.io/)
- **Issues**: [issues.jenkins.io](https://issues.jenkins.io/)

---

## Comparison of Methods

| Method | Ease of Install | Ease of Upgrade | Isolation | Performance | Best For |
|--------|----------------|----------------|-----------|-------------|----------|
| **Docker** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | Development, Testing |
| **Native Package** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | Production, Long-term use |
| **WAR File** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Quick testing, Custom setups |
| **Package Manager** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | Platform-integrated setups |

### Recommendations

- **For Beginners**: Docker installation
- **For Development**: Docker or WAR file
- **For Production**: Native package installation
- **For Learning**: WAR file (more transparent)
- **For CI/CD Pipelines**: Docker with orchestration

---

## Conclusion

Choose the installation method that best fits your needs:

- Use **Docker** for easy setup and isolation
- Use **Native packages** for production environments
- Use **WAR file** for maximum control and learning
- Use **Package managers** for integrated system management

Remember to:
- Keep Jenkins updated regularly
- Backup your Jenkins configuration
- Secure your Jenkins instance
- Monitor resource usage

Happy building with Jenkins! 🚀
