# Install K3D and K3S v1.17.14 on Ubuntu 18

### Install golang
```
$ sudo snap install go --classic
```
### Clone K3D source code
```
$ git clone github.com/rancher/k3d/v3@main
$ cd ./k3d
```
### Install Dependencies
These are some dependencies required for building source code. Verify if these are already installed on your System.
```
$ sudo apt install gcc
$ sudo apt install gox
```
### Edit version.go file
Open the file.
```
$ vim version/version.go
```
Replace *v1.17.14-k3s2* in k3sVersion variable at line 38 and save changes.
```
37 // K3sVersion should contain the latest version tag of k3s (hardcoded at build time)
38 //var K3sVersion = "v1.19.4-k3s1"
39 var K3sVersion = "v1.17.14-k3s2"
```
### Edit Makefile
Replace at line 28 with this link: 
https://update.k3s.io/v1-release/channels/v1.17
The final result should be like this:
```
28 K3S_TAG         := $(shell curl --silent "https://update.k3s.io/v1-release/channels/v1.17" | egrep -o '/v[^ ]+"' | sed -E 's/\/|\"//g' | s    ed -E 's/\+/\-/')
```
The same change should be applied at line 32, being the final result the following:
```
32 $(warning Output of curl: $(shell curl --silent "https://update.k3s.io/v1-release/channels/v1.17"))
```
Then, save all changes.

### Verify that go packages are installed
Run this command inside the repo.
```
sudo make install-tools
```
### Build for Linux
```
sudo make build
```
### Install K3D
Install to your GOPATH (**Note**: this will give you unreleased/bleeding-edge changes)
```
sudo go install
sudo make install
```
Verify the **k3d binary** file at:
```
$ ls /home/YOUR_USER_FOLDER/go/bin
```
### Move k3d binary file
```
$ sudo mv ./go/bin/k3d /usr/local/bin/
```
Test k3d command
```
$ k3d version
k3d version v3-dev
k3s version v1.17.14-k3s2 (default)
```
