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

Running with Docker
The pathfinder node can be run in the provided Docker image. Using the Docker image is the easiest way to start pathfinder. If for any reason you're interested in how to set up all the dependencies yourself please check the Installation from source guide.

The following assumes you have Docker installed and ready to go. (In case of Ubuntu installing docker is as easy as running sudo snap install docker.)

The example below uses $HOME/pathfinder as the data directory where persistent files used by pathfinder will be stored. It is easiest to create the volume directory as the user who is running the docker command. If the directory gets created by docker upon startup, it might be unusable for creating files.

The following commands start the node in the background, also making sure that it starts automatically after reboot:

# Ensure the directory has been created before invoking docker
mkdir -p $HOME/pathfinder
# Start the pathfinder container instance running in the background
sudo docker run \
  --name pathfinder \
  --restart unless-stopped \
  --detach \
  -p 9545:9545 \
  --user "$(id -u):$(id -g)" \
  -e RUST_LOG=info \
  -e PATHFINDER_ETHEREUM_API_URL="https://goerli.infura.io/v3/<project-id>" \
  -v $HOME/pathfinder:/usr/share/pathfinder/data \
  eqlabs/pathfinder
To check logs you can use:

sudo docker logs -f pathfinder
The node can be stopped using

sudo docker stop pathfinder
Updating the Docker image
When pathfinder detects there has been a new release, it will log a message similar to:

WARN New pathfinder release available! Please consider updating your node! release=0.4.5
You can try pulling the latest docker image to update it:

sudo docker pull eqlabs/pathfinder
After pulling the updated image you should stop and remove the pathfinder container then re-create it with the exact same command that was used above to start the node:

# This stops the running instance
sudo docker stop pathfinder
# This removes the current instance (using the old version of pathfinder)
sudo docker rm pathfinder
# This command re-creates the container instance with the latest version
sudo docker run \
  --name pathfinder \
  --restart unless-stopped \
  --detach \
  -p 9545:9545 \
  --user "$(id -u):$(id -g)" \
  -e RUST_LOG=info \
  -e PATHFINDER_ETHEREUM_API_URL="https://eth1.lava.build/lava-referer-dd946195-d07d-46cc-95f8-6759c1013778" \
  -v $HOME/pathfinder:/usr/share/pathfinder/data \
  eqlabs/pathfinder
Available images
Our images are updated on every pathfinder release. This means that the :latest docker image does not track our main branch here, but instead matches the latest pathfinder release.

Docker compose
You can also use docker-compose if you prefer that to just using Docker.

Create the folder pathfinder where your docker-compose.yaml is

mkdir -p pathfinder

# replace the value by of PATHFINDER_ETHEREUM_API_URL by the HTTP(s) URL pointing to your Ethereum node's endpoint
cp example.pathfinder-var.env pathfinder-var.env

docker-compose up -d
To check if it's running well use docker-compose logs -f.
