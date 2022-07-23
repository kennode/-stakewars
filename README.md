# stakewars
 stakewars episode 3
 
 
 
![image](https://user-images.githubusercontent.com/109859686/180608171-e70bbc2d-551f-4a07-9e4e-01e8ae1497ba.png)

# stakewars node setup 
Official documentation:

https://github.com/near/stakewars-iii

Chain explorer:

https://explorer.shardnet.near.org/

Wallet: 

https://wallet.shardnet.near.org/

### hardware requirement 

- CPU	4-Core CPU with AVX support

- RAM	8GB DDR4

- Storage	500GB SSD

# Challenge 001
membuat dompet dompet Shardnet  & deploy NEAR CLI

- membuat wallet

https://wallet.shardnet.near.org/

- Setup NEAR-CLI

Pertama pastikan mesin linux up-to-date.
```
sudo apt update && sudo apt upgrade -y
```
### Install developer tools, Node.js, and npm

```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
sudo apt install build-essential nodejs
PATH="$PATH"
```
Periksa Node.js dan npm versi:

```
node -v
```
v18.x.x
```
npm -v
```
8.xx
### Install NEAR-CLI
```
sudo npm install -g near-cli
```
> Sekarang setelah NEAR-CLI terinstal, mari kita uji CLI dan gunakan perintah berikut untuk berinteraksi dengan blockchain serta untuk melihat statistik validator.
```
export NEAR_ENV=shardnet
```
```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
```

# Challenge 002
Sebelum memulai kamu bisa memeriksa mesin CPU apakah memenuhi syarat
```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
```
> Supported

##### Install developer tools:
```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```
#####  Install Python pip:

```
sudo apt install python3-pip
```
##### Set the configuration:

```
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```

##### Install Building env
```
sudo apt install clang build-essential make
```

##### Install Rust & Cargo
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Anda akan melihat berikut ini:

![image](https://user-images.githubusercontent.com/109859686/180610308-5acf2396-7dab-4797-b4aa-e8a47ca38100.png)


Tekan 1 dan tekan enter.

##### Source the environment
```
source $HOME/.cargo/env
```





  
