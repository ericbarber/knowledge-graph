# Pass

## Docs
 site: [passwordstore.org](https://www.passwordstore.org/)

## Work Flow
### Commands List:
#### List the pass directory
```bash
pass
```

#### Generate new system key
```bash
gpg --gen-key
```

#### List the details of pass  
```bash
gpg -K
```

#### Edit existing system key 
```bash
gpg --gen-key
trust
expire
save
```

#### Add a new secret key 
```bash
pass insert <secret_name>
```
* Note: For secret names, do not use PII, this is visiable within directory

#### Edit existing secret key 
```bash
pass edit <secret_name>
```

#### Find existing secret key 
```bash
pass find <secret_name>
```

#### Find existing secret key 
```bash
pass find <secret_name>
```

#### Add a Github remote repo 
```bash
pass git remote add <remote_name> git@github.com-<ssh_profile>:repo/path.git 
```
