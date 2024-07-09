<br>![image1](images/image1.png)<br>
## Configuring MERN Stack on Your Local Machine Using GitHub and Daytona

### Introduction
Managing infrastructure to ensure smooth project delivery often involves overcoming numerous obstacles, particularly during the development environment setup. Frequent setup issues can hinder developers' ability to maintain consistent, repeatable, and easily deployable workspaces across various platforms. This guide examines how Daytona simplifies Development Environment Management (DEM), offering developers reliable workspaces that avoid common complications. It contrasts how to replicate a simple MERN stack repository locally from GitHub with and without Daytona.

### Technologies used
`Daytona` and `Docker` are employed for automating development environment management. Node.js serves as the runtime environment for executing JavaScript code on the server side.

#### Backend Libraries and Frameworks
For the backend, `Express` provides a web framework for creating robust APIs, and `MongoDB` serves as a NoSQL database for efficient data storage and retrieval. `Mongoose` simplifies data modeling and validation for MongoDB, while `CORS` middleware enables Cross-Origin Resource Sharing for the backend API. The `Dotenv` module is used to load environment variables from a .env file into process.env, and Nodemon automatically restarts the Node.js application when file changes are detected.

#### Frontend Libraries and Frameworks
On the frontend, `React` is used for building user interfaces, and `Recoil` manages the state of React applications. `Axios` is a promise-based HTTP client for making API requests, and `Vite` provides a fast development server and optimized builds. `TailwindCSS`, a utility-first CSS framework, is used for quickly designing responsive and modern layouts.

### Setting up the project without Daytona

In this section, we will set up this repository locally without Daytona.

