# How to Double Click to Run a Desktop Flask App

<br>

Flask apps are simple, powerful, transparent and can be run on a desktop. However, one area of friction is that the user needs to use the command line to install Python dependencies and launch the app. Non-programmers are not comfortable using terminal commands. This project shows how to solve that problem.

Here I show a step-by-step process to set up a Flask app so that the user only needs to double-click a file to install Python dependencies and run the app from the desktop. No command line needed. 

I followed this process when creating the myOfflineAi desktop app. The example templates that we will use here are from that app:<br>
https://github.com/vbookshelf/myOfflineAi-Privacy-First-Ai


<br>

### 1. Install UV

This workflow uses the UV python package manager and not pip. 

Here you will find the instructions to install uv on Mac and Windows:<br>
https://docs.astral.sh/uv/getting-started/installation

```
Open your terminal and copy paste this command:

Mac:
wget -qO- https://astral.sh/uv/install.sh | sh

Windows:
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

```

<br>

### 2. Follow these steps

```
The terminal commands shown below are for macOS.
There’s no need to type the % symbol.


1- Create a project folder

Create a project folder named: Example-Project-Folder.
Place Example-Project-Folder on your desktop.
cd into Example-Project-Folder.

% cd Desktop
% cd Example-Project-Folder

2- Create a pyproject.toml file
Add your flask app's project dependencies to a file named pyproject.toml
This is a sample template that you can edit:
https://github.com/vbookshelf/How-to-Double-Click-to-Run-a-Flask-App/blob/main/templates/pyproject.toml

The pyproject.toml file is the uv equivalent of the requirements.txt file.
UV does not use requirements.txt

Place the pyproject.toml file inside Example-Project-Folder.

This is what the pyproject.toml file looks like:

[project]
name = "myOfflineAi"
version = "1.0"
description = "A Flask UI for Ollama"
requires-python = "==3.10.*"
dependencies = [
	"flask==3.1.2",
	"ollama==0.6.0",
	"pymupdf==1.26.4",
	"pillow==11.3.0",
	"requests==2.32.5",
]

3- Make sure the file is named pyproject.toml and not pyproject.toml.txt
Use the terminal to change the file name if needed.

List all files
% ls

Change the file name if necessary
% mv pyproject.toml.txt pyproject.toml

4- Create the uv lock file
This terminal command will create a file named: uv.lock
% uv lock

5- For Mac users: Create a file named start-mac-app.command

Use this template as an example. Use ChatGPT to customize it for your flask app.
This example file checks that Ollama is installed. Your app may not need that check.
Template:
https://github.com/vbookshelf/How-to-Double-Click-to-Run-a-Flask-App/blob/main/templates/start-mac-app.command

6- Make the start-mac-app.command file executable
% chmod +x start-mac-app.command

Now when a user double clicks this file it will install all dependencies and start your app.

7- For Windows users: Create a file named start-windows-app.bat

Use this template as an example. Use ChatGPT to customize it for your flask app.
Again, this example file checks that Ollama is installed. Your app may not need that check.
Template:
https://github.com/vbookshelf/How-to-Double-Click-to-Run-a-Flask-App/blob/main/templates/start-windows-app.bat

8- Set up your Flask app to auto launch in the browser

Ensure that your app will auto launch in the browser i.e. the user won't need to copy the url from the terminal and paste in in the browser. This is the sample code that does that:

if __name__ == "__main__":
	
	# Auto launch in the browser
    import webbrowser, threading
    import os

    def open_browser():
        webbrowser.open("http://127.0.0.1:5000")

	# This check ensures the browser is opened only by the main process.
	# Prevents two browser tabs from being opened when in debug mode.
    if os.environ.get("WERKZEUG_RUN_MAIN") != "true":
        # Launch browser 1 second after server starts
        threading.Timer(1.0, open_browser).start()

    # Start Flask app
    app.run(host="127.0.0.1", port=5000, debug=False, threaded=True)

9- Place your app.py file and your static folder (if you have one) in  Example-Project-Folder

10- The full list of files and folders in  Example-Project-Folder should be:
app.py
static (if this folder is being used)
pyproject.toml
uv.lock
start-mac-app.command
start-windows-app.bat

11- Launch your app:

If you are using Mac double-click: start-mac-app.command
If you are using Windows double-click: start-windows-app.bat

Your app should open in your browser. You can check the terminal to see if there are any errors.

```
<br>

### Note 1

By default, uv sync only installs packages already locked in uv.lock. Normally, if you add something to pyproject.toml manually, you then need to run uv lock. However, the script in start-app.command automates this - uv lock runs every time. You don't have to manually run uv lock again — the script does it. The script always makes sure everything is up-to-date. When you add something to pyproject.toml, uv will ensure that all packages work together without any clashes.

This is where uv lock shines. When you add a new dependency, uv calculates the entire dependency graph. If two packages want different versions of the same library (e.g., requests==2.28 vs requests==2.32), uv tries to find a compatible version that satisfies both. If no compatible version exists, it will fail with a clear error (instead of installing something broken). This ensures that everything in your environment works together consistently.

### Note 2

On Windows:

.bat and .cmd files are recognized as executable by default.<br>
Double-clicking them will immediately launch Command Prompt and execute the script.<br>
No permission changes needed.

On macOS:

.command files need chmod +x to be executable.<br>
Without it, double-clicking will open them in a text editor instead of executing the code.

<br>
