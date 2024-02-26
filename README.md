# starknet-pathfinder
1. Register Alchemy
DISINI
2. Buat App 
a. Pilih Ethereum Mainnet

b. Masuk ke App
c. Pilih View Key
d. Salin HTTPS , simpan di Notepad

Ethereum Mainnet jangan sampai salah
Lanjut ke VPS
3. Install Docker
sudo apt update -y && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt install docker-ce
4. Install Alat
sudo apt-get install -y pkg-config curl git build-essential libssl-dev screen
5. Download Bahan
git clone --branch v0.4.0 https://github.com/eqlabs/pathfinder.git
6. Mulai
mkdir -p $HOME/pathfinder
docker run -d \
  --rm \
  -p 9545:9545 \
  --user "$(id -u):$(id -g)" \
  -e RUST_LOG=info \
  --name starknet \
  -e PATHFINDER_ETHEREUM_API_URL="XXXXXXXXXXXXX" \
  -v $HOME/pathfinder:/usr/share/pathfinder/data \
  eqlabs/pathfinder
XXXXXXXXXXXX  diganti dengan link HTTPS mu dari alchemy

Contoh:
mkdir -p $HOME/pathfinder docker run -d
--rm
-p 9545:9545
--user "$(id -u):$(id -g)"
-e RUST_LOG=info
--name starknet
-e PATHFINDER_ETHEREUM_API_URL="https://eth1.lava.build/lava-referer-dd946195-d07d-46cc-95f8-6759c1013778"
-v $HOME/pathfinder:/usr/share/pathfinder/data
eqlabs/pathfinder

7. Cek logs
docker logs -f starknet
OKAY!

Nunggu sync lama, cek blocks terkini
DISINI
8. Screenshot
a. Screenshot Dashboard Alchemy
harusnya udah ada angka2 disana
