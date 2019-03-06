#Install 4 peers hyperledger fabric in multiple servers

1. install go

1.1 download
```
sudo apt-get update
sudo apt-get -y upgrade
sudo curl -O https://storage.googleapis.com/golang/go1.11.5.linux-amd64.tar.gz
```

1.2 unzip and move

```
sudo tar -xvf go1.11.5.linux-amd64.tar.gz
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
mkdir -p $GOPATH/src/github.com/hyperledger/
cd $GOPATH/src/github.com/hyperledger/
git clone https://github.com/hyperledger/fabric-samples.git
```

3. in hyperledger folder, download all images by run
```
curl -sSL http://bit.ly/2ysbOFE | bash -s 1.4.0
```


4. install fabric

*. generate crypto and aritifacts

in laptop folder ~/hyperledger/fabric-samples/mi-network2, run script as below (note: generate.sh has been modified for 2 orgs used)
```
./generate.sh
```

*. copy crypto and channel_artifacts to all other peers


*.1 copy to other peers

```
scp -r  ~/hyperledger/fabric-samples/mi-network2/config fabric@192.168.1.230:~/go/src/github.com/hyperledger/fabric-samples/mi-orderer
scp -r  ~/hyperledger/fabric-samples/mi-network2/crypto-config fabric@192.168.1.230:~/go/src/github.com/hyperledger/fabric-samples/mi-orderer

scp -r  ~/hyperledger/fabric-samples/mi-network2/config fabric@192.168.1.231:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
scp -r  ~/hyperledger/fabric-samples/mi-network2/crypto-config fabric@192.168.1.231:~/go/src/github.com/hyperledger/fabric-samples/mi-peer

scp -r  ~/hyperledger/fabric-samples/mi-network2/config fabric@192.168.1.232:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
scp -r  ~/hyperledger/fabric-samples/mi-network2/crypto-config fabric@192.168.1.232:~/go/src/github.com/hyperledger/fabric-samples/mi-peer


```
*. copy docker compose file to peers

for orderer
```
scp -r  ~/hyperledger/fabric-samples/mi-network2/base fabric@192.168.1.230:~/go/src/github.com/hyperledger/fabric-samples/mi-orderer
scp  ~/hyperledger/fabric-samples/mi-network2/docker-compose-orderer.yaml fabric@192.168.1.230:~/go/src/github.com/hyperledger/fabric-samples/mi-orderer

```

for peer1

modify docker-compose-peer10.yaml file , put ca sk value with the value under ./crypto-config/peerOrganizations/org1.example.com/ca/

```
      - FABRIC_CA_SERVER_TLS_KEYFILE=/etc/hyperledger/fabric-ca-server-config/4fd3eadad30feb2c0fb7e52e17eb7b28643e702fb1159c00803f927266948d75_sk
      command: sh -c 'fabric-ca-server start --ca.certfile /etc/hyperledger/fabric-ca-server-config/ca.org1.example.com-cert.pem --ca.keyfile /etc/hyperledger/fabric-ca-server-config/4fd3eadad30feb2c0fb7e52e17eb7b28643e702fb1159c00803f927266948d75_sk -b admin:adminpw -d'


```

copy file to peer node
```
scp -r  ~/hyperledger/fabric-samples/mi-network2/base fabric@192.168.1.231:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
scp  ~/hyperledger/fabric-samples/mi-network2/docker-compose-peer10.yaml fabric@192.168.1.231:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
scp  ~/hyperledger/fabric-samples/mi-network2/.env fabric@192.168.1.231:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
```
for peer2

modify docker-compose-peer20.yaml file , put ca sk value with the value under ./crypto-config/peerOrganizations/org1.example.com/ca/

```
      - FABRIC_CA_SERVER_TLS_KEYFILE=/etc/hyperledger/fabric-ca-server-config/97014488d8dc62f9136f2aff1bfc0bc743a4e800e1f14b58952101b7aaa93611_sk
      command: sh -c 'fabric-ca-server start --ca.certfile /etc/hyperledger/fabric-ca-server-config/ca.org1.example.com-cert.pem --ca.keyfile /etc/hyperledger/fabric-ca-server-config/97014488d8dc62f9136f2aff1bfc0bc743a4e800e1f14b58952101b7aaa93611_sk -b admin:adminpw -d'


```

