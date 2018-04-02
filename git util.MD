### Git util 
## Git options 

1. setup user name and password:
```
git config --global user.email "woophily@outlook.com"
git config --global user.name "philywu"
```

2. check remote repository
```
git remote -v
```

3. push to remote 
```
git push -u origin master
```
4. pull from remote
```
git pull
```

5. merge branch local
```
git checkout master // the branch will be merged to
git merge branch_name // the branch_name is the branch which will read from
```