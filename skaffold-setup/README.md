# Skaffold

## Install
```shell
apk add --no-cache --virtual .build-deps make  curl # for editing JSON file
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 # Get skaffold-setup binary
chmod +x skaffold  # set executable
sudo mv skaffold  /usr/local/bin # move to system directory
skaffold version # Check 
```