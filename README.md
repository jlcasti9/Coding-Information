# Useful Linux Commands and Tips (For when I forget)

## How to Transfer Files from Windows to Linux using SCP (Secure Copy Protocol)

1. Open a terminal on your **Linux** machine.
2. Copy a file from Windows to Linux:

    ```bash
    scp /path/to/local/file username@linux_host:/path/to/remote/directory
    ```

    - `/path/to/local/file` – path to the file on Windows
    - `username` – your Linux user
    - `linux_host` – IP or hostname of the Linux machine
    - `/path/to/remote/directory` – destination folder on Linux

## How to SSH into a Linux machine from Windows

1. Generate SSH key pair on Windows with OpenSSH (if you haven't already):

    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
    ```

    For backwards compatibility, you can use:

    ```bash
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```

2. Copy the public key to the Linux machine:

    ```bash
    ssh-copy-id username@linux_host
    ```

    _This will copy the public key to the Linux machine and add it to the `~/.ssh/authorized_keys` file._  
    _You have to use Git Bash or WSL. Otherwise you have to manually add the public key to the `~/.ssh/authorized_keys` file on the Linux machine.._

3. Now you can SSH into the Linux machine without entering a password:

    ```bash
    ssh username@linux_host
    ```

    - `username` – your Linux user
    - `linux_host` – IP or hostname of the Linux machine

_You can type `exit` to log out of the SSH session._

## Easier SSH Access Setup

1. Open the config file in your `~/.ssh` directory. If the file does not exist, simply create it:

    ```bash
    %USERPROFILE%\.ssh\config
    ```

2. Add the following content to the file:

    ```
    # Default settings for all hosts
    Host *
        ServerAliveInterval 60
        ServerAliveCountMax 3
        TCPKeepAlive yes
        AddKeysToAgent yes

    # My Linux Host
    Host insert_alias_here
        HostName linux_host
        User username
        Port 22
        IdentityFile ~/.ssh/id_ed25519
    ```

    - `insert_alias_here` – a nickname for your Linux host (e.g., `myserver`)
    - `linux_host` – IP or hostname of the Linux machine (e.g., `Username`)
    - `username` – your Linux user
    - `Port` – the SSH port (default is 22)
    - `IdentityFile` – the path to your private SSH key
    - `ServerAliveInterval` – how often to send a keep-alive message (in seconds)
    - `ServerAliveCountMax` – how many keep-alive messages to send before disconnecting
    - `TCPKeepAlive` – whether to use TCP keep-alive messages
    - `AddKeysToAgent` – whether to add the private key to the SSH agent

    _The alias defined in config only applies to SSH tools (ssh, scp, and sftp). It does not create a system wide hostname._

3. Now you can SSH into your Linux machine using the alias you set up:

    ```bash
    ssh insert_alias_here
    ```

## Creating a Python Virtual Environment (venv) in Linux

1. Open a terminal.
2. Install the venv package (if needed):

    ```bash
    sudo apt install python3-venv
    ```

3. Navigate to your project directory.
4. Create the virtual environment:

    ```bash
    python3 -m venv myenv
    ```

    _Change `myenv` to whatever name you like._

5. Activate it:

    ```bash
    source myenv/bin/activate
    ```

## How to create a requirements.txt file in Linux

1. Open a terminal.
2. Navigate to your project directory.
3. Activate your virtual environment if you have one:

    ```bash
    source myenv/bin/activate
    ```

    _Replace `myenv` with your virtual environment name._

4. Run the following command to create a requirements.txt file:

    ```bash
    pip freeze > requirements.txt
    ```

    _This will list all installed packages and their versions in the current environment._

## How to install packages from requirements.txt in Linux

1. Open a terminal.
2. Navigate to your project directory.
3. Activate your virtual environment if you have one:

    ```bash
    source myenv/bin/activate
    ```

    _Replace `myenv` with your virtual environment name._

4. Ensure you have a `requirements.txt` file in your project directory.

    _If you don't have one, you can create it using the previous section's instructions._

5. Run the following command:

    ```bash
    pip install -r requirements.txt
    ```

## How to run a Python Script in Linux

1. Open a terminal.

    _Ensure you have activated your virtual environment if you are using one._

2. Navigate to the directory containing your script.
3. Run the script using Python 3:

    ```bash
    python3 script_name.py
    ```

    _Replace `script_name.py` with the name of your Python script._

## How to run a Python Script and keep it running in the background

1. Open a terminal.

    _Ensure you have activated your virtual environment if you are using one._

2. Navigate to the directory containing your script.
3. Run the script in the background:

    ```bash
    nohup python3 script_name.py &
    ```

    _Replace `script_name.py` with your script's name._

## How to see the output of a Python script running in the background

1. Open a terminal.
2. Navigate to the directory where you ran the script.
3. Check the output file:

    ```bash
    cat nohup.out
    ```

    _This file contains the output of your script._

4. If you want to see the output in real-time, you can use:

    ```bash
    tail -f nohup.out
    ```

## Finding a Process ID (PID) in Linux

1. Open a terminal.
2. List running processes:

    ```bash
    ps aux
    ```

3. Locate your process and note its **PID** (second column).
4. Or search directly:

    ```bash
    pgrep process_name
    ```

    _Replace `process_name` with the process you’re looking for._

## How to Kill a Process in Linux using PID

1. Open a terminal.
2. Use the `kill` command with the PID:

    ```bash
    kill PID
    ```

    _Replace `PID` with the actual process ID._

# Useful VSCode Shortcuts

- **Move Line Up/Down**: `Alt + Up/Down Arrow`
- **Delete Line**: `Ctrl + Shift + K`
- **Comment/Uncomment Line**: `Ctrl + /`
- **Select current line**: `Ctrl + L`
- **Copy Line Up/Down**: `Shift + Alt + Up/Down Arrow`

---

- **Add multiple cursors**: `Ctrl + Alt + Click` or `Ctrl + Alt + Up/Down Arrow`
- **Select all occurrences of a operation**: `Ctrl + F2`
- **Navigate to a specific line**: `Ctrl + G`
- **Go to Definition**: `F12`
- **Trim Trailing Whitespace**: `Ctrl + K` then `Ctrl + X`

---

- **Open Command Palette**: `Ctrl + Shift + P`
- **Open settings**: `Ctrl + ,`

# Docker Commands and Tips

- **See what is taking up space**: `docker system df`
- **Remove stopped containers, unused networks, dangling images, and build cache**: `docker system prune`
- **Remove all unused images, not just dangling ones**: `docker system prune -a`

_You may need to compact the virtual disk of your Docker Desktop installation to reclaim space after pruning._

## How to compact the virtual disk of your Docker Desktop installation

1. Find the location of your Docker Desktop virtual disk. On Windows, it is usually located at:

    ```
    C:\Users\<YourUsername>\AppData\Local\Docker\wsl\disk\docker_data.vhdx
    ```

2. Open a PowerShell terminal as Administrator.
3. Make sure WSL is not running. You can do this by running:

    ```
    wsl --shutdown
    ```

4. Run the following command to compact the virtual disk:
    ```
    diskpart
    select vdisk file="C:\Users\<YourUsername>\AppData\Local\Docker\wsl\disk\docker_data.vhdx"
    attach vdisk readonly
    compact vdisk
    detach vdisk
    exit
    ```
