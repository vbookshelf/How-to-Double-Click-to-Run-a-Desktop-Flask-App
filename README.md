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

### Note 3

Say you created an app and uploaded it to GitHub. Your intention is for your users to download the app, and then double-click a start-app.command file to run the app. macOS will block this. When you download an .app (or .command, .sh, .pkg, or other executable) file from GitHub (or anywhere on the internet), macOS marks it with a “quarantine” flag for security. This is part of macOS’s Gatekeeper system, which is designed to protect users from running potentially unsafe or unverified code. If the user tries to double click a file to run it, macOS will block it. On the myOfflineAi project I got around this problem by modifying the setup instructions so that the user runs a terminal command that creates a copy of the start-mac-app.command file and then makes it executeable. I also included instructions to get around this problem in Windows.

This is an excerpt from the setup instructions. See macOS point 7.

```

4. Initial Setup
--------------------------------------------------------------

[ macOS ]

1. Open Terminal (Command+Space, type "Terminal")
2. Paste this command into the terminal to install uv:
wget -qO- https://astral.sh/uv/install.sh | sh
3. Wait for uv installation to finish
4. Type 'cd ' in the terminal (with a space after cd)
5. Drag the myOfflineAi-v1.1 folder into the Terminal window
6. Press Enter
7. Paste this command into the terminal:
cat start-mac-app.command > temp && mv temp start-mac-app.command && chmod +x start-mac-app.command
8. Press Enter
9. Open the myOfflineAi-v1.1 folder
10. Double-click: start-mac-app.command


[ Windows ]

1. Press the Windows key on your keyboard
2. Type cmd and press Enter (a black window will open)
3. Copy this entire command:
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
4. Right-click in the black window to paste
5. Press Enter
6. Wait for "uv installed successfully" or similar message
7. Close the window and open a new one for the changes to take effect
8. Navigate to the myOfflineAi-v1.1 folder thats on your desktop
9. Double-click: start-windows-app.bat

If Windows shows a security warning:
1. Right-click on start-windows-app.bat 
2. Select "Properties"
3. Check the "Unblock" box at the bottom
4. Click "OK"
5. Now double-click start-windows-app.bat to run


```

<br>
