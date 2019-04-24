1. ```sudo apt install openjdk-8-jdk```

2. remove old symlik to /usr/bin/java -> old java location

```sudo rm -rf /usr/bin/java```

3. create new symlink to new java location

```sudo ln -s /usr/lib/jvm/java-8-openjdk-amd64/ /usr/bin/java```

4. add /usr/bin/java to $PATH in ~/.zshrc, it would have been removed when you deleted /usr/bin/java

5. ```source ~/.zshrc```

6. ```sudo update-alternatives --config java``` select version
