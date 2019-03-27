This role will report the host finished ansible play
Primarily intended for the last role in an install playbook. 
Will only run if variables are set

    install_run = True
    notify_emails = "comma, separated, list"
    
'install_run' is set to false by default. Can be set on cmd line or in group/host var file.
Cobbler plugin will set install_run to true. 