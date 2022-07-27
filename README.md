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
> tekan y
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
> contoh gambar

Tekan 1 dan tekan enter.

##### Source the environment
```
source $HOME/.cargo/env
```
#### Clone `nearcore` project from GitHub

```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```

```
git checkout 0d7f272afabc00f4a076b1c89a70ffc62466efe9
```

```
cargo build -p neard --release --features shardnet
```
> tunggu sampai proses ini selesai,proses ini akan memakan waktu beberapa saat tergantung performa yang anda miliki
### Initialize working directory

```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```

### Replace the config.json

```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```
### Jalankan Node

```
cd ~/nearcore
./target/release/neard --home ~/.near run
```
> tunggu sampai proses download header 100% dan download block 100% ,ketika suda 100% anda akan melihat pada bagian EpochId(11111111111111111111111111111111) akan berubah ,lalu kemudian klik ctrl + c

### membuat service
> disini 2 ada pilihan menggunakan vi atau menggunakan tee jadi kamu harus memilih salah satunya

 menggunakan vi
 ```
 sudo vi /etc/systemd/system/neard.service
 ```
 lalu paste 
 ```
 [Unit] 
Description=NEARd Daemon Service 
[Service] 
Type=simple 
User=$USER
#Group=near 
WorkingDirectory=$HOME/.near
ExecStart=$HOME/nearcore/target/release/neard run 
Restart=on-failure 
RestartSec=30 
KillSignal=SIGINT 
TimeoutStopSec=45 
KillMode=mixed 
[Install] 
WantedBy=multi-user.target
```
> Kemudian tekan ESC dan ketik :wq enter

