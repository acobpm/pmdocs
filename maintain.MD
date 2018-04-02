### maintain
## update existing BNA

# generate new BNA file 
1. update package.json , version ++1 , for example
```
"version": "0.0.2",
```
2. generate new bna file , and overwrite the existing
```
composer archive create -t dir -a promise03.bna -n .
```
3. install new version to network ( when network is running)
```
composer network install -c PeerAdmin@hlfv1 -a promise03.bna
```
4. upgrade network 
```
composer network upgrade -n promise03 -V 0.0.2 -c PeerAdmin@hlfv1
```