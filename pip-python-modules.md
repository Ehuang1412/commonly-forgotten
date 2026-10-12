Fixing "No module named 'flask'"

This error almost always means one of three things. Let's diagnose it systematically.

Step 1: Check Which Python Is Running

Run these three commands from inside your catalog-vapt/ folder and note the output:

bash
# Which Python is being used?
which python        # macOS/Linux
where python        # Windows

# Which pip is being used?
which pip           # macOS/Linux
where pip           # Windows

# Is Flask installed anywhere?
python -m pip show flask
What to look for: The paths for python and pip should both point inside your venv/ folder. If they point to a system Python (like /usr/bin/python or C:\Python313\python.exe), your virtual environment is not active.

Step 2: Check If the venv Is Actually Activated

Your terminal prompt should show (venv) at the start, like this:

text
(venv) user@machine:~/catalog-vapt$
If you don't see (venv), activate it:

bash
# macOS/Linux:
source venv/bin/activate

# Windows (Command Prompt):
venv\Scripts\activate

# Windows (PowerShell):
venv\Scripts\Activate.ps1
Windows PowerShell tip: If you get a "scripts disabled" error, run this once in an admin PowerShell:

powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Then try activating again.

Step 3: Install Flask (Inside the Activated venv)

Once (venv) appears in your prompt:

bash
pip install flask
Then verify:

bash
python -m pip show flask
You should see version info and a Location: path that includes venv.

Step 4: The Most Common Gotcha — Running the App Wrong

There are two ways to run Python, and only one uses your venv:

Command	Uses venv?	Result
python app.py	✅ Yes (if venv is active)	Works
python3 app.py	❌ Often uses system Python	No module named 'flask'
Double-clicking app.py in a file explorer	❌ Uses system Python	No module named 'flask'
Running from an IDE that isn't configured for the venv	❌ Uses IDE's interpreter	No module named 'flask'
Make sure you run python app.py (not python3 app.py) with (venv) visible in your prompt.

Step 5: If You're Using an IDE (VS Code / PyCharm)

Your editor might be using a different Python interpreter than your terminal.

VS Code:

Press Ctrl+Shift+P (or Cmd+Shift+P on Mac).
Type "Python: Select Interpreter".
Choose the one whose path includes catalog-vapt/venv/ (e.g., ./venv/bin/python or .\venv\Scripts\python.exe).
Open a new integrated terminal — the old one won't pick up the change.
PyCharm:

Go to Settings → Project → Python Interpreter.
Click the gear icon → Add Interpreter → Add Local Interpreter.
Choose Existing and browse to catalog-vapt/venv/bin/python (or venv\Scripts\python.exe on Windows).
Apply, then re-run.
Step 6: Nuclear Option — Rebuild the venv

If none of the above works, delete and rebuild:

bash
# Deactivate if active
deactivate

# Delete the venv
# macOS/Linux:
rm -rf venv
# Windows:
rmdir /s /q venv

# Recreate
python -m venv venv

# Activate (see Step 2)

# Upgrade pip and install Flask
python -m pip install --upgrade pip
pip install flask

# Verify
python -m pip show flask
Quick Diagnostic Checklist

Run these in order and share the output if you're still stuck:

bash
# 1. Are you in the right folder?
pwd              # macOS/Linux
cd               # Windows

# 2. Is venv active? (look for "(venv)" in prompt)
# If not:
source venv/bin/activate          # macOS/Linux
venv\Scripts\activate             # Windows

# 3. Where is Python?
which python     # macOS/Linux
where python     # Windows

# 4. Is Flask installed?
python -m pip show flask

# 5. Run the app
python app.py
If you paste the output of these five commands, I can pinpoint exactly what's wrong. The most likely culprit based on your error is that the venv isn't activated or you're using python3 instead of python — those two account for about 90% of this error.
