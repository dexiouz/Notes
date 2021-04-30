# Error

Got this after installing brew and running ```brew --version```
```sh
Homebrew 3.1.3
Homebrew/homebrew-core (no Git Repository)
```

Fixed with 
```sh
 git -C $(brew --repo homebrew/core) checkout master
```