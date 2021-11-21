# [Anthem](https://tryhackme.com/room/anthem) by [Chevalier](https://tryhackme.com/p/Chevalier)

```bash
IP = 10.10.220.204*
Difficulty: Easy 
Machine OS: Windows
Learning Platform: tryhackme.com
Finished on: Arch Linux
```

**Note: IP address may vary.*

## **Reconnaissance**

### *Scoping and Preparation*

* Connect to OpenVPN Server using:

    ``sudo openvpn {PATH_TO_OVPN_FILE}``

* I used my tool [CTFRecon](https://www.github.com/hambyhacks/CTFRecon) to automate directory creation, network scanning, web directory brute-forcing and adding entry to `/etc/hosts` file.

* To use [CTFRecon](https://www.github.com/hambyhacks/CTFRecon):

    ```bash
    1. git clone https://www.github.com/hambyhacks/CTFrecon
    2. cd CTFRecon
    3. chmod +x ctfrecon.sh && cp ctfrecon.sh ../ 
    #to move ctfrecon.sh to your working directory.
    4. sudo ./ctfrecon.sh [IP] [DIRECTORY NAME] [PLATFORM] [WORDLIST] 
    #platform refers to hackthebox(htb) or tryhackme(thm). Wordlist is used for GoBuster directory brute-forcing.
    ```

### *Preliminary Enumeration via nmap*

#### *Table 1.1: nmap Results Summary*

PORT | STATUS | SERVICE | VERSION
:---: | :---: | :---: | :---:
80/tcp | open | http | *Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)*
3389/tcp | open | ms-wbt-server | *Microsoft Terminal Services*

![Nmap Scan](../imgs/Anthem/Anthem_nmapScan.png)

Machine OS: Based on nmap results, it is a Windows OS machine.

## Enumeration

### *Manual Web Enumeration*

* Looking at the webpage at port 80, we are greeted by this webpage below.

![Webpage at port 80](../imgs/Anthem/webpage_port80.png)

* We can also look at the web technologies used in ``anthem.thm`` using [Wappalyzer](https://www.wappalyzer.com/).

![Wappalyzer](../imgs/Anthem/anthem_webTech.png)

* We also tried to check if there is a file named `robots.txt`.

![Robots](../imgs/Anthem/anthem_robots_txt.png)

* We got some directories and a password-looking string: `UmbracoIsTheBest!`

*Note: Manual Enumeration is important.*

* We try to check pages in the web server and we get some information about the email format of `anthem.com` by looking at the posts done by `Jane Doe` user.

![Email Format](../imgs/Anthem/email_format.png)

* We also check the other post which the contents describe their `admin` in a poem. So we tried to search it through google.

![Poem](../imgs/Anthem/poem_about_admin.png)

![Admin](../imgs/Anthem/admin_user_google_search.png)

* Nice! we got some informatio about `admin` user!

*Tip: When in doubt, search it in Google.*

#### *Table 1.2: Credentials*

Username | Password
:---: | :---:
sg@anthem.com | UmbracoIsTheBest!

## Exploitation

*Steps to reproduce:*

1. Navigate to `/umbraco` directory in web server which is a login page using gathered credentials.
    ![Umbraco Login](../imgs/Anthem/login_umbraco.png)

2. You should be logged in as `SG` which is an administrator account.
    ![Logged In](../imgs/Anthem/sg_admin.png)

3. Login via RDP (Remote Desktop Protocol) using `xfreerdp`.

    Syntax: ``xfreerdp /u:{USERNAME} /p:{PASSWORD} /v:{IP:PORT}``

    ![RDP Login](../imgs/Anthem/RDPing_to_anthem.png)

4. You should be logged in as `SG` via RDP.

    ![RDP Success](../imgs/Anthem/success_rdp.png)

## Privilege Escalation / Post-Exploitation

### *Internal Enumeration*

#### *Table 1.3: Checklist for Windows Internal Enumeration*

COMMAND | DESCRIPTION
:---: | :---:
``whoami``  | gets current user name
``whoami /priv`` | gets privileges granted on user
``net users`` | lists all users in the machine.

*Notes: This is not a complete list. To see more detailed list, refer to [this](https://book.hacktricks.xyz/windows/checklist-windows-privilege-escalation).*

*Tip: When nothing else makes sense, try to use [LinPEAS](https://github.com/carlospolop/PEASS-ng) ([winPEAS](https://github.com/carlospolop/PEASS-ng) for windows machines.).*

* We can see the `user.txt` file hanging in the desktop of `SG` user. `Double-Click` to open the file or if you want to to it in the terminal:

    Syntax: ``cd C:\Users\SG\Desktop`` then ``type user.txt``

### *Vertical Privilege Escalation*

* We can try to list all directories in `C:\` by using:

    Syntax: `cd C:\` then `dir /a`

    ![List All Directories](../imgs/Anthem/list_all_files.png)

* Run powershell.

    ![Perms](../imgs/Anthem/ps_to_get_dir_perm.png)

* Let's check if there is another user in the machine.

    Syntax: `net users`

    ![Net Users](../imgs/Anthem/net_user.png)

* There is a directory named `backup`. Let's look what are the contents and who owns that directory.

    Syntax: `dir C:\backup | Get-Acl`

    Explanation: `runs powershell and check access lists control for C:\backup directory` 

    ![C contents](../imgs/Anthem/C_backup_contents.png)

* We tried to get the content of `restore.txt` and we dont have access. Since `SG` is the owner of file. We can try to change the access permission of that file. `/e` denotes edit the permission but do not add new permission. `/p` denotes to add permission. `f` means full control.

    ![Access Denied](../imgs/Anthem/cacls_change_perm.png)

    Syntax: `cacls {FILE/DIR} /e /p {USER}:{ACCESS}`

* Getting the contents of the `restore.txt` file gives us a password like string.

* Let's try to escalate our privileges by using the `runas` command.

    Syntax: `runas /user:{USERNAME} {COMMAND}`

    ![Run As](../imgs/Anthem/escalation.png)

* We got admin command prompt!

    ![Admin cmd](../imgs/Anthem/admin_cmd.png)

    ![rooted](../imgs/Anthem/rooted.png)

### STATUS: ROOTED

The next two steps are not necessary for completion of the machine but it completes the 5 Phases of Penetration Testing.

## Post Exploitation / Maintaining Access

* Added another administator user for easy access.

## Clearing Tracks

* Removed all logs and footprints to to prevent risk of exposure of breach to security administrator.

## **Status: Finished**

Feel free to reach out and if there is something wrong about the above post. Feedbacks are also appreciated :D

### Donation Box

#### *Not required but appreciated :D*

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/hambyhaxx)

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/hambyhaxx)

<-- [Go Back](https://hambyhacks.github.io/)