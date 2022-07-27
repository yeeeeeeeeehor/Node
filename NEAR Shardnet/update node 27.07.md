# STOP SERVICE NEARD
sudo systemctl stop neard

# REMOVE OLD DB
rm ~/.near/data/*

# BUILD NEW COMMIT
export NEAR_ENV=shardnet
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
cd ~/nearcore
git fetch
git checkout 0d7f272afabc00f4a076b1c89a70ffc62466efe9
cargo build -p neard --release --features shardnet

# DOWNLOAD NEW GENESIS AND CONFIG
cd ~/.near
rm config.json
rm genesis.json
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json


# START SERVICE NEARD
sudo systemctl start neard

# CHECK STATUS, PEERS AND SYNCING
sudo journalctl -n 1000 -f -u neard | ccze -A

# DON'T FORGET THE PING SCRIPT