menggunakan tee
```
sudo tee /etc/systemd/system/neard.service > /dev/null <<EOF 
[Unit] 
Description=NEARd Daemon Service 
[Service] 
Type=simple 
User=$USER
#Group=near 
WorkingDirectory=$HOME/.near
ExecStart=$HOME/nearcore/target/release/neard run 
Restart=on-failure 
RestartSec=30 
KillSignal=SIGINT 
TimeoutStopSec=45 
KillMode=mixed 
[Install] 
WantedBy=multi-user.target 
EOF
```
### jalankan service
```
sudo systemctl daemon-reload
sudo systemctl enable neard
sudo systemctl start neard
journalctl -n 100 -f -u neard
```
### optional
```
sudo apt install ccze

journalctl -n 100 -f -u neard | ccze -A
```
### Menghubungkan Wallet ke NEAR-CLI
```
near login
```
![image](https://user-images.githubusercontent.com/109859686/180615392-cd2beae1-bdd8-4e1c-b32e-1f55b00005b8.png)
> contoh gambar

> kamu akan melihat link contoh seperti di gambar copy dan paste di browser kamu,note:pastikan kamu sudah membuat wallet

![image](https://user-images.githubusercontent.com/109859686/180615533-277eaf56-5a35-47e3-b728-06201200c9fc.png)
> contoh gambar

> klik connect

![image](https://user-images.githubusercontent.com/109859686/180615561-a404556c-b5bd-4feb-8174-49a0cbb0666a.png)
> contoh gambar

> masukan accounid kamu contoh gambar:  tester01.shardnet.near

![image](https://user-images.githubusercontent.com/109859686/180615642-56204b29-f771-4e9d-a627-a1f4f067105c.png)
> contoh gambar

> kamu akan melihat yang seperti ini,lalu kembali ke terminal dan paste accounid kamu seperti contoh yang ada di gambar

![image](https://user-images.githubusercontent.com/109859686/180615722-6de0d038-f4dd-420b-90f5-66136cab101c.png)
> contoh gambar

### Generate Key untuk validator_key.json
```
near generate-key xx.factory.shardnet.near
```
> Ganti xx dengan nama wallet 
### pindahkan file validator_key.json ke folder .near
```
cp ~/.near-credentials/shardnet/xx.factory.shardnet.near.json ~/.near/validator_key.json
```
> Ganti xx dengan nama wallet 
```
nano ~/.near/validator_key.json
```
> temukan kata private_key dan mengubahnya menjadi secret_key kemudian CTRL + O enter CTRL + X  enter

 Konten file harus dalam pola berikut:

```
 {
  "account_id": "xx.factory.shardnet.near",
  "public_key": "ed25519:HeaBJ3xLgvZacQWmEctTeUqyfSU4SDEnEwckWxd92W2G",
  "secret_key": "ed25519:****"
}
```

> NOTE simpan isi file node_key.json dan validator_key.json ditempat aman ,dan jangan sampai hilang.!
```
nano ~/.near/node_key.json
```

```
nano ~/.near/validator_key.json
```

### Challenge 003

# Mounting Staking Pool

Membuat Staking Pool

```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "nama_wallet", "owner_id": "xx.shardnet.near", "stake_public_key": "public_key_kamu", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="xx.shardnet.near" --amount=30 --gas=300000000000000
```
> nama_wallet ganti dengan nama wallet kamu (contoh : tester01). public_key_kamu ganti dengan public_key wallet kamu. XX ganti dengan nama wallet kamu

![image](https://user-images.githubusercontent.com/109859686/180616641-4ef730f4-e252-40ee-bc4b-c2312678dc16.png)
 > contoh gambar
 
 >jika berhasil akan muncul seperti yang ada digambar,anda bisa coppy link explorer untuk melihat nya langsung di explorer 

# Deposit dan Stake NEAR
```
near call xx.factory.shardnet.near deposit_and_stake --amount jumlah --accountId xx.shardnet.near --gas=300000000000000
```
> edit xx menjadi nama wallet kamu dan ubah kata jumlah yang mau kamu inginkan,NOTE jangan masukan semua balance yang kamu punya

 
# unstake dalam jumlah

```
near call xx.factory.shardnet.near unstake '{"amount": "jumlah"}' --accountId xx.shardnet.near --gas=300000000000000
```

# unstake semua
```
near call xx.factory.shardnet.near unstake_all --accountId xx.shardnet.near --gas=300000000000000
```

> edit xx menjadi nama wallet kamu dan ubah kata jumlah yang mau kamu inginkan dan Unstaking membutuhkan waktu 2-3 epoch agar bisa di withdraw 

# Withdraw

```
near call xx.factory.shardnet.near withdraw_all --accountId xx.shardnet.near --gas=300000000000000
```
> edit xx menjadi nama wallet kamu

# Ping

```
near call xx.factory.shardnet.near ping '{}' --accountId xx.shardnet.near --gas=300000000000000
```

# Total Balance

```
near view xx.factory.shardnet.near get_account_total_balance '{"account_id": "xx.shardnet.near"}'
```

# Staked Balance

```
near view xx.factory.shardnet.near get_account_staked_balance '{"account_id": "xx.shardnet.near"}'
```

# Unstaked Balance

```
near view xx.factory.shardnet.near get_account_unstaked_balance '{"account_id": "xx.shardnet.near"}'
```

# Available for Withdrawal

```
near view xx.factory.shardnet.near is_account_unstaked_balance_available '{"account_id": "xx.shardnet.near"}'
```

# Pause / Resume Staking

```
near call xx.factory.shardnet.near pause_staking '{}' --accountId xx.shardnet.near
```

```
near call xx.factory.shardnet.near resume_staking '{}' --accountId xx.shardnet.near
```

> edit xx menjadi nama wallet kamu

### Challenge 004

# Membuat Monitoring Node Status

```
sudo apt install curl jq
```

```
curl -s http://127.0.0.1:3030/status | jq .version
```

# Cek Delegators dan Stake
```
near view xx.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId xx.shardnet.near
```

> edit xx menjadi nama wallet kamu

### Cek Block Produced

```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("xx.factory.shardnet.near"))' | jq .reason
```

### Cek Reason Validator Kicked

```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("xx.factory.shardnet.near"))' | jq .reason
```

### Cek Sinkronisasi
```
curl -s http://127.0.0.1:3030/status | jq .sync_info
```

> Pastikan status syncing adalah false

# Challenge 005 (OPTIONAL)

> Kirim formulir dengan artikel Anda.

> Kriteria penerimaan:
- Dokumentasikan proses dan publikasikan (GitHub atau Medium)
- Sertakan petunjuk langkah demi langkah untuk memasang validator simpul menggunakan petunjuk Perang Pasak (Tantangan 001, 002, 003, dan 004).
- Sertakan tangkapan layar dan deskripsi terperinci tentang prosesnya.
- Sertakan harga untuk menjalankan validator.
- Artikel dapat dilakukan dalam bahasa apa pun. Sebuah review dapat dilakukan untuk penerimaan.

https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform

# Challenge 006

### Membuat Ping Otomatis Setiap 5 Menit
```
mkdir $HOME/nearcore/logs
```

```
nano $HOME/nearcore/scripts/ping.sh
```

> paste kode dibawah ini ,XX dirubah menjadi nama wallet kamu 

#!/bin/sh
# Ping call to renew Proposal added to crontab
```
export NEAR_ENV=shardnet
export LOGS=$HOME/nearcore/logs
export POOLID="xx"
export ACCOUNTID="xx"

echo "---" >> $LOGS/all.log
date >> $LOGS/all.log
near call $POOLID.factory.shardnet.near ping '{}' --accountId $ACCOUNTID.shardnet.near --gas=300000000000000 >> $LOGS/all.log
near proposals | grep $POOLID >> $LOGS/all.log
near validators current | grep $POOLID >> $LOGS/all.log
near validators next | grep $POOLID >> $LOGS/all.log
```
### crontab
```
crontab -e
```
> tekan nomor 1 enter

### masukan kode dibawah ini di paling bawah

```
0 */2 * * * sh /home/<USER_ID>/scripts/ping.sh
```

> CTRL + O enter CTRL + X enter

### Cek Log

```
cat $HOME/nearcore/logs/all.log
```

> kamu bisa melihat hasilnya di explorer dan akan terlihat seperti ini

![image](https://user-images.githubusercontent.com/109859686/181278477-ccfce3be-70db-4374-9ec5-90d83a35c107.png)

> submit form 

- Challenge URL: The link to the explorer of your staking pool.
- Challenge image: Screenshots of PING transaction being done periodically.

https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform

# Challenge 007 (optional)

> Kriteria penerimaan

- Mohon berikan wawasan data selain yang bisa langsung kami dapatkan dari dompet NEAR explorer / NEAR.
- Adapun hasilnya, dasbor data yang divisualisasikan lebih disukai, juga spreadsheet dengan data yang terstruktur dengan baik juga dapat diterima.
- Apa yang dapat kami pelajari dari data sebenarnya penting, jadi harap berikan juga ringkasan tentang bagaimana Anda menginterpretasikan data dan bagaimana kami dapat meningkatkan sistem NEAR berdasarkan pekerjaan Anda.
- Analisis dengan data mainnet NEAR sangat disarankan. Anda juga dapat menganalisis data dari testnet atau shardnet, tetapi mainnet memiliki transaksi yang lebih nyata dan kompleks.

# Challenge 008 ( OPTIONAL)

### Install cargo and Rust in case you don't have it. This command is for Linux or MacOS.

```
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh

source $HOME/.cargo/env
```

```
rustup target add wasm32-unknown-unknown
```

### Clone the project found https://github.com/zavodil/near-staking-pool-owner

```
git clone https://github.com/zavodil/near-staking-pool-owner
```

### Kompilasi kontrak pintar

```
cd near-staking-pool-owner/contract
rustup target add wasm32-unknown-unknown
cargo build --target wasm32-unknown-unknown --release
```

### Terapkan kontrak pintar di akun pemilik Anda. Sesuaikan jalur ke file .wasm jika diperlukan.

```
NEAR_ENV=shardnet near deploy <OWNER_ID>.shardnet.near --wasmFile target/wasm32-unknown-unknown/release/contract.wasm
```

### Inisialisasi akun pengambilan kontrak pintar untuk membagi pendapatan.


CONTRACT_ID=<OWNER_ID>.shardnet.near

```
# Change numerator and denomitor to adjust the % for split.
near call $CONTRACT_ID new '{"staking_pool_account_id": "<STAKINGPOOL_ID>.factory.shardnet.near", "owner_id":"<OWNER_ID>.shardnet.near", "reward_receivers": [["<SPLITED_ACCOUNT_ID_1>.shardnet.near", {"numerator": 3, "denominator":10}], ["<SPLITED_ACCOUNT_ID_2>.shardnet.near", {"numerator": 70, "denominator":100}]]}' --accountId $CONTRACT_ID
```

### Tunggu hingga Anda mulai menerima hadiah di kumpulan staking node Anda

Lakukan penarikan hadiah.

```
CONTRACT_ID=<OWNER_ID>.shardnet.near
NEAR_ENV=shardnet near call $CONTRACT_ID withdraw '{}' --accountId $CONTRACT_ID --gas 200000000000000
```

### Challenge submission

- Challenge URL: The link to the explorer of your withdraw transaction.
- Challenge image: Screenshots of tokens distribution transaction.

> contoh tangkapan layar seperti ini

![image](https://user-images.githubusercontent.com/109859686/181284933-1556b179-697c-4051-9290-c0723e9ed49e.png)

- isi form https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform

- 
- 
- 
- 

# TERIMAKASIH TELAH MENGIKUTI TUTORIAL SAYA ,SEMOGA BERHASIL !!!
