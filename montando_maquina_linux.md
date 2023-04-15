##### Tutorial de criação de uma máquina para desenvolvimento do zero no Ubuntu

**Instalação do Linux Ubuntu:

a) Baixe a ISO do Ubuntu Desktop no site https://releases.ubuntu.com/jammy/. No momento da montagem deste tutorial a versão era a 22.04.2
b) Baixar o programa para criação de um pendrive bootável a partir da ISO.
https://rufus.ie/pt_BR/
Obs.:  A formatação precisa ser em Fat32. Já tem um pendrive montado (SanDisk vermelho de 8GB)
c) Entrar na BIOS e configurar o BOOT pela USB :
Para entrar na BIOS do Dell tecle F2;
Na BIOS, o security boot precisa estar desabilitado. O modo de boot precisa ser em UEFI e não Legacy;

---

Instalar Shell ZSH:
~~~
sudo apt-get update
sudo apt install zsh
~~~
Agora você precisa configurar o ZSH como o Shell padrão. Para isso execute:
~~~
chsh -s $(which zsh)
~~~
Depois disso, você precisa reiniciar a máquina. Entre no terminal. A primeira vez vai mostrar o menu do ZSH (escolha a opção 2). Depois execute echo $SHELL. Você verá que agora é o ZSH.

---

Instalar GIT:

sudo apt install git

git config --global user.email "you@example.com"
git config --global user.name "Your Name"

Gerar uma chave SSH:
ssh-keygen -t ed25519 -C "your_email@example.com" --> vai dando enter

Adding your SSH key to the ssh-agen:
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519


Copiar a chave: 
cat ~/.ssh/id_ed25519.pub

No github, settings > SSH and GPG keys e adione a chave

Depois disso pode clonar o projeto pegando o link com SSH e não HTTP

###### 3) Instalar themes para zsh
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
→ para verficiar digite env
sudo apt install fonts-powerline -y

Para ver os themes: https://github.com/ohmyzsh/ohmyzsh/wiki/Themes

Mudando o theme: sudo nano ~/.zshrc 


- Instalar plugin AutoSuggestion
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

sudo nano ~/.zshrc
```
plugins=( 
    # other plugins...
    zsh-autosuggestions
)
```
restart o terminal


###### 4) Instalar notepad++
sudo snap install snapd
sudo snap install notepad-plus-plus


###### 5) Outas instalações
Visual Code:
Baixar o arquivo .deb da página https://code.visualstudio.com/docs/?dv=linux64_deb e sudo apt install home/paulo/Transferências/code_1.77.3-1681292746_amd64.deb

Docker: 
https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository

-> Dar permissão ao seu usuário:
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
reboot

---

Chrome:
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb

---

Gerenciador de versões Java (sdkman):
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"

Ver versões do java: sdk ls java
Instalar: sdk install java <versão>. Ex.: sdk install java 21.ea.18-open
Trocar de versão: sdk use java <versão>
Setar uma versão como default: sdk default java <versão>
Ver a versão corrente: sdk current java

---

Eclipse:
Baixe o arquivo em https://www.eclipse.org/downloads/packages/
Depois: tar xfz ~/

--- 

Gerenciadores de versões Node (nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash --> Depois feche a abra novamente o terminal e digite: nvm --version

Ver versões:
nvm ls-remote
nvm ls

Instalar uma versão: nvm install v19.9.0
Usar uma versão específica: nvm use <versão>

---