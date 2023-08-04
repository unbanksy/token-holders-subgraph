# Token Holders Subgraph

## Setup
1. Install neccessary packages: 
```
npm install -g @graphprotocol/graph-cli
yarn global add @graphprotocol/graph-cli
yarn global add @graphprotocol/graph-ts
```
2. Create a subgraph in Subgraph Studio, get the deploy key.
3. Authenticate: `graph auth --studio DEPLOY_KEY`
4. Deploy: `graph deploy --studio olympus-tokenholders-subgraph`

## Intended Uses

- Tracking the number of holders of a particular token
- Tracking the balances of particular token and holder combinations

## Architecture

### Considerations

- Calculating daily balances within a subgraph is currently prohibitively slow, so this is outsourced to the Google Cloud Function deployed by the [token-holder-balances](https://github.com/OlympusDAO/token-holder-balances) repo.

### Design Principles

- The subgraph should need minimal maintenance
- The subgraph entities should enable users to calculate the metrics they need (i.e. don't try too hard to anticipate the metrics needed)

### Indexing

Indexing takes place in the following circumstances:

- Transfer function call
    - When the `transfer` function is called on any of the tokens, a `TokenHolderTransaction` record is created for each of the sender and receiver.
