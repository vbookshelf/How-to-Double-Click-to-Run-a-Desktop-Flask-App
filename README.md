# How to Double Click to Run a Flask App

A step-by-step process to set up a Flask app so that the user only needs to double-click a file to run the app. No command line needed. The steps below cover both Mac and Windows.

This process uses the UV python package manager and not pip. Start by installing uv.

Here you will find the instructions to install uv on Mac and Windows:
https://docs.astral.sh/uv/getting-started/installation

```
Open your terminal and copy paste this command:

Mac:
wget -qO- https://astral.sh/uv/install.sh | sh

Windows:
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

```

<br>

```
1- Create a project folder<br>

Create a project folder named: Example-Project-Folder.
Place Example-Project-Folder on your desktop.
cd into Place Example-Project-Folder.

% cd Desktop
% cd Example-Project-Folder

2- Create a pyproject.toml file and place it inside Example-Project-Folder<br>
Add your flask app's project dependencies to a file named pyproject.toml<br>
This is a sample template that you can edit: xxxxxxxx

The pyproject.toml file is the uv equivalent of the requirements.txt file. UV does not use requirements.txt

3- Make sure the file is named pyproject.toml and not pyproject.toml.txt<br>
Use the terminal to change the file name if needed.

List all files
% ls
Change the file name if necessary
% mv pyproject.toml.txt pyproject.toml

4- Create the uv lock file
This terminal command will create a file named: uv.lock
% uv lock

5- For Mac users: Create a file named start-app.command
Use this template as an example. Use ChatGPT to customize it for your flask app e.g. This file checks that Ollama is installed. Your app may not need that check.
Template: xxxxxx

6- Make the start-app.command executable
% chmod +x start-app.command

Now when a user double clicks this file it will install all dependencies and start your app.

7- For Windows users: Create a file named start-app.bat
Use this template as an example. Use ChatGPT to customize it for your flask app e.g. This file checks that Ollama is installed. Your app may not need that check.
Template: xxxxxx

8- Ensure that your app will auto start in the bowser i.e. the user won't need to copy the url from the terminal and paste in in the browser. This is the sample code that does that:

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
start-app.command
start-app.bat

11- Launch your app:
If you are using Mac double-click: start-app.command
If you are using Windows double-click: start-app.bat

Your app should open in your browser

```
