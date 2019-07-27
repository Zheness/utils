# Linux Commands

Here are some useful Linux commands.

_(Currently all coming from the video of [Engineer Man](https://www.youtube.com/watch?v=Zuwa8zlfXSY))_

```bash
# Redo the last command, but as root
sudo !!

# Open an editor to run a command
ctrl+x+e

# Create a super fast ram disk
mkdir -p /mnt/ram
mount -t tmpfs tmpfs /mnt/ram -o size=8192M

# Avoid command to be saved in history. (Put a space at the beginning)
 ls -l
 
# Fix a long command
fc

# Quickly create subfolders
mkdir -p folder/{sub1,sub2}/{sub1,sub2,sub3}
mkdir -p folder/{1..20}/{1..5}

# Exit a terminal but keep the proccesses running
disown -a && exit

# Create a folder and go inside
mkdir -p folder/my/new/very/long/path
cd !$
# or
mkdir -p folder/my/new/very/long/path && cd $_
```
