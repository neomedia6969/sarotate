# SARotate
For rotating Google service accounts to bypass bans or any other myriad of reasons.

"I am not a coder. I have no idea what I am doing. Use at your own risk!"

## Changelog v2.0
   - [Added]: Script now uses a .yml config file.
   - [Added]: The config file is passed through the CLI with the script by `-c`.
   - [Added]: If no config file is passed then it will default to `config.yml`
   - [Added]: Different rclone config files can be defined in the .yml file.
   - [Added]: Script has a little bit of error handling now and will exit if a config file is not passed through to the script.
   - [Added]: Set files are no longer needed and the .yml holds all of the remotes/ports needed.
   - [Added]: Improved logging.
   - [Added]: Improved looping logic.
   - [Added]: Comments in the .yml file will be stripped before being run through the script.
   - [Added]: Check to see if log file exists, and create it if not.
   - [Old]: Apprise notifications. 
   - [Old]: Logging.
   - [Old]: Initial looping logic. 


## What is it?
This script was created with the help of 88lex. It uses portion of his amazing sasync (https://github.com/88lex/sasync) script. This was written mainly for CloudBox users but can be utilized by anyone.

# Requirements
 1. yq: https://github.com/mikefarah/yq
   - If you have Cloudbox installed then you will already have this installed.
   - If you do not have Cloudbox you must follow the below steps to install it.
 2. Install Instructions:
   - Get current version of yq: `wget -qO- https://api.github.com/repos/mikefarah/yq/releases/latest | jq -r ".assets[] | select(.name | test(\"yq_linux_amd64\")) | .browser_download_url"`
   - `sudo wget -o /usr/local/bin/ <insert above command result here>`
   - `sudo mv /usr/local/bin/yq_linux_amd64 /usr/local/bin/yyq`
   - `sudo chmod 775 /usr/local/bin/yyq`
   - `sudo chown root:root /usr/local/bin/yyq`
   - `yyq --version`
  3. We rename this due to the fact that there is 2 other versions of yq out there that are named the same.
  

## Installation
1. `cd /opt`

2. `git clone https://github.com/Visorask/sarotate.git`

3. `sudo chown -R user:group sarotate` - Run `id` to find your user / group.

4. `cd /opt/sarotate && chmod +x sarotate`

5. `nano sarotate` Edit the variables to match your settings. Save and close.

6. `nano sarotate.set` Add in w/e values you need.

7. You will also need the latest rclone, minimum version = 1.52. The beta is no longer needed. ```curl https://rclone.org/install.sh | sudo bash```

8. Once you have done all this then you will need to restart the mounts you want to rotate before running this script. A command might look like this: ```sudo systemctl restart example.service```

  Otherwise you might get an error like this:
```
Failed to rc: Failed to read rc response: 404 Not Found: {
        "error": "couldn't find method \"backend/command\"",
        "input": {
                "command": "set",
                "fs": "<remotename>:",
                "opt": {
                        "service_account_file": "<file/path/1.json>"
                }
        },
        "path": "backend/command",
        "status": 404
}
```

---

### Extra Information 
   For the script to work you must edit your mounts to have either `--rc-no-auth` or `--rc-user=<username>` and `--rc-pass=<password>`. You can add those into a live mount by doing:
  
   - `sudo nano /etc/systemd/system/<nameofmount>.service` (Fill in <nameofmount> with your information.)
   - `sudo systemctl daemon-reload`
   - `sudo systemctl restart /etc/systemd/system/<nameofmount>.service` (Fill in <nameofmount> with your information.)
   - `./sarotate sarotate.set`
   
  You can also use this amazing script which has `--rc-no-auth` built into it, as well as, creating as many mounts as you want. https://github.com/maximuskowalski/smount/tree/sarotate-rclonebeta
  
---
   
## If you would like to use crontab then follow the below steps:
  1. `crontab -e`
  2. Add `@reboot sleep 1m && /opt/sarotate/sarotate`
 ---
 
## If you would like to use systemd then follow the below steps: 
  1. `cd /opt/sarotate/system`
  2. `nano sarotate.service`  
  3. Edit the user / group from `changethis` to your user / group. -Run `id` to find your user / group.   
  4. Exit and save the file.   
  5. `sudo cp /opt/sarotate/system/sarotate.service /etc/systemd/system/`  
  6. `sudo systemctl daemon-reload`  
  7. `sudo systemctl enable sarotate.service`  
  8. `sudo systemctl start sarotate.service`  
  9. If you would like to check that it is running working correctly run: `sudo service sarotate status`
---

## Troubleshooting
Will add more as people run into any issues.