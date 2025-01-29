# Setting up a dev container for Go

* Primary author: [Anton Sun](https://github.com/antonsun6)
* Reviewer: [Jerry Wen](https://github.com/jerrywen2005)


Welcome! In this tutorial, you'll learn how to set up a development container for Go. We'll walk you through the process of creating a Go dev container in Visual Studio Code, ensuring a seamless and consistent development environment.


## **Prerequisites**

Before we dive in, make sure you have:

1. **A GitHub account:** If you don’t have one yet, sign up at [GitHub](github.com).
2. **Git installed:** [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.
3. **Visual Studio Code (VS Code):** Download and install it from [here](https://code.visualstudio.com/).
4. **Docker installed:** Required to run the dev container. [Get Docker here](https://www.docker.com/products/docker-desktop/).

## **Part 1: Setup**

### Step 1: Create a Git Repository

1. Open your terminal or command prompt to create a new directory.
2. Create a new directory for your Go project by running:
```bash
mkdir go-dev-container
cd go-dev-container 
```
3. Set up a new Git repository
    ```bash 
    git init
    ```
4. Create a README.md file that will serve as an initial commit:
```bash
echo "# Dev Container for Go" > README.md
git add README.md
git commit -m "Initial commit with README"
```
Now that the Git repository is set up, let’s connect it to a remote hosting platform. In this tutorial, we’ll be using GitHub.
### Step 2: Create a Remote Repository on GitHub
1. Log in to your GitHub account and go to the [Create a New Repository](https://github.com/new) section.
2. Name this new repository ```go-dev-container```
3. Add whatever description you want and have the visibility set to public.
4. Make sure to **not** to initialize the repository with a README, .gitignore, or license.
5. Lastly, click **Create Repository** 
### Step 3: Link your Local Repository to GitHub
1. Add the GitHub repository as a remote:
```bash
git remote add origin https://github.com/<your-username>/go-dev-container.git
```
Replace ```<your-username>``` with your GitHub username.
2. Check the name of your default branch using the command ```git branch```. If it’s not named ```main```, you can rename it to ```main``` with the following command: ```git branch -M main```.
3. Push your local commits to the GitHub repository:
```bash
git push --set-upstream origin main
```

    !!! note "Usage of the --set-upstream flag"

        git push --set-upstream origin main: This command pushes the main branch to the remote repository origin. The --set-upstream flag sets up the main branch to track the remote branch, meaning future pushes and pulls can be done without specifying the branch name and just writing git push origin when working on your local main branch. This long flag has a corresponding -u short flag.

4. Return to your web browser and refresh your GitHub repository. You should now see that the commit made locally has been pushed to the remote repository. To confirm, run ```git log``` locally to view the commit ID and message—they should match the latest commit on GitHub. This verifies that your changes were successfully pushed to the remote repository.

## **Part 2: Setting up the Development Environment**


### Step 1: Add Development Container Configuration
1. Open the ```go-dev-container``` directory in VS Code. Navigate to File > Open Folder and select the ```go-dev-container``` directory.
2. Install the Dev Containers extension in VS Code: Open the Extensions view (```Ctrl+Shift+X``` or ```Cmd+Shift+X``` on macOS), search for "Dev Containers," and click Install.
3. Create a ```.devcontainer``` directory at the root of your project and then set up the following configuration file inside this directory: 
```bash
.devcontainer/devcontainer.json
```

The ```devcontainer.json``` file defines the configuration for your development environment. Here, we're specifying the following:

* ```name```: A descriptive name for your dev container.
* ```image```: The Docker image to use, in this case, the latest version of a Go environment.
* ```customizations```: Adds useful configurations to VS Code, like installing the Go extension. When you search for VSCode extensions on the marketplace, you will find the string identifier of each extension in its sidebar. Adding extensions here ensures other developers on your project have them installed in their dev containers automatically.
* ```postCreateCommand```: This defines a command that runs after the development container is created. In this case, it initializes a new Go module and manages dependencies by executing ```go mod init``` followed by ```go mod tidy```.
```json
{
    "name": "Go Dev Container",
    "image": "mcr.microsoft.com/vscode/devcontainers/go:latest",
    "customizations": {
    "vscode": {
        "settings": {},
        "extensions": ["golang.go"]
        }
    },
    "postCreateCommand": "go mod init go-dev-container && go mod tidy"
}
```

!!! note "Golang VS Code Extension"
    The Go extension for VS Code, developed by Google, enhances Go development with essential features such as syntax highlighting, intelligent code completion, integrated debugging, linting, automatic formatting, and built-in test support. It also simplifies Go module management and provides real-time error detection for a seamless coding experience.
/go-dev-container

### Step 2: Reopen the Project in a VSCode Dev Container
Reopen the project in the container by pressing ```Ctrl+Shift+P``` (or ```Cmd+Shift+P``` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab, open a new terminal pane within VSCode, and try running ```go version``` to check which version of Go was installed. You should see a recent version of Go was installed and that your dev container was properly created.

## **Part 3: Writing and Running a Go Program**

In the previous section, we used the ```postCreateCommand``` in the ```devcontainer.json``` file to initialize a new Go module for running a program. If this step was not included in your ```go-dev-container```, you can manually run the following command:
```bash
go mod init go-dev-container
```

### Step 1: Make a New Go File
Make a new file in your ```go-dev-container``` directory and name it ```main.go```.
### Step 2: Write your First Go Program
In the ```main.go``` file, write the following code to create a basic program that prints "Hello COMP423."
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello COMP423")
}
```
### Step 3: Run your Newly Written Program
Open a new terminal in VSCode and run the following command:
```bash
go run main.go
```
Alternatively, you could run the following series of commands that will do the same:
```bash
go build
./go-dev-container
```
After running those commands, you should see your terminal output "Hello COMP423".

!!! abstract "Understanding Different Methods to Execute a Go Program"
    The `go run` command compiles the code into an executable and immediately runs it, making it convenient for quick testing. Conversely, the `go build` command functions similarly to `gcc` in C, generating an executable file that can be manually executed using `./<executable_name>`. This approach is useful when you want to distribute or repeatedly run a compiled binary.


When you run the ```go run``` command, Go first compiles your code into an executable and then immediately executes it. In contrast, the ```go build``` command functions similarly to how ```gcc``` works in C. For example, when you execute ```gcc main.c```, the compiler produces an executable file named after the directory. This file can then be run directly using ```./<directory_name>```.

## **Part 4: Push your Changes to a Remote Repository**
Now that we've updated our project, it's essential to commit and push these changes to the remote repository created earlier in the tutorial to keep it up to date.

Execute the following commands in your terminal to transfer the latest changes to the remote repository:
```bash
git add .
git commit -m "Create Hello COMP423 Program in Go"
git push
```

**Works Cited**: This tutorial draws inspiration from Kris Jordan's [Starting a Static Website Project with MkDocs](https://comp423-25s.github.io/resources/MkDocs/tutorial/), particularly the sections on Prerequisites, Part 1, and Part 2.