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

###### 3) Instalar themes para zsh


https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh
Para escolher os temas: https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
cd ~
sudo nano .zshrc
Depois preciso recarreger usando o camando: source .zshrc 