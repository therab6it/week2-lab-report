# Welcome to CSE 15L!


Hope you're taking CSE 12 too. Okay, let's get started.   
You'd need VSCode, so if you don't have that installed, follow these steps.



## 1. Installing VSCode

Head on over to the [Visual Studio Code](https://code.visualstudio.com/) website, and it should look something like this.

![Image](https://i.ibb.co/VHWTVFh/Screenshot-1.png)


Click the big blue button to download and install the software onto your computer. Once that's done, open the software. The Welcome splash screen should look something like this.

![Image](https://i.ibb.co/6yx5gv7/Screenshot-2.png)


You're all set!

---

## 2. Remotely Connecting

If your computer is running Windows, you'd have to download and install a program called OpenSSH.


### Installing OpenSSH

There are two ways to install OpenSSH onto your computer. You can either: 

1. *Windows 10*: Go to **Settings** -> **Apps** -> **Apps & Features** -> **Optional Features**   
   *Windows 11*: Go to **Settings** -> **Apps** -> **Optional Features**   
   
   Then, scan the list to see if the OpenSSH is already installed. If not, at the top of the page, select **Add a feature**, then:   
   
   * Find OpenSSH Client, then click Install
   * Find OpenSSH Server, then click Install


2. Click *Windows + x*, then click *a*. This opens Windows PowerShell as an administrator. First, ensure that OpenSSH hasn't already been installed by running this line of code.

   ```
   Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
   ```
   
   The output should look like this if niether has been installed.
   
   ```
   Name  : OpenSSH.Client~~~~0.0.1.0
   State : NotPresent

   Name  : OpenSSH.Server~~~~0.0.1.0
   State : NotPresent
   ```
   
   Then, install the server or client as needed.
   
   ```
   # Install the OpenSSH Client
   Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

   # Install the OpenSSH Server
   Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
   ```
   
   This might take a while. Both should return the following output.
   
   ```
   Path          :
   Online        : True
   RestartNeeded : False
   ```
   
   Finally, to start and configure OpenSSH for initial use run the following commands.
   
   ```
   # Start the sshd service
   Start-Service sshd
   ```
   
   ```
   # OPTIONAL but recommended:
   Set-Service -Name sshd -StartupType 'Automatic'
   ```
   
   ```
   # Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
   if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
   } else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
   ```
   
Great! Now you need to look up your account for CSE 15L on this [UC San Diego website](https://sdacs.ucsd.edu/~icc/index.php). You might have to change your account's password.
Keep your account name and password handy for the next step.



### Configuring remote access

Open the terminal in VSCode and type out the following command. The `***` are a placeholder for your three account-specific characters, like aws or app, so replace them!

```
ssh cs15lwi22***@ieng6.ucsd.edu
```

Since this is the first time you're connecting to the server, you should see the following.

```
⤇ ssh cs15lwi22***@ieng6.ucsd.edu
The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/jxp1wUQcec.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type in `yes` and hit enter.

Once you've entered your password (which may sometimes take a few attempts to get through correctly), you would have successfully connected to a computer remotely!

Your screen should look something like this.

```
⤇ ssh cs15lwi22***@ieng6.ucsd.edu
The authenticity of host 'ieng6-202.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksJSzyeLSH+sySHnHAtLUHngrPEyZTDl/15iuSnmAle.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
Password: 
Last login: Thu Jan  13 14:03:05 2022 from d28-23-253-200.dim.wideopenwest.com
quota: No filesystem specified.
Hello cs15lwi22***, you are currently logged into ieng6-203.ucsd.edu

You are using 0% CPU on this system

Cluster Status 
Hostname     Time    #Users  Load  Averages  
ieng6-201   14:05:01   40  0.17,  0.22,  0.30
ieng6-202   14:05:01   56  2.26,  2.36,  2.37
ieng6-203   14:05:01   22  0.18,  0.22,  0.30

Thu Jan 13, 2022 2:08pm - Prepping cs15lwi22
[cs15lwi22***@ieng6-201]:~:1$
```

If, at any point, you wish to terminate the connection, simple type in `exit` onto the console.

---

## 3. Trying Some Commands

Some basic commands are listed below.

* `$cd <path>`               : Changes the directory or folder to the specified `<path>`
* `$cd ..`                   : Changes the directory to the parent folder
* `$cd ~`                    : Changes the directory to the root or main folder
* `$ls`                      : Lists the folders and files present in the current directory
* `$ls -a`                   : Lists all the folders and files (including the hidden ones) present in the current directory
* `$cp <filepath> <to path>` : Copies the file at `<file path>` and adds it to the `<to path>`
* `$mkdir <name>`            : Makes a new directory or folder with the specified `<name>`
* `$rmdir <name>`            : Removes or deletes the directory with the specified `<name>`
* `$echo <text>`             : Prints the `<text>` to the console
* `$cat <filename>`          : Prints the contents of the file `<filename>` to the console

For more commands, check out this [Hostinger.com](https://www.hostinger.com/tutorials/linux-commands) webpage.

---

## 4. Moving files to the remote server

It is important to know how to send files across to the remote server from your computer. This can be done with the `scp` command.

First, ensure you are logged out from the server, and the terminal is opened from a folder from your computer. Essentially, ensure the command is run on the *client* computer. Type in the following command.

```
scp <filepath> cs15lwi22***@ieng6.ucsd.edu:~/
```

Once again, the `***` is simply a placeholder. You will have to type in your three account-specific characters. This command will copy the file located at `<filepath>`, login to your account on the server, and paste it in the home directory. If you want the file in a specific folder, append the desired directory path to the code. You would have to type your password in when prompted (more on this in the next section).

Here is an example to display this. The 'WhereAmI.java' file gets the information of:
  1. Name of the operating system
  2. User name
  3. Home directory
  4. Current directory

When run on the client computer, the output look something like this.

![Image](https://i.ibb.co/3RWTZnd/Screenshot-3.png)


Using `scp`, the same file can be sent over to the remote server like this.

![Image](https://i.ibb.co/BBHtwKq/Screenshot-4.png)


Once this is done, logging in to the server and typing in `ls` to the terminal displays the file being present. Running the Java file on the server will produce a different set of results.

![Image](https://i.ibb.co/B3NbDyP/Screenshot-5.png)


Try this yourself! Here's the code for the WhereAmI.java file.  
```
class WhereAmI {
  public static void main(String[] args) {
    System.out.println(System.getProperty("os.name"));
    System.out.println(System.getProperty("user.name"));
    System.out.println(System.getProperty("user.home"));
    System.out.println(System.getProperty("user.dir"));
  }
}
 ```


---

## 5. Setting an SSH Key

As you may have noticed, it would be very tedious to enter your password everytime you login from your own computer or when you have to transfer a file. This step can be eliminated using the Secure Shell Protocol (SSH). Using a set of public and private keys encoded using SHA256, SSH enables a secure means to access the server from a trusted client (your computer). Here's how you set it up.

Open the terminal on your computer. Ensure you are **not** logged on to the server yet. Run the following command.

```
ssh-keygen
```

You should expect the following output.

![Image](https://i.ibb.co/N3wpcST/Screenshot-6.png)


When prompted to enter a file path, simply copy the path within the parentheses and paste it.


You have now generated a public and private key. The public key will have to be sent to the server.

So login and create the following folders on the server to store the public key using the following commands.

```
mkdir .ssh
cd .ssh
mkdir authorized_keys
```

Ensure that the you have created the folders accurately using the `ls` command.   
Logout of the server.


On your computer, locate the folder in which the public and the private keys are stored; it is the file path you provided earlier. Copy the file path for the **public** key. It should be a file with a *.pub* extension. With the file path copied, run the following command on the terminal on your computer. 

```
scp <filepath> cs15lwi22***@ieng6.ucsd.edu:~/.ssh/authorized_keys
```

And you're all set!

---

## 6. Optimizing Remote Running

They previous sections had a lot of steps that can actually be condensed into lesser effort to get the job done.

Using an IDE instead of the terminal or Notepad is one of the most basic ways to edit a piece of code. Make sure you use VSCode or other IDEs like IntelliJ. This allows more efficient local editing.   
Then, using the integrated terminal inside VSCode can help stay better organised with all the required operations taking place inside a single application window. You can also install the **Remote Development** extension on VSCode. Check out this [link](https://code.visualstudio.com/docs/remote/ssh#_connect-to-a-remote-host) on how to set it up. This feature displays the files located on the server, in a graphical interface, which might help many beginners.   
Once a file is ready to be transferred to and run on the server, you may find it easier to execute all these actions with one line of code, by using double inverted commas (" ") to include any code that is to be run on the server.

For example, with just two lines of code, WhereAmI.java can be sent to the server, complied on the server, and executed on the server as follows.

```
scp WhereAmI.java cs15lwi22***@ieng6.ucsd.edu:~/
ssh cs15lwi22***@ieng6.ucsd.edu "javac WhereAmI.java; java WhereAmI"
```

A quick way to access these commands is by using the up arrow key to scroll through all the previously executed commands within that session.   


Have fun!   


