**Ubuntu 22.04 + Chrome Remote Desktop**

I just want to log in with the main screen, not the extended screen. So, record the modified file for the next update.

website: https://remotedesktop.google.com/
1. Download deb and install it
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

2. Stop Sevice of Chrome Remote Desktop
```
~~(old version) /opt/google/chrome-remote-desktop/chrome-remote-desktop --stop~~
**(new on ubuntu24.04) sudo systemctl stop chrome-remote-desktop@$USER**
```

###

sudo vim /opt/google/chrome-remote-desktop/chrome-remote-desktop

```
FIRST_X_DISPLAY_NUMBER = 0  #change 20 to 0
```


Disable 2 line
```
#while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
# display += 1
```

```
"def launch_session(self, server_args, backoff_time):
    """"""Launches process required for session and records the backoff time
    for inhibitors so that process restarts are not attempted again until
    that time has passed.""""""
    logging.info(""Setting up and launching session"")
    self._setup_gnubby()
    display=self.get_unused_display_number()       #add this line
    self.child_env["DISPLAY"]=":%d" % display  #add this line
#    self._launch_server(server_args)		   #disable this line
#    if not self._launch_pre_session():		   #disable this line
#      # If there was no pre-session script, launch the session immediately.
#      self.launch_desktop_session()		   #disable this line
    self.server_inhibitor.record_started(MINIMUM_PROCESS_LIFETIME,
                                      backoff_time)
    self.session_inhibitor.record_started(MINIMUM_PROCESS_LIFETIME,
                                     backoff_time)
```

3. Ensure KDE Plasma is Installed (if needed): Although Kubuntu should already have KDE Plasma installed, you can verify or reinstall it with:
```
sudo apt install kde-plasma-desktop 
```
Edit Chrome Remote Desktop Session Configuration:
Open the Chrome Remote Desktop session file:
```
sudo nano /etc/chrome-remote-desktop-session
```
Set KDE Plasma as the Session: Modify the file to start the KDE Plasma session by adding this line:
```
exec /usr/bin/startplasma-x11
```
4. Start the service
~~(old version) /opt/google/chrome-remote-desktop/chrome-remote-desktop --start~~
```
sudo systemctl start chrome-remote-desktop@$USER #kubuntu 24.04
```
