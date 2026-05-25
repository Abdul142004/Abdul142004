# 🚀 DevOps Practical Exam — Complete Guide
**Avinash Avhale | Roll No: 24 | TY9-A | SAKEC**

---

## 📋 Table of Contents
1. [Topic 1 — Git Init, Add, Commit](#topic-1--git-init-add-commit)
2. [Topic 2 — Push to GitHub](#topic-2--push-to-github)
3. [Topic 3 — Branches and Merge](#topic-3--branches-and-merge)
4. [Topic 4 — Pull Request and Merge Conflict](#topic-4--pull-request-and-merge-conflict)
5. [Topic 5 — GitHub Actions](#topic-5--github-actions)
6. [Topic 6 — CI/CD Pipeline](#topic-6--cicd-pipeline)
7. [Topic 7 — Automated Testing](#topic-7--automated-testing)
8. [Topic 8 — Dockerfile and Docker Image](#topic-8--dockerfile-and-docker-image)
9. [Topic 9 — Docker Compose](#topic-9--docker-compose)
10. [Topic 10 — Ansible Playbook](#topic-10--ansible-playbook)
11. [Quick Reference](#-quick-reference---all-commands)
12. [Viva Questions](#-viva-questions---all-topics)

---

## Topic 1 — Git Init, Add, Commit

### 🧠 Theory
Git is a **distributed version control system** that tracks changes in source code. Think of it as a photographer that takes snapshots of your code at different points in time. Every snapshot is called a **commit**.

- **git init** → Creates a new empty repository (like opening a new notebook)
- **git add** → Stages files (like arranging files in front of camera)
- **git commit** → Saves permanent snapshot (like clicking the photo)

### ⚡ Commands
```bash
# Set your identity first (one time only)
git config --global user.name "Avinash Avhale"
git config --global user.email "avinash.18121@sakec.ac.in"

# Verify config
git config --list

# Create folder and go inside
mkdir myproject
cd myproject

# Initialize Git
git init

# Create a file
echo "Hello World" > file.txt

# Stage all files
git add .

# Stage one specific file
git add file.txt

# Save snapshot
git commit -m "first commit"

# See history
git log

# Check what changed
git status
```

### 📝 What Each Command Does
| Command | Meaning |
|---------|---------|
| `git init` | Creates empty Git repo in folder |
| `git add .` | Stages ALL files for commit |
| `git add filename` | Stages ONE file |
| `git commit -m "msg"` | Saves permanent snapshot |
| `git log` | Shows history of all commits |
| `git status` | Shows what files changed |

---

## Topic 2 — Push to GitHub

### 🧠 Theory
GitHub is a **cloud platform** that hosts Git repositories. Your laptop = local repo. GitHub = remote repo. **Pushing** sends commits from laptop → GitHub so anyone can access from anywhere.

### ⚡ Commands
```bash
# Link local repo to GitHub
git remote add origin https://github.com/AviAvhale/myproject.git

# Rename branch to main
git branch -M main

# First time push
git push -u origin main

# All future pushes
git push

# Check remote connection
git remote -v
```

### 📝 What Each Command Does
| Command | Meaning |
|---------|---------|
| `git remote add origin URL` | Links laptop repo to GitHub repo |
| `git branch -M main` | Renames branch from master to main |
| `git push -u origin main` | First push — sets GitHub as default |
| `git push` | All future pushes |

---

## Topic 3 — Branches and Merge

### 🧠 Theory
A branch is an **independent copy** of your code. Like a photocopy of your exam paper — try new answers on photocopy, if good stick it on original (merge).

- **main branch** = original paper
- **feature branch** = photocopy
- **merge** = sticking photocopy onto original

### ⚡ Commands
```bash
# Create new branch
git branch feature

# Switch to feature branch
git checkout feature

# Create AND switch in one command
git checkout -b feature

# See all branches (* = current)
git branch

# Make changes on feature branch
echo "feature change" >> file.txt
git add .
git commit -m "feature branch change"

# Go back to main
git checkout main

# Check file (feature changes NOT here yet)
cat file.txt

# Merge feature into main
git merge feature

# Check file again (NOW feature changes are here!)
cat file.txt

# Delete branch after merging
git branch -d feature

# Push to GitHub
git push
```

> ⚠️ **Remember:** `>>` adds to file, `>` overwrites file!

---

## Topic 4 — Pull Request and Merge Conflict

### 🧠 Theory
**Merge Conflict** = When two branches edit the **same line** of same file differently. Git gets confused and asks YOU to decide which version to keep.

**Pull Request** = A request on GitHub to review and merge one branch into another before it goes into main.

### ⚡ Commands
```bash
# Create conflict — on main branch
git checkout main
echo "this is main line" > conflict.txt
git add .
git commit -m "main branch conflict file"

# Switch to feature branch
git checkout feature

# Edit SAME file with different content
echo "this is feature line" > conflict.txt
git add .
git commit -m "feature branch conflict file"

# Go back to main and try to merge
git checkout main
git merge feature
# ⚡ CONFLICT APPEARS HERE!

# Open file to fix conflict
notepad conflict.txt

# Fix, save, then:
git add .
git commit -m "resolved merge conflict"

# Push feature branch for Pull Request
git push origin feature
```

### 🔧 Conflict Markers Explained
```
<<<<<<< HEAD
this is main line        ← YOUR version (current branch)
=======
this is feature line     ← THEIR version (incoming branch)
>>>>>>> feature
```
**Fix:** Delete all markers, keep only the line you want, save file, then `git add .` and `git commit`

### Pull Request Steps (on GitHub website)
1. Go to your repo on GitHub
2. Click **"Pull requests"** tab
3. Click **"New pull request"**
4. Set base: `main` ← compare: `feature`
5. Click **"Create pull request"**
6. Click **"Merge pull request"** → **"Confirm merge"**

---

## Topic 5 — GitHub Actions

### 🧠 Theory
GitHub Actions is a **CI/CD tool built into GitHub**. It automatically runs tasks (workflows) whenever you push code. Think of it as a **robot servant** 🤖 — you give instructions in a YAML file and it runs them on every push.

### 📁 File Location
```
myproject/
└── .github/
    └── workflows/
        └── build.yml    ← Create this file
```

### ⚡ Commands
```bash
# Create folders
mkdir -p .github/workflows

# Create YAML file
notepad .github/workflows/build.yml

# After saving, push to GitHub
git add .
git commit -m "added workflow"
git push
```

### 📄 build.yml
```yaml
name: Hello World

on:
  push:
  workflow_dispatch:

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Say Hello
        run: echo "Hello, World!"
```

### 📝 YAML Explained
| Line | Meaning |
|------|---------|
| `name: Hello World` | Name shown in Actions tab |
| `on: push:` | Run on every code push |
| `workflow_dispatch:` | Allow manual run from GitHub |
| `runs-on: ubuntu-latest` | Use temporary Ubuntu machine |
| `uses: actions/checkout@v4` | Download repo code |
| `run: echo "Hello, World!"` | Execute this command |

---

## Topic 6 — CI/CD Pipeline

### 🧠 Theory
- **CI (Continuous Integration)** = Auto build and test on every push ✅
- **CD (Continuous Deployment)** = Auto deploy after successful build ✅
- **CI/CD Pipeline** = Complete automated flow: push code → build → test → deploy

### 📁 Files Needed
```
topic6/
├── index.html
└── .github/workflows/deploy.yml
```

### 📄 index.html
```html
<!DOCTYPE html>
<html>
<head>
    <title>My CI/CD Page</title>
</head>
<body>
    <h1>Hello from CI/CD Pipeline!</h1>
    <p>Deployed automatically by GitHub Actions</p>
    <p>Student: Avinash Avhale | Roll No: 24</p>
</body>
</html>
```

### 📄 deploy.yml
```yaml
name: Deploy Static HTML

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Show build message
        run: echo "Building and deploying HTML page!"

      - name: Display HTML content
        run: cat index.html
```

### ⚡ Commands
```bash
# New folder on Desktop
cd ~/Desktop
mkdir topic6
cd topic6
git init
code .

# After creating files in VS Code
git remote add origin https://github.com/AviAvhale/topic6.git
git branch -M main
git add .
git commit -m "added html and cicd workflow"
git push -u origin main
```

---

## Topic 7 — Automated Testing

### 🧠 Theory
**Automated testing** = Running test cases automatically without manual effort on every push. `pytest` is a Python testing framework. If any test fails → workflow fails → bad code is blocked!

### 📁 Files Needed
```
topic7/
├── app.py
├── test_app.py
└── .github/workflows/test.yml
```

### 📄 app.py
```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b
```

### 📄 test_app.py
```python
from app import add, subtract, multiply

def test_add():
    assert add(2, 3) == 5

def test_subtract():
    assert subtract(10, 4) == 6

def test_multiply():
    assert multiply(3, 4) == 12
```

### 📄 test.yml
```yaml
name: Run Tests

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install pytest
        run: pip install pytest

      - name: Run tests
        run: pytest test_app.py -v
```

### ⚡ Commands
```bash
cd ~/Desktop
mkdir topic7
cd topic7
git init
code .

# After creating all files
git remote add origin https://github.com/AviAvhale/topic7.git
git branch -M main
git add .
git commit -m "added automated testing"
git push -u origin main
```

---

## Topic 8 — Dockerfile and Docker Image

### 🧠 Theory
Docker solves **"works on my machine"** problem. A Docker **image** is a self-contained package with app + all dependencies. A **container** is a running instance of image. A **Dockerfile** has instructions to build the image.

| Term | Meaning | Analogy |
|------|---------|---------|
| Image | Template with app + dependencies | Cookie cutter 🍪 |
| Container | Running instance of image | Actual cookie |
| Dockerfile | Build instructions | Recipe |

### 📁 Files Needed
```
topic8/
├── app.py
├── requirements.txt
└── Dockerfile
```

### 📄 app.py
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return '''
    <html>
    <body>
        <h1>Hello from Docker!</h1>
        <p>Student: Avinash Avhale</p>
        <p>Roll No: 24</p>
        <p>Running inside a Docker container!</p>
    </body>
    </html>
    '''

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
```

### 📄 requirements.txt
```
flask
```

### 📄 Dockerfile
```dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### 📝 Dockerfile Explained
| Line | Meaning |
|------|---------|
| `FROM python:3.9` | Use Python 3.9 as base image |
| `WORKDIR /app` | Create /app folder inside container |
| `COPY requirements.txt .` | Copy requirements into container |
| `RUN pip install -r requirements.txt` | Install Flask inside container |
| `COPY . .` | Copy all files into container |
| `CMD ["python", "app.py"]` | Run app.py when container starts |

### ⚡ Commands
```bash
cd ~/Desktop
mkdir topic8
cd topic8
code .

# After creating all 3 files — build image
docker build -t myapp .

# Run container
docker run -p 5000:5000 myapp

# Open browser → http://localhost:5000

# Other useful commands
docker images              # List all images
docker ps                  # List running containers
docker stop <id>           # Stop container
docker rmi myapp           # Delete image
docker rm -f $(docker ps -aq)  # Delete ALL containers
```

> ⚠️ **Make sure Docker Desktop is running before any docker command!**

---

## Topic 9 — Docker Compose

### 🧠 Theory
Docker Compose runs **multiple containers together**. Real apps need web server + database + cache etc. One command `docker-compose up` starts everything defined in `docker-compose.yml`.

### 📁 Files Needed
```
topic9/
├── app.py
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

> app.py, requirements.txt, Dockerfile = same as Topic 8!

### 📄 docker-compose.yml
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root123
```

### 📝 docker-compose.yml Explained
| Line | Meaning |
|------|---------|
| `services:` | List of all containers |
| `web:` | First container — Flask app |
| `build: .` | Build from local Dockerfile |
| `ports: 5000:5000` | Expose port to laptop |
| `db:` | Second container — MySQL |
| `image: mysql` | Use official MySQL from Docker Hub |
| `MYSQL_ROOT_PASSWORD: root123` | Set DB password |

### ⚡ Commands
```bash
cd ~/Desktop
mkdir topic9
cd topic9
code .

# After creating all 4 files
docker-compose up

# Open browser → http://localhost:5000

# In NEW terminal window
docker ps                  # See both containers running!

# Stop everything
docker-compose down
```

---

## Topic 10 — Ansible Playbook

### 🧠 Theory
Ansible is an **open-source automation tool** for configuration management. Instead of manually going to 100 servers and typing commands, write one YAML playbook and Ansible runs it on all servers automatically. **Agentless** — uses SSH, no software needed on target machines.

### ⚡ Setup Commands
```bash
# Pull Ubuntu image
docker pull ubuntu

# Run Ubuntu in interactive mode
docker run -it ubuntu bash

# Inside container — update packages
apt update

# Install Ansible
apt install ansible -y

# Create project folder
mkdir ansible-demo
cd ansible-demo
```

### 📄 Create playbook.yml (inside container)
```bash
cat > playbook.yml << 'EOF'
---
- hosts: localhost
  connection: local
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started
EOF
```

### ▶️ Run the Playbook
```bash
ansible-playbook playbook.yml
```

### ✅ Expected Output
```
TASK [Gathering Facts] ******* ok: [localhost]
TASK [Update apt cache] ****** ok: [localhost]
TASK [Install nginx] ********* changed: [localhost]
TASK [Start nginx] *********** changed: [localhost]

PLAY RECAP
localhost : ok=4  changed=2  unreachable=0  failed=0
```

### 📝 Playbook Explained
| Line | Meaning |
|------|---------|
| `hosts: localhost` | Run on this same machine |
| `connection: local` | Don't use SSH, run locally |
| `become: yes` | Use sudo/root privileges |
| `apt: update_cache: yes` | Run apt update |
| `apt: name: nginx state: present` | Install nginx |
| `service: name: nginx state: started` | Start nginx |
| `ok:` in output | Task ran, no change needed |
| `changed:` in output | Task ran and made a change |
| `failed=0` | Everything worked! ✅ |

---

## ⚡ Quick Reference — All Commands

### Git Commands
```bash
git init                          # Initialize repo
git add .                         # Stage all files
git add filename.txt              # Stage one file
git commit -m "message"           # Save snapshot
git push                          # Upload to GitHub
git push -u origin main           # First time push
git pull origin main              # Download from GitHub
git branch feature                # Create branch
git checkout feature              # Switch branch
git checkout -b feature           # Create + switch branch
git merge feature                 # Merge branch
git log                           # View history
git status                        # Check changes
git remote add origin URL         # Link to GitHub
git remote -v                     # Check remote URL
git branch -d feature             # Delete branch
```

### Docker Commands
```bash
docker build -t myapp .           # Build image
docker run -p 5000:5000 myapp     # Run container
docker run -it ubuntu bash        # Interactive Linux
docker images                     # List images
docker ps                         # List running containers
docker ps -a                      # List ALL containers
docker stop <id>                  # Stop container
docker rmi myapp                  # Delete image
docker pull ubuntu                # Download image
docker rm -f $(docker ps -aq)     # Delete ALL containers
docker-compose up                 # Start all services
docker-compose up -d              # Start in background
docker-compose down               # Stop all services
docker-compose ps                 # List compose containers
```

### Ansible Commands
```bash
apt install ansible -y            # Install Ansible
ansible-playbook playbook.yml     # Run playbook
ansible --version                 # Check version
```

---

## 🎯 Viva Questions — All Topics

### Git (Topics 1-4)
| Question | Answer |
|----------|--------|
| What is Git? | Distributed version control system to track code changes |
| What does git init do? | Creates empty Git repository in the folder |
| Difference between git add and git commit? | add stages files (prepares for photo), commit saves permanently (clicks photo) |
| What is HEAD? | Pointer that points to your latest commit |
| What is a branch? | Independent copy of code to work without affecting main |
| What is merging? | Combining changes from one branch into another |
| What is a merge conflict? | When two branches edit same line differently and Git can't auto-merge |
| How to resolve conflict? | Open file, remove markers, keep correct line, git add, git commit |
| What is a Pull Request? | Request on GitHub to review and merge one branch into another |
| What is origin? | Default nickname for your GitHub repository URL |
| What does git push do? | Uploads local commits to remote GitHub repository |

### GitHub Actions (Topics 5-7)
| Question | Answer |
|----------|--------|
| What is GitHub Actions? | CI/CD automation tool built into GitHub |
| What is a workflow? | YAML file in .github/workflows/ defining automated jobs |
| What is CI? | Continuous Integration — auto build and test on every push |
| What is CD? | Continuous Deployment — auto deploy after successful build |
| What is on: push? | Trigger — runs workflow every time code is pushed |
| What is workflow_dispatch? | Allows manual run from GitHub website |
| What is runs-on: ubuntu-latest? | GitHub creates temporary Ubuntu machine to run the job |
| What is pytest? | Python testing framework that runs test functions automatically |
| What is assert? | Checks if condition is true — if false, test fails |

### Docker (Topics 8-9)
| Question | Answer |
|----------|--------|
| What is Docker? | Platform to run apps in isolated containers that work identically anywhere |
| What is a container? | Lightweight isolated environment with app + all dependencies |
| Difference between image and container? | Image is static template, container is running instance |
| What is Dockerfile? | Text file with step-by-step instructions to build Docker image |
| What does -p 5000:5000 mean? | Map laptop port 5000 to container port 5000 |
| Why use Docker? | Eliminates "works on my machine" problem |
| What is Docker Compose? | Tool to run multiple containers together |
| What does docker-compose up do? | Starts all containers defined in docker-compose.yml |
| What is a service in Compose? | Each container defined in docker-compose.yml |

### Ansible (Topic 10)
| Question | Answer |
|----------|--------|
| What is Ansible? | Open-source automation tool for configuration management |
| What is a playbook? | YAML file defining tasks to run on managed machines |
| What is agentless? | No software needed on target machines — uses SSH only |
| What does become: yes do? | Run all tasks with sudo/root privileges |
| What is idempotent? | Running playbook multiple times gives same result — safe to re-run |
| What does ok mean in output? | Task ran, no change was needed |
| What does changed mean in output? | Task ran and made a change on the system |
| What is the apt module? | Ansible module for managing packages on Ubuntu |

---

## ⚠️ Common Mistakes to Avoid

```bash
# WRONG                    CORRECT
git add.                   git add .          # Space before dot!
docker build -t myapp.     docker build -t myapp .   # Space before dot!
form flask import Flask    from flask import Flask    # from not form!
```

- ✅ YAML files — always **copy paste**, never type manually (spaces matter!)
- ✅ **Docker Desktop must be running** before any docker command
- ✅ **Dockerfile has NO extension** — just `Dockerfile`
- ✅ If push fails → `git pull origin main` first then push
- ✅ `>>` adds to file, `>` overwrites file

---

## 🌐 Internet Requirements

| Topic | Internet? |
|-------|-----------|
| 1. Git Init, Add, Commit | ❌ Local only |
| 2. Push to GitHub | ✅ Yes |
| 3. Branches + Merge | ❌ Local only |
| 4. Pull Request | ✅ Yes |
| 5. GitHub Actions | ✅ Yes |
| 6. CI/CD Pipeline | ✅ Yes |
| 7. Automated Testing | ✅ Yes |
| 8. Docker (first time) | ✅ Yes (downloads image) |
| 9. Docker Compose (first time) | ✅ Yes (downloads MySQL) |
| 10. Ansible | ✅ Yes (downloads Ubuntu) |

> 💡 After first download, Docker images are cached on laptop!

---

*Best of luck Avi! You've got this! 💪🔥*
