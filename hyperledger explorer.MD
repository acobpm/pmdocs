# config the hyperledger explorer
### not stable version yet, might be changed in future when the version is update

1. Clone this repository to get the latest using the following command.
```
git clone https://github.com/hyperledger/blockchain-explorer.git
```

2. install postgres sql 

* MacOS

    install via brew 

```
brew install postgresql

ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
```

edit ~/.bash_profile , add 2 lines below in fiel
```
alias pg-start="launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist"
alias pg-stop="launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist"
```

* Ubuntu
```
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
```

3. Connect to PostgreSQL database.

* MacOS
```
psql
```
* Ubuntu
```
sudo -u postgres psql
```
Run create database script. it will show some error , just ignore them , only check the database and tables are created
```
\i app/persistance/postgreSQL/db/explorerpg.sql
\i app/persistance/postgreSQL/db/updatepg.sql
Run db status commands.

\l view created fabricexplorer database
\d view created tables
```

4. fabric network running

    just make sure your fabric network is running

5. change hyperledger-explorer config file

* change file blockchain-explorer/app/platform/fabric/config.json

### attention:

1. user the absolute directory for crypto file , cannot user ~/ , or link folder.
2. add orderer portion
3. use correct channel name

example 

```
{
	"network-config": {
		"org1": {
			"name": "peerOrg1",
			"mspid": "Org1MSP",
			"peer1": {
				"requests": "grpcs://192.168.1.211:7051",
				"events": "grpcs://192.168.1.211:7053",
				"server-hostname": "peer0.org1.example.com",
				"tls_cacerts": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt"
			},
			"peer2": {
				"requests": "grpcs://192.168.1.212:7051",
				"events": "grpcs://192.168.1.212:7053",
				"server-hostname": "peer1.org1.example.com",
				"tls_cacerts": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org1.example.com/peers/peer1.org1.example.com/tls/ca.crt"
			},
			"admin": {
				"key": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore",
				"cert": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/signcerts"
			}
		},
		"org2": {
			"name": "peerOrg2",
			"mspid": "Org2MSP",
			"peer1": {
				"requests": "grpcs://192.168.1.213:7051",
				"events": "grpcs://192.168.1.212:7053",
				"server-hostname": "peer0.org2.example.com",
				"tls_cacerts": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt"
			},
			"peer2": {
				"requests": "grpcs://192.168.1.214:7051",
				"events": "grpcs://192.168.1.214:7053",
				"server-hostname": "peer1.org2.example.com",
				"tls_cacerts": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org2.example.com/peers/peer1.org2.example.com/tls/ca.crt"
			},
			"admin": {
				"key": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp/keystore",
				"cert": "/Users/phily/project/fabric4/share/crypto-config/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp/signcerts"
			}
		}
	},
	"host": "localhost",
	"port": "8080",
	"channel": "ch1",
	"orderers":[
				{
				"mspid": "OrdererMSP",
				"server-hostname":"orderer.example.com",
				"requests":"grpcs://192.168.1.220:7050",
				"tls_cacerts":"/Users/phily/project/fabric4/share/crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tls/ca.crt"
				}
				],
	"keyValueStore": "/tmp/fabric-client-kvs",
	"eventWaitTime": "30000",
	"users":[
		{
		   "username":"admin",
		   "secret":"adminpw"
		}
	 ],
	"pg": {
		"host": "127.0.0.1",
		"port": "5432",
		"database": "fabricexplorer",
		"username": "hppoc",
		"passwd": "password"
	},
	"license": "Apache-2.0"
}

```

6. build hyperleger explorer

* in folder blockchain-explorer, run 
```
npm install
```

* in folder blockchain-explorer/client,this is to generate web UI. run
```
npm install
npm run build
```

* go to folder blockchain-explorer, run 
```
./start.sh
```

* check logs in file 
logs/console/console.log-* file

7. see in web page
```
http://localhost:8080
```