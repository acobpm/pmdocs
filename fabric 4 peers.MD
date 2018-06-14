#Install 4 peers hyperledger fabric in multiple servers

1. install go

1.1 download
```
sudo apt-get update
sudo apt-get -y upgrade
sudo curl -O https://storage.googleapis.com/golang/go1.9.1.linux-amd64.tar.gz
```

1.2 unzip and move

```
sudo tar -xvf go1.9.1.linux-amd64.tar.gz
sudo mv go /usr/local
```

1.3 set enviroment 

```
sudo nano ~/.profile

```
in the file add below lines
```
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go 

```

make the setting effect 
```
source ~/.profile
```
1.4 create folder go under home
```
mkdir ~/go
```

2. install fabric source code

get source code 

in folder $HOME/go

```
go get github.com/hyperledger/fabric
```

3. run single server version (optional)

```
cd ~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/
./network_setup.sh up
```

if seeing below message

===================== All GOOD, End-2-End execution completed ===================== 


 _____   _   _   ____            _____   ____    _____ 
| ____| | \ | | |  _ \          | ____| |___ \  | ____|
|  _|   |  \| | | | | |  _____  |  _|     __) | |  _|  
| |___  | |\  | | |_| | |_____| | |___   / __/  | |___ 
|_____| |_| \_| |____/          |_____| |_____| |_____|


you can shutdown and delete all setting by using 
```
./network_setup.sh down
```

4. install fabric

*. generate crypto and aritifacts

in folder
```
./generateArtifacts.sh mychannel
```

5. copy crypto and channel_artifacts to all other peers

5.1 copy to laptop

```
scp -r fabric@192.168.1.211:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/crypto-config/ ~/project/fabric4/share/crypto-config/
scp -r fabric@192.168.1.211:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/channel-artifacts/ ~/project/fabric4/share/
```

5.2 copy to other peers

```
scp -r  ~/project/fabric4/share/* fabric@192.168.1.212:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

scp -r  ~/project/fabric4/share/* fabric@192.168.1.213:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

scp -r  ~/project/fabric4/share/* fabric@192.168.1.214:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

scp -r  ~/project/fabric4/share/* fabric@192.168.1.220:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

```

6. modify and copy docker-compose.yaml file

base file
```
scp -r  ~/project/fabric4/docker/base fabric@192.168.1.211:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

scp -r  ~/project/fabric4/docker/base fabric@192.168.1.212:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

scp -r  ~/project/fabric4/docker/base fabric@192.168.1.213:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

scp -r  ~/project/fabric4/docker/base fabric@192.168.1.214:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

scp -r  ~/project/fabric4/docker/base fabric@192.168.1.220:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/
```

compose file ( change all ip address to releveant)

```
scp  ~/project/fabric4/docker/docker-compose-peer1.yaml fabric@192.168.1.211:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/docker-compose-peer.yaml

scp  ~/project/fabric4/docker/docker-compose-peer2.yaml fabric@192.168.1.212:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/docker-compose-peer.yaml

scp  ~/project/fabric4/docker/docker-compose-peer3.yaml fabric@192.168.1.213:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/docker-compose-peer.yaml

scp  ~/project/fabric4/docker/docker-compose-peer4.yaml fabric@192.168.1.214:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/docker-compose-peer.yaml

scp  ~/project/fabric4/docker/docker-compose-orderer.yaml fabric@192.168.1.220:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

```

7. start fabric 

in VM orderer , 
folder : ~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

```
docker-compose -f docker-compose-orderer.yaml up -d
```

in VM peers , 
folder : ~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/

```
docker-compose -f docker-compose-peer.yaml up -d
```
