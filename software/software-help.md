# 輔助

## 自動完成指令 Bash auto completion

*安裝*

```shell
sudo apt update
sudo apt install bash-completion
```

*測試*

```shell
cat /etc/profile.d/bash_completion.sh
```

*載入*

```shell
## source it from ~/.bashrc or ~/.bash_profile ##
echo "source /etc/profile.d/bash_completion.sh" >> ~/.bashrc
## Another example Check and load it from ~/.bashrc or ~/.bash_profile ##
grep -wq '^source /etc/profile.d/bash_completion.sh' ~/.bashrc || echo 'source /etc/profile.d/bash_completion.sh'>>~/.bashrc
```

```
source /etc/profile.d/bash_completion.sh
```

## 變更 Bash

找出目前 shell

```shell
echo $SHELL
```

變更 shell

```shell
chsh -s /bin/bash
chsh -s /bin/zsh
```

## 參考資料
* [How to add bash auto completion in Ubuntu Linux - nixCraft](https://www.cyberciti.biz/faq/add-bash-auto-completion-in-ubuntu-linux/)
