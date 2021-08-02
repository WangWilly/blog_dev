```powershell
# 1. Network setting
wsl -l -v # 查看現在目前機器
# 確認欲使用的機器名稱
netsh interface portproxy delete v4tov4 listenport="4000" # Delete any existing port 4000 forwarding
$wslIp=(wsl -d <mechine name> -e sh -c "ip addr show eth0 | grep 'inet\b' | awk '{print `$2}' | cut -d/ -f1") # Get the private IP of the WSL2 instance
netsh interface portproxy add v4tov4 listenport="4000" connectaddress="$wslIp" connectport="4000"
```

```bash
# 2. Install nodejs and npm
sudo apt-get update
sudo apt-get upgrade
sudo apt install nodejs
sudo apt install npm
# Restart terminal

# 3. Install pandoc
wget # From https://github.com/jgm/pandoc/releases
sudo dpkg -i # <XXXXXXXX-amd64.deb wget from internet>

# 4. Install hexo
sudo npm install hexo-cli -g
hexo # 測試 hexo 是否被正確安裝

# 5. (In existing project)
# .
# ├── _config.yml
# ├── package.json
# ├── scaffolds/
# ├── source/
# └── themes/
npm install # Adding a folder and a file
# .
# ├── node_modules/
# └── package-lock.json
hexo g # Adding a folder and a file
# .
# ├── public/
# └── db.json
hexo s # Start localhost server
# 後台撰寫
# http://localhost:4000/willywangkaa/admin/
```