#### Step 1: Clone the Git Repository Locally
- Open your terminal or command prompt and clone the repository locally using the following command. Alternatively, you can download the zip file and extract its contents.
```bash
git clone <repository_url>
```
#### Step 2: Install Node.js
- To check if Node.js is installed, open a terminal or command prompt and run:
```bash
node --version
```
- If Node.js is not installed on your machine, download it from [Node.js official website](https://nodejs.org/) and follow the installation instructions specific to your operating system.

- Adding Node.js to the system path

  **Windows:** Open the Settings and search for `Environment Variables`. Click on the `Environment Variables` button and select the `Path` variable. Click on the "Edit" button and add an entry: `.\node_modules\.bin`.

  **Linux / MacOS:** execute the following command:
```bash
export PATH=$PATH:/usr/local/nodejs/bin
```
#### Step 3: How to Start the Backend Server
- Open your terminal or command prompt. Use the `cd` command to navigate to the `backend` directory within the cloned repository and run the following command to install all necessary dependencies:
```bash
npm install
```
- Create a `.env` file in the `backend` directory and add the following line in it. Change "Your Database Connection String" with real MongoDB connection uri.
```
MONGO_URI="Your Database Connection String"
```
- Start the backend server by running:
```bash
npm run dev
```

#### Step 4: How to Start the Frontend
- Open your terminal or command prompt. Use the `cd` command to navigate to the `frontend` directory within the cloned repository and run the following command to install all necessary dependencies:
```bash
npm install
```
- Create a `.env` file in the `frontend` directory and add the following line:
```
VITE_BACKEND_URL="http://localhost:8000/api/customers/"
```
- Start the frontend development server by running:
```bash
npm run dev
```
  Your React app should now be up and running.

### Problems with Conventional Setup

- **Dependency Management**: Traditional setups often face issues where different projects require conflicting versions of dependencies or libraries. Resolving these conflicts manually can be time-consuming and error-prone.<br><br>
- **Time Consumption**: Setting up development environments manually can consume valuable time that developers could otherwise spend on actual coding and development tasks.<br><br>
- **Collaboration Difficulties**: Maintaining consistency across team members' development environments can be challenging. Differences in operating systems, configurations, or installed software versions can lead to compatibility issues.

### How Daytona Helps

Daytona automates the entire process, addressing these issues effectively:

- **Automated Provisioning**: Daytona automates the provisioning and configuration of development environments. This includes setting up necessary dependencies, libraries, and tools based on project requirements, reducing the likelihood of conflicts and mismatches.<br><br>
- **Seamless IDE Integration**: Daytona seamlessly integrates with popular integrated development environments (IDEs) such as Visual Studio Code and IntelliJ IDEA. Developers can securely connect to these IDEs and work within their familiar coding environments.<br><br>
- **Easy Sharing and Collaboration with Cross-Platform Support**: Daytona enhances collaboration through robust cross-platform support, ensuring seamless sharing across team members and stakeholders. It accommodates diverse operating systems like Linux, macOS, and Windows, supporting both x86-64 and AArch64 architectures. 
### Setting up the project using Daytona

#### Step 1: Install Daytona
Daytona’s functionality is exposed through a command-line tool that runs on Linux, macOS, and Windows systems, on both x86-64 and AArch64 architectures.
  Pre-requisites: Ensure Docker is installed and running.

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
Alternatively, you can use Homebrew or Nix for installation. Refer to the official installation guide for more options.
After executing the official installation script for your respective operating system, Daytona will get installed locally on your machine and daytona server will start running.<br>
<br>![image1](images/image2.png)<br>

#### Step 2: Configure GitHub as a Git Provider

- Add a New Git Provider using the following command and select GitHub from the list. Enter GitHub Personal Access Token. Generate a token if you do not have one, by following [this guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) and enter it when prompted.<br>
```sh
daytona git-providers add
```
#### Step 3: Install a Provider
Provider is a plugin that interfaces with the Daytona Server, responsible for Workspace deployment and lifecycle management. After installation, Daytona creates a default Provider for Docker `dytona-provider-docker`. 
- To install a provider, run the following command and select the Default Docker Provider.<br>
```sh
daytona provider install
```

#### Step 4: Set a Target
Target is a set of configuration that governs how Daytona manages and deploys Workspaces. Each Target is tied to an individual Provider. When installing Daytona, A default Target using the Docker Provider is also added, which uses your local installation of Docker to deploy Workspaces. 
- To set a New Target, execute yhe following command:<br>
```sh
daytona target set
```
- Select the Docker provider and set the default local Docker target. Specify the sock path as `/var/run/docker.sock`. Verify the Target by executing the following command:<br>
```sh
daytona target list
```
<br>![image1](images/image7.png)<br>
#### Step 5: Create a Workspace
- initiate the process by executing the following command. Opt for GitHub and select a repository, or alternatively, create a Workspace using any Git URL by specifying your remote repository's URL.<br>
```sh
daytona create
```
<br>![image1](images/image10.png)<br>
#### Step 6: Set a Preferred IDE and Open the Workspace

- To set up your preferred IDE, list all supported IDEs using the following command and select from them. For this guide we will use Visual Studio Code.<br>
```sh
daytona ide
```
- Once you've selected your preferred IDE, open the Workspace by executing the following command. This will list all existing workspaces. Select the desired workspace, and it will open in your chosen IDE.<br>
```sh
daytona code
```
#### Step 7: Rebuild and Reopen in container
**Pre-requisite:** You must have Microsoft's Dev Containers extension preinstalled in Visual Studio Code.<br>
- press `ctrl + shift + p` and choose "Dev Containers: Rebuild and Reopen in Container".<br>
<br>![image1](images/image11.png)<br>

#### Step 8: Start the Development Servers

**Backend**
- Navigate to the backend directory and create a `.env` file in the `backend` directory and add the following line:
```
MONGO_URI="Your Database Connection String"
```
- Execute the following command to start the backend server:
```sh
npm run dev
```
**Frontend**
- Navigate to the frontend directory and create a `.env` file in the `frontend` directory and add the following line:
```
VITE_BACKEND_URL="http://localhost:8000/api/customers/"
```
- Execute the following command to start the frontend server:
```sh
npm run dev
```

### How Daytona Simplifies Development Environments

Daytona streamlines the process of setting up development environments by leveraging a `.devcontainer` folder. This folder contains configuration files that ensure all necessary dependencies and plugins are installed and configured correctly, providing a consistent and hassle-free setup.

#### The `.devcontainer` Folder and `devcontainer.json`

The `.devcontainer` folder includes a `devcontainer.json` file that specifies the configuration for the development environment. This file covers the base image, workspace folder settings, port forwarding, and additional customizations.

#### `devcontainer.json` Configuration

Here are the details of the `devcontainer.json` file used in this project:

```json
{
    "name": "Node.js, Express, React, MongoDB & Tailwind",
    "image": "mcr.microsoft.com/vscode/devcontainers/javascript-node:20",
    "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",
    "portsAttributes": {
        "5174": {
            "label": "Frontend",
            "onAutoForward": "openPreview"
        },
        "8000": {
            "label": "Backend",
            "onAutoForward": "openPreview"
        },
        "27017": {
            "label": "MongoDB",
            "onAutoForward": "ignore"
        }
    },
    "customizations": {
        "vscode": {
            "extensions": [
                "mongodb.mongodb-vscode",
                "dbaeumer.vscode-eslint",
                "esbenp.prettier-vscode",
                "bradlc.vscode-tailwindcss",
                "davidanson.vscode-markdownlint"
            ]
        }
    },
    "postCreateCommand": "echo 'package-import-method=clone-or-copy' >> ~/.npmrc && npm install -g npm@10.8.1 nodemon && cd backend && npm install --unsafe-perm && cd ../frontend && npm install --unsafe-perm",
    "remoteUser": "root"
}
```

#### Key Features

##### Base Image
- **Base Image:** Uses `mcr.microsoft.com/vscode/devcontainers/javascript-node:20` for a consistent environment. This base image provides a pre-configured Node.js environment, ensuring that all necessary dependencies and tools for Node.js development are available right out of the box. It simplifies the setup process by including common development utilities and ensuring compatibility with the specified Node.js version.

##### Workspace Folder
- **Workspace Folder:** Maps the local workspace folder to the container, ensuring that the development environment is consistent across different machines and setups.

##### Port Forwarding
- **Port Forwarding:** Configures ports for frontend, backend, and MongoDB services with specific labels and behaviors, making it easier to manage and access these services during development.

##### Customizations
- **Customizations:** Installs essential Visual Studio Code extensions automatically:
  - MongoDB, ESLint, Prettier, Tailwind CSS, and Markdown linting.

##### Post Create Command
- **Post Create Command:** Runs commands after container creation to set npm configurations and install dependencies for both backend and frontend, ensuring that the development environment is ready to use immediately.

##### Remote User
- **Remote User:** Sets the user to `root` for necessary permissions, which can be adjusted as needed.

By leveraging the `.devcontainer` folder and `devcontainer.json`, developers can enjoy a consistent, automated, and fully-featured development environment, allowing them to focus on coding.

### Conclusion
By following this guide, you have learned to set up a MERN stack project both with and without Daytona. The conventional method often faces challenges such as dependency management, time consumption, and collaboration difficulties. Daytona simplifies this process through automated provisioning, seamless IDE integration, and robust cross-platform support. By automating environment setup, Daytona ensures consistency, streamlines workflows, and improves collaboration. Embracing Daytona for development environment management reduces setup time, minimizes errors, and enhances productivity.