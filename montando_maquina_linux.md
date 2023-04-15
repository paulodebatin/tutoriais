##### Tutorial de criação de uma máquina para desenvolvimento do zero no Ubuntu

###### 1) Instalação do Linux Ubuntu
a) Baixe a ISO do Ubuntu Desktop no site https://releases.ubuntu.com/jammy/. No momento da montagem deste tutorial a versão era a 22.04.2
b) Baixar o programa e criar um pendrive bootável a partir da ISO.
https://rufus.ie/pt_BR/
Obs.:  A formatação precisa ser em Fat32. Já tem um pendrive montado (SanDisk vermelho de 8GB)
c) Entrar na BIOS e configurar o BOOT pela USB 
Obs:
Para entrar na BIOS do Dell tecle F2;
Na BIOS, o security boot precisa estar desabilitado. O modo de boot precisa ser em UEFI e não Legacy;

###### 1) Instalar Shell ZSH
~~~
sudo apt-get update
sudo apt install zsh
~~~
Agora você precisa configurar o ZSH como o Shell padrão. Para isso execute:
~~~
chsh -s $(which zsh)
~~~
Depois disso, você precisa reiniciar a máquina. Entre no terminal. A primeira vez vai mostrar o menu do ZSH (escolha a opção2). Depois execute echo $SHELL. Você verá que agora é o ZSH.

###### 2) Instalar GIT
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
plugins=( 
    # other plugins...
    zsh-autosuggestions
)
restart o terminal


###### 4) Instalar notepad++
sudo snap install snapd
sudo snap install notepad-plus-plus


###### 5) Outas instalações
Visual Code:
Baixar o arquivo .deb da página https://code.visualstudio.com/docs/?dv=linux64_deb e sudo apt install home/paulo/Transferências/code_1.77.3-1681292746_amd64.deb