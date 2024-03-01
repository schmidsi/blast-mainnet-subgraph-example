# Blast Mainnet Subgraph Example

This is an example subgraph for Blast Mainnet. It's simply the ERC721 subgraph from [OpenZeppelin Subgraphs](https://github.com/OpenZeppelin/openzeppelin-subgraphs).

Important piece is the `network` in [subgraph.yaml#7](./subgraph.yaml#7): It's set to: `blast-mainnet`. Otherwise, the subgraph is normal. See the official [Quick Start on thegraph.com](https://thegraph.com/docs/en/quick-start/).

Check out the [Preview GraphQL Playground](https://api.studio.thegraph.com/query/64409/blast-erc721/version/latest). A more sophisticated subgraph was published to

## Deploy a Blast testnet subgraph on Blast Mainnet

If there is no Blast testnet subgraph yet, [Quick Start on thegraph.com](https://thegraph.com/docs/en/quick-start/) to create a subgraph for Blast testnet.
Assuming there is already a subgraph that indexes Blast testnet, these are the steps to switch it to Blast Mainnet:

- Change the `network` in `subgraph.yaml` to `blast-mainnet`
- Change the `address` to the address of the contracts on Blast mainnet
- Change the `startBlock` to the block where the contracts have been deployed on mainnet

Example subgraph.yaml:

```yaml
specVersion: 1.0.0
indexerHints:
  prune: auto
schema:
  file: ./schema.graphql
dataSources:
  - kind: ethereum
    name: Contract
    network: blast-mainnet # <- Change this
    source:
      address: "0x4300000000000000000000000000000000000004" # <- Change this
      abi: Contract
      startBlock: 0 # <- Change this
    mapping:
      kind: ethereum/events
      apiVersion: 0.0.7
      language: wasm/assemblyscript
      entities:
        - Approval
        - ApprovalForAll
        - Transfer
      abis:
        - name: Contract
          file: ./abis/Contract.json
      eventHandlers:
        - event: Approval(indexed address,indexed address,indexed uint256)
          handler: handleApproval
        - event: ApprovalForAll(indexed address,indexed address,bool)
          handler: handleApprovalForAll
        - event: Transfer(indexed address,indexed address,indexed uint256)
          handler: handleTransfer
      file: ./src/contract.ts
```

- Run `graph codegen && graph build` to verify that it worked
- Go to [thegraph.com/studio](https://thegraph.com/studio) and log in
- Create a new subgraph
- In the right column, scroll down to "Auth & deploy"
- Copy/paste the `graph auth ...`, command to the terminal
- Run `graph deploy --studio testst` (assuming the subgraphs name was "testst") to deploy to the Subgraph Studio
- This gives you a development query URL to test everything and iterate
- Once the subgraph is ready for prime time, go to the subgraph studio and click publish. There is no need to add curation signal as of now.
