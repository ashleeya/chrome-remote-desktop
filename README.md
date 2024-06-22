**Ubuntu 22.04 + Chrome Remote Desktop**
**I just want to log in with the main screen, not the extended screen. So, record the modified file for the next update.**
website: https://remotedesktop.google.com/
1. Download deb and install it
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

2. Stop Sevice of Chrome Remote Desktop
/opt/google/chrome-remote-desktop/chrome-remote-desktop --stop
###
sudo vim /opt/google/chrome-remote-desktop/chrome-remote-desktop
"def launch_session(self, server_args, backoff_time):
    """"""Launches process required for session and records the backoff time
    for inhibitors so that process restarts are not attempted again until
    that time has passed.""""""
    logging.info(""Setting up and launching session"")
    self._setup_gnubby()
    display=self.get_unused_display_number()
    self.child_env[""DISPLAY""]="":%d"" % display
#    self._launch_server(server_args)
#    if not self._launch_pre_session():
#      # If there was no pre-session script, launch the session immediately.
#      self.launch_desktop_session()
    self.server_inhibitor.record_started(MINIMUM_PROCESS_LIFETIME,
                                      backoff_time)
    self.session_inhibitor.record_started(MINIMUM_PROCESS_LIFETIME,
                                     backoff_time)

"
