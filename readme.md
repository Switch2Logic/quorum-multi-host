# Quorum IBFT network with permissoned-nodes 
This example makes use of the istanbul-tool to generate genisis and docker-compose file.

# Generate files
`$ istanbul setup --num 3 --docker-compose --quorum --save`

# Modify docker-compose.yml file
1. Remove all network configurations related to docker
2. Change the IP Addresses to point to host peer ipaddress
3. Foreach peer add the --permissioned \ flag under the entrypoint -> "geth \ --permissioned \
4. Foreach peer copy the static nodes 'echo ["enode://da99607a....' to a line below the copied line`
5. Change the file directory and the end of the new line '> /eth/geth/static-nodes.json too  > ./permissioned-nodes.json
6. Split the files up into 2 or 3 and copy it to each host.

# Start the docker containers
`$ docker-compose -f docker-compose-quorum-node1.yaml up -d`

# Attach to peer rpc
`geth attach http://192.168.31.101:8545`

# Create and Login to an user account
`personal.newAccount()`
`personal.unlockAccount(eth.accounts[0], "passwordHere", 300)`


# Post private contract
`var abi = [{"constant":true,"inputs":[],"name":"storedData","outputs":[{"name":"","type":"uint256"}],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"retVal","type":"uint256"}],"payable":false,"type":"function"},{"inputs":[{"name":"initVal","type":"uint256"}],"payable":false,"type":"constructor"}];`

`var bytecode = "0x6060604052341561000f57600080fd5b604051602080610149833981016040528080519060200190919050505b806000819055505b505b610104806100456000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff1680632a1afcd914605157806360fe47b11460775780636d4ce63c146097575b600080fd5b3415605b57600080fd5b606160bd565b6040518082815260200191505060405180910390f35b3415608157600080fd5b6095600480803590602001909190505060c3565b005b341560a157600080fd5b60a760ce565b6040518082815260200191505060405180910390f35b60005481565b806000819055505b50565b6000805490505b905600a165627a7a72305820d5851baab720bba574474de3d09dbeaabc674a15f4dd93b974908476542c23f00029";`

`var privateContract = web3.eth.contract(abi);`

`var contractTransaction = privateContract.new(42, {from:web3.eth.accounts[0], data: bytecode, gas: 0x47b760, privateFor: ["{PumKeyOfNode}"]}, function(e, contract) {
  if (e) {
    console.log("err creating contract", e);
  } else {
    if (!contract.address) {
      console.log("Contract transaction send: TransactionHash: " + contract.transactionHash + " waiting to be mined...");
    } else {
      console.log("Contract mined! Address: " + contract.address);
      console.log(contract);
    }
  }
});`


# Query contract
`var abi = [{"constant":true,"inputs":[],"name":"storedData","outputs":[{"name":"","type":"uint256"}],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"retVal","type":"uint256"}],"payable":false,"type":"function"},{"inputs":[{"name":"initVal","type":"uint256"}],"payable":false,"type":"constructor"}];`

`var address = "Conract Address here"`

`var private = eth.contract(abi).at(address)`

`private.get()`

# Notes
We should never expose the rpc of a peer to outside world. In this example the ipc is expose just for easy of testing. Be sure to change the --rpcaddr "0.0.0.0" -> --rpcaddr "127.0.0.1"




