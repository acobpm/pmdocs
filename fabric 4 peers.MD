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
./generateArtifacts.sh ch1
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

8. test fabric

in VM Peer1 , login to docker peer

```
docker exec -it cli bash
```

8.1 set global variable
```
CHANNEL_NAME=ch1
COUNTER=1
MAX_RETRY=5
ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

below are optional 

-----------
```
if use peer0.org1 , user below
```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer0.org1.example.com:7051

'''
if use peer1.org1 , user below
```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer1.org1.example.com:7051

'''
if use peer0.org2 , user below
```
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ADDRESS=peer0.org2.example.com:7051

'''
if use peer1.org2 , user below
```
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ADDRESS=peer1.org2.example.com:7051

'''
----------------

8.2 test orderer connection

```
CORE_PEER_LOCALMSPID="OrdererMSP"
CORE_PEER_TLS_ROOTCERT_FILE=$ORDERER_CA
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp

peer channel fetch 0 0_block.pb -o orderer.example.com:7050 -c "testchainid" --tls --cafile $ORDERER_CA 

```

8.3 create channel (channel was set as ch1 on 8.1)
```
peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls --cafile $ORDERER_CA
```

8.4 join channel


```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer0.org1.example.com:7051

peer channel join -b $CHANNEL_NAME.block
```


```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer1.org1.example.com:7051

peer channel join -b $CHANNEL_NAME.block
```

```
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ADDRESS=peer0.org2.example.com:7051

peer channel join -b $CHANNEL_NAME.block
```

```
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ADDRESS=peer1.org2.example.com:7051

peer channel join -b $CHANNEL_NAME.block
```

8.5 set anchor peer for org1 & org2

Org1
```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer0.org1.example.com:7051

peer channel update -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/${CORE_PEER_LOCALMSPID}anchors.tx --tls --cafile $ORDERER_CA
```

Org2

```
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

CORE_PEER_ADDRESS=peer0.org2.example.com:7051

peer channel update -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/${CORE_PEER_LOCALMSPID}anchors.tx --tls --cafile $ORDERER_CA
```

8.6 install chaincode on peer0/org1 & peer0/org2

Org1
```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer0.org1.example.com:7051

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 
```

Org2

```
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

CORE_PEER_ADDRESS=peer0.org2.example.com:7051

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 
```

8.7 initial chaincode on peer0/org2

```
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ADDRESS=peer0.org2.example.com:7051

peer chaincode instantiate -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P "OR	('Org1MSP.peer','Org2MSP.peer')"
```

8.8 query chaincode on peer0/org1 and peer0/org2

peer0/Org1
```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer0.org1.example.com:7051

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```
 
**open VM** peer0/org2 , 
set global variable

```   
CHANNEL_NAME=ch1
COUNTER=1
MAX_RETRY=5
ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

8.9 invoke chaincode

peer0/Org1
```
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ADDRESS=peer0.org1.example.com:7051

peer chaincode invoke -o orderer.example.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'
```

**open VM** peer0/org2 , invoke again
set global variable

```
CHANNEL_NAME=ch1
COUNTER=1
MAX_RETRY=5
ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

peer chaincode invoke -o orderer.example.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'
```
query chaincode or peer0/org1 and peer0/org2, it should show value '80' as result

8.10 install chaincode on peer1/org1 and peer1/org2

**open VM** peer1/org1 , 

login docker
```
docker exec -it cli bash
```

```
CHANNEL_NAME=ch1
COUNTER=1
MAX_RETRY=5
ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 
```

do query
```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

**open VM** peer1/org2 , 
login docker
```
docker exec -it cli bash
```

```
CHANNEL_NAME=ch1
COUNTER=1
MAX_RETRY=5
ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 
```

do query
```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```




