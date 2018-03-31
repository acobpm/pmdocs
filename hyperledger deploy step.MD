
# Deploy hyperledger
## steps
1. add business network from runtime  
```
composer runtime install -c PeerAdmin@hlfv1 -n promise05
```
2. start network
```
composer network start -c PeerAdmin@hlfv1 -A admin -S adminpw -a promise05@0.0.1.bna -f networkadmin05.card
```
3. import card
```
composer card import -f networkadmin05.card
```

4. ping the network. (*only after ping , the network will display in playground*)

```
composer network ping -c admin@promise05
```




