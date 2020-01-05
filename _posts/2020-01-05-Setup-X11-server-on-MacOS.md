https://www.unixtutorial.org/get-x11-forwarding-in-macos-high-sierra

## Get X11 forwarding in MacOS
1. Download and install the latest release from xquartz.org website
2. Start XQuartz
3. IMPORTANT: verify xauth location
   SSH configuration file /etc/ssh/ssh_config might contain path to xauth tool, which may be incorrect depending on your OSX/MacOS version. Here's how to check:

    ```
    greys@maverick:~ $ grep xauth /etc/ssh/sshd_config
    ```
       
    if this returns nothing, you can skip to Step 4 below.  If this gives you an output, compare it to the path from the next command:

    ```
    greys@maverick:~ $ which xauth
    /opt/X11/bin/xauth
    ```
   
    If the locations differ or if path is missing, update the /etc/ssh/ssh_config file:

    ```
    greys@maverick:~ $ sudo vim /etc/ssh/ssh_config
    ```
    
    Add the following lines in the configuration file:
    
    ```
    Host *
      XAuthLocation /opt/X11/bin/xauth
    ```
    
   
4. Connect to remote server using -X option which does X11 forwarding for SSH:

    greys@maverick:~ $ ssh -X centos.unixtutorial.or

5. Check the DISPLAY variable, it should now be set correctly:
    
    ```
    greys@centos:~ $ echo $DISPLAY
    localhost:10.0
    ```