copy file to peer node
```
scp -r  ~/hyperledger/fabric-samples/mi-network2/base fabric@192.168.1.232:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
scp  ~/hyperledger/fabric-samples/mi-network2/docker-compose-peer20.yaml fabric@192.168.1.232:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
scp  ~/hyperledger/fabric-samples/mi-network2/.env fabric@192.168.1.232:~/go/src/github.com/hyperledger/fabric-samples/mi-peer
```



*. run docker compose

orderer
in orderer node , folder ~/go/src/github.com/hyperledger/fabric-samples/mi-orderer , run command

```
docker-compose -f docker-compose-orderer.yaml up -d
```

peer1
in peer1 node , folder ~/go/src/github.com/hyperledger/fabric-samples/mi-peer , run command

```
docker-compose -f docker-compose-peer10.yaml up -d
```

peer2
in peer2 node , folder ~/go/src/github.com/hyperledger/fabric-samples/mi-peer , run command

```
docker-compose -f docker-compose-peer20.yaml up -d
```


8. test fabric

in VM Peer1 , login to docker peer

```
docker exec -it cli bash
```

8.1 set global variable
```
CHANNEL_NAME=michannel
COUNTER=1
MAX_RETRY=5
ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem


8.2 test orderer connection

```
CORE_PEER_LOCALMSPID="OrdererMSP"
CORE_PEER_TLS_ROOTCERT_FILE=$ORDERER_CA
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp

peer channel fetch 0 0_block.pb -o orderer.example.com:7050 -c "testchainid" --tls --cafile $ORDERER_CA

```

8.3 create channel
```

CHANNEL_NAME=michannel
COUNTER=1
MAX_RETRY=5
ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls --cafile $ORDERER_CA
```


//peer channel fetch 0 0_block.pb -o orderer.example.com:7050 -c "michannel" --tls --cafile $ORDERER_CA

8.4 join channel

in peer1 cli
```


peer channel join -b $CHANNEL_NAME.block
```
8.5 copy channel block to peer2
in peer1 cli, make sure the file can be read from outside of docker
```
cp michannel.block ./channel-artifacts/
```

do action on client(macbook) copy block from peer1 to peer2
```
scp fabric@192.168.1.231:~/go/src/github.com/hyperledger/fabric-samples/mi-peer/config/michannel.block ./

scp ./michannel.block fabric@192.168.1.232:~/go/src/github.com/hyperledger/fabric-samples/mi-peer/config/

```

in peer2 ,goto cli docker and run
```
docker exec -it cli bash
cp ./channel-artifacts/michannel.block ./

```

8.4.2 peer2 join channel

```

peer channel join -b $CHANNEL_NAME.block
```


8.5 set anchor peer for org1 & org2

Peer1 in docker
```
peer channel update -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/${CORE_PEER_LOCALMSPID}anchors.tx --tls --cafile $ORDERER_CA
```

Org2

```
peer channel update -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/${CORE_PEER_LOCALMSPID}anchors.tx --tls --cafile $ORDERER_CA
```

8.6 install chaincode on peer0/org1 & peer0/org2

scp -r  ~/hyperledger/fabric-samples/mi-network2/contract fabric@192.168.1.231:~/go/src/github.com/hyperledger/fabric-samples/mi-peer

scp -r  ~/hyperledger/fabric-samples/mi-network2/contract fabric@192.168.1.232:~/go/src/github.com/hyperledger/fabric-samples/mi-peer




Org1
```
peer chaincode install -n papercontract -v 0 -p /opt/gopath/src/github.com/contract -l node

```

in

Org2

```
peer chaincode install -n papercontract -v 0 -p /opt/gopath/src/github.com/contract -l node

```

8.7 initial chaincode on peer0/org2

```
peer chaincode instantiate -o orderer.example.com:7050 -n papercontract -v 0 -l node -c '{"Args":["org.papernet.commercialpaper:instantiate"]}' -C michannel -P "OR ('Org1MSP.member','Org2MSP.member')" --tls --cafile $ORDERER_CA
```

invoke chaincode to issue a paper
```
peer chaincode invoke -o orderer.example.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n papercontract -c '{"Args":["issue", "MagnetoCorp", "00001", "2020-05-31", "2020-11-30", "5000000"]}'

peer chaincode invoke -o orderer.example.com:7050  --tls --cafile $ORDERER_CA -C $CHANNEL_NAME -n papercontract -c '{"Args":["issue", "MagnetoCorp", "00002", "2021-05-31", "2021-11-30", "8000000"]}'

```

query chaincode from org2

peer chaincode query -C $CHANNEL_NAME -n papercontract -c '{"Args":["query","a"]}'


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
