# asteroid

# Program that send log formatted using a golang template to a logdna server
git clone https://github.com/GalacticHiker/asteroid.git

# to build
cd ~/git_repos/asteroid/src

go build -x -o ../runtime/bin/logdna-feeder cmd/logdna-feeder/main.go

# should always be a stand-alone executable

../runtime/ is the directory footprint that is assumed when using optional file artifacts (e.g log template)

if a file path starts with a '/' it is assumed to be an absolute path

# to execute
export LOGDNA_API_KEY=your_LOGDNA_API_KEY


../runtime/bin/logdna-feeder --hostname='logdna-feeder-host' --log-file-name='logdna-feeder-filename' --logsPerTick=1 --nLogsToSend=10 --tick=1s

../runtime/bin/logdna-feeder --hostname='logdna-feeder-host' --log-file-name='logdna-feeder-filename' --logsPerTick=1 --nLogsToSend=10 --tick=1s --template=defaultKVP

https://github.com/GalacticHiker/asteroid.git

# unimplemented cli arguments

tick  - the tick rate of the send clock, hardcoded to 500ms 
logsPerTick - how many logs to send per tick, hardcoded to 1; so send 2 log per second
nLogsToSend - how many log to send, hardcoded to 10

