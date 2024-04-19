# Drupal Local Development
This solution was found because the traditional Docker and DDEV setup was too slow for Local Development. This solution increases the speed of Drupal Local Development environment up to 10 times by using WSL2 in Windows. With this solution, Admin pages are opened within 3-4 seconds at the latest.

You can install the [extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) on Visual Studio Code and create a more effective working environment.

- Delete existing docker Desktop and DDEV
- If you already have a working build, you don't need to do anything in this step. Open WSL2. You can open PowerShell and type wsl to check if it is on.
- Download the [.wslconfig](.wslconfig) file and move it to C:\Users\USER. You can update this file according to your own computer specifications. It works most efficiently with 2GB RAM. Try to give a minimum of 2GB RAM.
- Start PowerShell as administrator.
- Enter the command ```wsl -l -v```.
  - You need to delete the unused ones in the list that appears.
  - For example ```wsl --unregister DISTRO_NAME``` = ```wsl --unregister Ubuntu-16.04```
- Enter the command ```wsl --install Ubuntu-22.04``` to install Ubuntu 22.04.
- You will be prompted for username and password. Enter this information and continue. Remember the password because you will need it in the future.
- On the Ubuntu system, enter the command ```sudo apt update && sudo apt -y upgrade```. It will ask you for a password and enter the password you set above.
- When the installation is complete, type ```exit``` and exit.
- Enter the following commands via PowerShell (for ddev installation);
  - ```Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072;```
  - ```iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/ddev/ddev/master/scripts/install_ddev_wsl2_docker_inside.ps1'))```
- Close PowerShell when the installation is complete.
- Open the file manager. On the left side, under Linux, go to the Ubuntu-22.04 folder.
- Navigate to home/USER.
- Open a folder named **projects** to work comfortably.
- Put your existing Drupal project in this folder. *This is important. Your project should be inside WSL2 for best performance.
- Open this project in Ubuntu with VS Code or whatever editor you are using.
- Put the [docroot](docroot) folder into the main project. There is only ***[settings.local.php](docroot/sites/default/settings.local.php)*** file in this directory. Thanks to the codes I added at the end of the file, you can connect to remote SOLR servers.
- When adding a terminal with + in the terminal tab in VS Code, open the *Ubuntu-22.04 (WSL)* one or you can open it by typing wsl in the terminal.
- Enter the command ```sudo cp /mnt/c/users/USER/.ssh/* /home/USER/.ssh``` to copy SSH files on Windows to Ubuntu.
- Mutagen reduced the performance of my computer a lot. So I had to turn off Mutagen. Turn off Mutagen by entering the following command.
  - ```ddev mutagen reset && ddev config global --performance-mode=none && ddev config --performance-mode=none```
- Enter the command ```sed -i 's/^M$//' .ddev/commands/web/blt && sed -i 's/^M$//' .ddev/commands/web/acli``` to fix the line endings.
- After this step, it proceeds like a standard DDEV installation.
1) ```ddev start```
1) ```ddev composer install```
1) ```ddev auth ssh```
1) ```ddev ssh```
1) ```sudo blt setup```
1) ```exit```
1) ```sudo chmod 777 docroot/sites/default/files/*```

## Known Issues
- [ ] There is a database import problem in the ACLI command I added to the system. ```ddev acli pull:database```

## Resources
- [WSL2 + Docker CE Inside Install Script](https://ddev.readthedocs.io/en/stable/users/install/ddev-installation/#windows)
- [WSL2 Best practices for set up](https://learn.microsoft.com/en-us/windows/wsl/setup/environment#file-storage) **!IMPORTANT**
- [DDEV Mutagen Performance](https://ddev.readthedocs.io/en/latest/users/install/performance/#mutagen)

