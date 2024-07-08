<br>![image1](images/image1.png)<br>

## Configuring MERN Stack on Your Local Machine Using GitHub and Daytona

### Introduction
Managing infrastructure to ensure smooth project delivery often involves overcoming several obstacles, particularly during the development environment setup. Frequent setup issues can disrupt developers' ability to maintain consistent, repeatable, and easily deployable workspaces across various platforms. This blog explores how Daytona streamlines Development Environment Management (DEM), providing developers with reliable workspaces free from common complications. We demonstrate how to replicate a fundamental MERN stack repository from GitHub locally using Daytona.

### Technologis used

#### Backend
- **Express**: A minimalist web framework for Node.js to create robust APIs.
- **MongoDB**: A NoSQL database for storing and retrieving data efficiently.
- **Mongoose**: An ODM library for MongoDB, simplifying data modeling and validation.
- **Cors**: Middleware to enable Cross-Origin Resource Sharing for the backend API.
- **Dotenv**: Module to load environment variables from a `.env` file into `process.env`.
- **Nodemon**: A tool that automatically restarts the Node.js application when file changes are detected.

#### Frontend
- **React**: A JavaScript library for building user interfaces.
- **Recoil**: A state management library for React applications.
- **Axios**: A promise-based HTTP client for making API requests.
- **Vite**: A build tool that provides fast development server and optimized builds.
- **TailwindCSS**: A utility-first CSS framework for quickly designing responsive and modern layouts.

### Setting up the repository without Daytona

In this section, we will set up this repository locally without Daytona, After you have successfully cloned the repository locally.

#### Prerequisites
Make sure you have Node.js installed on your machine. You can download it from [Node.js official website](https://nodejs.org/).

#### Step 1: How to Start the Backend Server
- Navigate to the `backend` directory and run:
```bash
npm install
```
- Create a `.env` file in the `backend` directory and write in it:
```
MONGO_URI="Your Database Connection String"
```
Replace `Your Database Connection String` with your actual database URI.

- In the `backend` directory, run:
```bash
npm run dev
```
#### Step 2: How to Start the Frontend
- Navigate to the `frontend` directory and run:
```bash
npm install
```
- Create a `.env` file in the `frontend` directory and write in it:
```
VITE_BACKEND_URL="http://localhost:8000/api/customers/"
```
- In the `frontend` directory, run:
```bash
npm run dev
```
Your React app should be up and running.

### Problems with Conventional Setup

- **Dependency Management**: Conflicts, version mismatches, and inconsistencies.
- **Environment Configuration**: Error-prone and time-consuming setup of environment variables.
- **Time Consumption**: Diverts focus from coding to setup tasks.
- **Collaboration Difficulties**: Ensuring consistent environments across team members.
- **Security Risks**: Vulnerabilities in manually configuring secure access.

### How Daytona Helps

Daytona automates the entire process, addressing these issues effectively:

- **Automated Provisioning**: Automatically provisions and configures environments.
- **Prebuilds and Secure Connections**: Sets up prebuilds and secure VPN connections.
- **Seamless IDE Integration**: Connects securely to local or Web IDEs like Visual Studio Code.
- **Easy Sharing and Collaboration**: Assigns a fully qualified domain name for easy sharing.
- **Reproducible Environments**: Uses OCI containers and supports Dev Container standards, ensuring consistency.
- **Cross-Platform Support**: Works on Linux, macOS, and Windows, supporting x86-64 and AArch64 architectures.

### Setting up the repository without Daytona
#### Step 1: Install Daytona
- Ensure Docker is installed and running, then execute:
##### For Windows
- Run the following script in PowerShell:
```powershell
$architecture = if ($env:PROCESSOR_ARCHITECTURE -eq "AMD64") { "amd64" } else { "arm64" }
md -Force "$Env:APPDATA\bin\daytona"; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]'Tls,Tls11,Tls12';
Invoke-WebRequest -URI "https://download.daytona.io/daytona/v0.21/daytona-windows-$architecture.exe" -OutFile "$Env:APPDATA\bin\daytona\daytona.exe";
$env:Path += ";" + $Env:APPDATA + "\bin\daytona"; [Environment]::SetEnvironmentVariable("Path", $env:Path, [System.EnvironmentVariableTarget]::User);
```

##### For Linux / macOS
- Run the following script:
```sh
(curl -sf -L https://download.daytona.io/daytona/install.sh | sudo bash) && daytona server -y && daytona
```
- Alternatively, you can use Homebrew or Nix for installation. Refer to the official installation guide for more options.

#### Step 2: Configure GitHub as a Git Provider

- Add a New Git Provider:
```sh
daytona git-providers add
```
- Select GitHub from the list.

- Enter GitHub Personal Access Token
Generate a token, if you do not have one following [this guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) and enter it when prompted.

#### Step 3: Install a Provider
- Run the following command.
```sh
daytona provider install
```
- Select the Default Docker Provider.
- Check if the provider is installed:
```sh
daytona provider list
```

#### Step 4: Set a Target

- Set a New Target:
```sh
daytona target set
```
- Select the Docker provider and set the default local Docker target. Specify the sock path as `/var/run/docker.sock`.

- Verify the Target by executing th following command:

```sh
daytona target list
```

#### Step 5: Create a Workspace

##### From GitHub
- Execute the following command:
```sh
daytona create
```
- Select GitHub and choose a repository. Name your workspace.

##### From a Git URL
- Execute the following command:
```sh
daytona create <Git URL>
```

#### Step 5: Set a Preferred IDE
- Execute the following command:
```sh
daytona ide
```
- Choose your preferred IDE, for now choose Visual Studio Code.

#### Step 6: Open the Workspace

- Choose a Workspace:
```sh
daytona code
```
- Rebuild the Container:
**Pre-requisite:** You must have Microsoft's Devcontainer extension preinstalled in Visual Studio Code
In Coomand pallet of Visual Studio Code, type "Rebuild and Reopen in Container".

- Start the Development Servers:

**Backend**
- Navigate to the backend directory and run:
```sh
npm run dev
```
**Frontend**
- Navigate to the frontend directory and run:
```sh
npm run dev
```

With Daytona, the setup process is simplified and automated, ensuring consistent and secure development environments, enabling you to focus on coding.