# Asteroid

logmill is a utility that sends logs formatted using a golang template at a specified rate to a rsyslogd:tcp:514, logdna server

# Status
Works on my machine. 😏  YMMV.

Supported logging servers: Logdna API, TCP Sylog

# Source code
git clone https://github.com/GalacticHiker/asteroid.git

# Build

## Build (on mac workstation)
cd ~/git_repos/asteroid/src

go build -x -o ../runtime/bin/logmill cmd/logmill/main.go

## Build for docker (all libraries statically linked)
CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o ../runtime/bin/logmill-linux cmd/logmill/main.go

## Build on Centos7 VM 
/usr/local/go/bin/go build -x -o ../runtime/bin/logmill cmd/logmill/main.go/home/tharnett/asteroid/src

# Design Intents
should always be a stand-alone executable (no required support files)

../runtime/ is the directory footprint that is assumed when using optional file artifacts (e.g log templates)

if a file path starts with a '/' it is assumed to be an absolute path

# Kafka Dependencies
 Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install librdkafka pkg-config
export PKG_CONFIG_PATH="/usr/local/opt/openssl/lib/pkgconfig"

go build -x -o ../runtime/bin/kafka_producer cmd/kafka_producer_example/main.go

# Example Usage

## To send logs via logdna api
export LOGDNA_API_KEY=your_LOGDNA_API_KEY


## To send logs to rsyslog tcp.  *TODO:* enabling rsyslog 
../runtime/bin/logmill --protocol tcp --destAddr 192.168.0.29:514 --tick=1s --logsPerTick=1 --nLogsToSend=10 --tag "HelloTcp" --template=defaultKVP

# Usage
## Logdna
1. hostname
2. logdna file

## Syslog
1. destAddr

## Common

1. tick  - the tick rate of the send clock, hardcoded to 500ms 
2. logsPerTick - how many logs to send per tick, hardcoded to 1
3. nLogsToSend - how many log to send, hardcoded to 10
4. template -- the golang template to use default (defaultKVP)

## Docker 
```bash
# copy binary to dockerfile context
cp ../runtime/bin/logmill-linux ../docker/context/bin
# copy optional auxilary artifacts
cp ../runtime/conf/example.template ../docker/context/conf

docker build --no-cache -t logmill:v1 -f ../docker/Dockerfile ../docker/context
docker run --rm -d logmill:v1

# arguments specified on docker command, when ENTRYPOINT is defined
docker run --rm -d logmill:v1  --protocol tcp --destAddr 192.168.0.29:514 --logsPerTick=1 --nLogsToSend=10 --tick=1s --template=defaultKVP --tag ENTRYPOINT

# COMMAND command & arguments specified on docker command when CMD is specified
docker run --rm -d logmill:v1 /logmill/bin/logmill-linux --protocol tcp --destAddr 192.168.0.29:514 --logsPerTick=1 --nLogsToSend=10 --tick=1s --template=defaultKVP --tag COMMAND

# override entrypoint to get bash shell
docker run --rm -it --entrypoint "/bin/bash" logmill:v1

```
# TODO
1. List default templates. With examples
2. Argument default values
3. Validate arguments are valid for mill type; e.g. logdna vs syslog
4. template context values
5. lotsa features
6. tests


