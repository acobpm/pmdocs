1. complete all steps in file 'fabric 4 peers.MD'

2. config CA server, 

    1. find CA SK file from folder crypto-config/peerOrganizations/org1.example.com/ca
    2. copy the sk file name to below and update to docker compose file for peer0 org1
    ```
    ca0:
    image: hyperledger/fabric-ca
    environment:
      - FABRIC_CA_HOME=/etc/hyperledger/fabric-ca-server
      - FABRIC_CA_SERVER_CA_NAME=ca-org1
      - FABRIC_CA_SERVER_TLS_ENABLED=true
      - FABRIC_CA_SERVER_TLS_CERTFILE=/etc/hyperledger/fabric-ca-server-config/ca.org1.example.com-cert.pem
      - FABRIC_CA_SERVER_TLS_KEYFILE=/etc/hyperledger/fabric-ca-server-config/a61d1cc72e2875c0259bde61980a891a7f6a5cbb85680ca4f5baeadc91024c79_sk
    ports:
      - "7054:7054"
    command: sh -c 'fabric-ca-server start --ca.certfile /etc/hyperledger/fabric-ca-server-config/ca.org1.example.com-cert.pem --ca.keyfile /etc/hyperledger/fabric-ca-server-config/a61d1cc72e2875c0259bde61980a891a7f6a5cbb85680ca4f5baeadc91024c79_sk -b admin:adminpw -d'
    volumes:
      - ./crypto-config/peerOrganizations/org1.example.com/ca/:/etc/hyperledger/fabric-ca-server-config
    container_name: ca_peerOrg1
    ```

    3. do the same for ca org2 , keep port as 7054 (instead of 8054)

    ```
    ca1:
    image: hyperledger/fabric-ca:$IMAGE_TAG
    environment:
      - FABRIC_CA_HOME=/etc/hyperledger/fabric-ca-server
      - FABRIC_CA_SERVER_CA_NAME=ca-org2
      - FABRIC_CA_SERVER_TLS_ENABLED=true
      - FABRIC_CA_SERVER_TLS_CERTFILE=/etc/hyperledger/fabric-ca-server-config/ca.org2.example.com-cert.pem
      - FABRIC_CA_SERVER_TLS_KEYFILE=/etc/hyperledger/fabric-ca-server-config/3b63c707a37f20b0db460acbabf23db4a396c7562cae1cf8cd2885e797e1a12c_sk
    ports:
      - "7054:7054"
    command: sh -c 'fabric-ca-server start --ca.certfile /etc/hyperledger/fabric-ca-server-config/ca.org2.example.com-cert.pem --ca.keyfile /etc/hyperledger/fabric-ca-server-config/3b63c707a37f20b0db460acbabf23db4a396c7562cae1cf8cd2885e797e1a12c_sk -b admin:adminpw -d'
    volumes:
      - ./crypto-config/peerOrganizations/org2.example.com/ca/:/etc/hyperledger/fabric-ca-server-config
    container_name: ca_peerOrg2
    ```

    4. update these 2 files to VM peer1 and peer3

    ```
    scp  ~/project/fabric4/docker/docker-compose-peer1.yaml fabric@192.168.1.211:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/docker-compose-peer.yaml

    scp  ~/project/fabric4/docker/docker-compose-peer3.yaml fabric@192.168.1.213:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/docker-compose-peer.yaml

    ```

3. start VM 
    1. for peer 1 and peer 3, in folder ~/go/src/github.com/hyperledger/fabric/examples/e2e_cli/ , use 
    ```
        docker-compose -f docker-compose-peer.yaml up -d --no-recreate
    ```
    2. for peer2 and peer 4 and order, use
    ```
    docker start $(docker ps -aq)
    ```
    