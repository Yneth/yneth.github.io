+++
title = "setup notification on ssh session"

[taxonomies]
tags = ["snippet", "bash", "ssh", "telegram"]
+++

Before executing script make sure `/etc/profile` runs scripts from `/etc/profile.d` and
do not forget to replace all required variables for telegram API.
```bash
cat <<EOF > /etc/profile.d/ssh_notif.sh
#!/bin/sh
caller="\$(ps -o comm= \$PPID)"
if [ "sshd" = "\$caller" ]; then
  BOT_TOKEN='<YOUR_TOKEN_HERE>'
  CHAT_ID='<YOUR_CHAT_ID_HERE>'
  MESSAGE='login'
  curl "https://api.telegram.org/bot\$BOT_TOKEN/sendMessage" \
    -H 'Content-Type: application/json' \
    -d "{"\""chat_id"\"": \$CHAT_ID, "\""text"\"": "\""\$MESSAGE"\""}" &> /dev/null &
fi
EOF
chmod +x /etc/profile.d/ssh_notif.sh
```
