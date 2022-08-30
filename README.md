#### Readme file from [README-NEAR.md](./README-NEAR.md)
#### Repo [Link](https://github.com/near-examples/nft-tutorial)

In this example 5 main folder exist.
### 1. integration-tests:
In this folder 2 projects are prasent. 1st is in `rust` and this project is about integration tests.
2nd is in `typescript` and this project is also about integration tests.
### 2. market-contract:
In this project write near nft marketplace contract in rust.
### 3. nft-contract:
In this project write near nft contract in rust.
### 4. nft-series:
This the updated project of `nft-contract`
### 5. out :
In this folder all `.wasm` files are present. When we build the near contract a `.wasm` file is created and we deploy
these file on near blockchain.


## setup:
clone this project and `cd near-nft-tutorial`
1. install near-cli `npm install -g near-cli` [Link](https://docs.near.org/tools/near-cli#setup)
2. install wasm32-unknown-unknown `rustup target add wasm32-unknown-unknown`
3. create 2 new wallet [Link](https://wallet.testnet.near.org/)
    Near: tset-nft.testnet
    monkey view oppose asthma shock border crisp across since strategy gaze clown

    Near: test-market.testnet
    unique pride legend rocket island civil melody symptom hand report future elbow

## build and deploy nft-contract:
1. lonin in 'tset-nft.testnet' account in console `near login`
2. check account stste `near state tset-nft.testnet`
3. create sub account `near create-account sub.tset-nft.testnet --masterAccount sub.tset-nft.testnet`
4. build nft contract `cd nft-contract` and `./build.sh` this will create the `main.wasm` in `./out` folder
5. deploy contract `near deploy --accountId sub.tset-nft.testnet --wasmFile out/main.wasm `
6. if we want to redeploy then first we delete sub account `near delete sub.tset-nft.testnet`

## build and deploy marketplace-contract:
1. lonin in 'test-market.testnet' account in console `near login`
2. check account stste `near state test-market.testnet`
3. create sub account `near create-account sub.test-market.testnet --masterAccount test-market.testnet`
4. build nft contract `cd market-contract` and `./build.sh` this will create the `market.wasm` in `./out` folder
5. deploy contract `near deploy --accountId sub.test-market.testnet --wasmFile out/market.wasm`
6. if we want to redeploy then first we delete sub account `near delete sub.test-market.testnet`


## test marketplace-contract using near-cli:

- #### init:
`near call sub.test-market.testnet new '{"owner_id": "sub.test-market.testnet"}' --accountId sub.test-market.testnet`
this will initiate the `marketplace-contract`

- ####  get total sale:
`near view sub.test-market.testnet get_supply_sales ''`
this will give the total number of sale

- #### get number of nft on sale by single users:
`near view sub.test-market.testnet get_supply_by_owner_id '{"account_id": "any-account.testnet"}'`
this will give the total number of sale against the given user

- #### get list of nft on sale by single users:
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "test-market.testnet", "from_index": "0", "limit": 10}'`
this will give the list of nft against the given user address

- #### get number of nft belong to a nft contract:
`near view sub.test-market.testnet get_supply_by_nft_contract_id '{"nft_contract_id": "nft-contract.testnet"}'`
this will give the total number of sale against the given nft-contract address

- #### get list of nft on sale by single nft-contract address:
`near view sub.test-market.testnet get_sales_by_nft_contract_id '{"nft_contract_id": "sub.tset-nft.testnet", "from_index": "0", "limit": 10}'`
this will give the list of nft against the given nft-contract address

- #### get specific sale:
`near view sub.test-market.testnet get_sale '{"nft_contract_token": "contract_id.token_id"}'`
this will give the specific sale details we will give the `nft-contract-address.token-id` 
we can get the aproval id from this method

- #### pay and get storage:
`near call --accountId test-market.testnet sub.test-market.testnet storage_deposit '{"account_id": "test-market.testnet"}' --depositYocto 10000000000000000000000`
if we want to list  an nft in marketplace then first we will pay some storage fee using this method

- #### refund storage:
`near call --accountId test-market.testnet sub.test-market.testnet storage_withdraw '' --depositYocto 1`
we can get funds back after remove the sale 

- #### get storage fee:
`near view sub.test-market.testnet storage_minimum_balance ''`
get the storage fee for list or sale nft using nft-contract

- #### get current storage balance:
`near view sub.test-market.testnet storage_balance_of '{"account_id": "test-market.testnet"}'`
get current balane which we pay to the nft contract for the storage

- #### add new sale or list nft:
we can add new sale using `nft_on_approve` and this will call from `nft-contract` method `nft_approve`
we will give the approval to the `marketplace-contract` and pass a `msg` that msg will hold the nft price.
for list or sale see the `nft_approve` method

- #### remove sale:
`near call --accountId sub.test-market.testnet sub.test-market.testnet remove_sale '{"nft_contract_id": "nft-contract.testnet", "token_id": "nay-token-id"}' --depositYocto 1`
we can remove the sale using this method

- #### update sale price:
`near call --accountId test-market.testnet sub.test-market.testnet update_price '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-1", "price": "2000000000000000000000000"}' --depositYocto 1`
we can update the sale price using this method

- #### buy nft:
`near call --accountId tset-nft.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-1"}' --amount 2 --gas 200428073043512`
if we want to buy the nft then we will use the offer method 
this method get a veri high gass fee because in this method some `cross contract call` happens like `nft_transfer_payout` and `nft_approve`


## test nft-contract using near-cli:

- #### init:
`near call sub.tset-nft.testnet new_default_meta '{"owner_id": "sub.tset-nft.testnet"}' --accountId sub.tset-nft.testnet`
this will initiate the `nft-contract`, we can also use `new` instead of `new_default_meta`

- #### view:
`near view sub.tset-nft.testnet nft_metadata`
this will show the metedata values

- #### mint:
`near call --accountId tset-nft.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-1", "metadata": {"title": "My NFT Token 1", "description": "The Team Most Certainly Goes :)", "media": "https://bafybeiftczwrtyr3k7a2k4vutd3amkwsmaqyhrdzlhvpt33dyjivufqusq.ipfs.dweb.link/goteam-gif.gif"}, "receiver_id": "tset-nft.testnet"}' --amount 0.1`

`near call --accountId tset-nft.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-2", "metadata": {"title": "My NFT Token 2", "description": "Mint new image by account 1 :)", "media": "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQocUexY6mRJgooChOVC1HA3nG-O5FzARd6HQFgTyajYw&s"}, "receiver_id": "tset-nft.testnet"}' --amount 0.1`

`near call --accountId test-market.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-3", "metadata": {"title": "My NFT Token 3", "description": "Mint NFT token for test on 2nd account ;)", "media": "https://media.sproutsocial.com/uploads/2017/02/10x-featured-social-media-image-size.png"}, "receiver_id": "test-market.testnet"}' --amount 0.1`

`near call --accountId test-market.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-4", "metadata": {"title": "My NFT Token 4", "description": "Mint NFT token for test on 2nd account ;)", "media": "https://cdn.pixabay.com/photo/2015/04/23/22/00/tree-736885__480.jpg"}, "receiver_id": "test-market.testnet"}' --amount 0.1`

we can mint the new nft using mint method and pass the 0.1 Near for minting fee and storage fee whic will use the nft

- #### mint with royalty:
`near call --accountId example-status-msg.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-5", "metadata": {"title": "My NFT Token 5", "description": "Mint NFT token for test Roylty on example-status-msg.testnet ;)", "media": "https://miro.medium.com/max/775/0*rZecOAy_WVr16810"}, "receiver_id": "example-status-msg.testnet", "perpetual_royalties": {"example-status-msg.testnet":50}}' --amount 0.1`
we can add the royalty with mint method as a perameter

- #### view token:
`near view sub.tset-nft.testnet nft_token '{"token_id": "token-1"}'`
this will give the nft token details

- #### number of total nfts:
`near view sub.tset-nft.testnet nft_total_supply ''`
this will give the total count of minted nfts

- #### list of all nfts:
`near view sub.tset-nft.testnet nft_tokens '{"from_index": "0", "limit": 10}'`
this will gige the detail of all minted nfts

- #### number of against given account:
`near view sub.tset-nft.testnet nft_supply_for_owner '{"account_id": "test-market.testnet"}'`
`near view sub.tset-nft.testnet nft_supply_for_owner '{"account_id": "tset-nft.testnet"}'`
this will give the number of nft against the given account

- #### list of against given account:
`near view sub.tset-nft.testnet nft_tokens_for_owner '{"account_id": "test-market.testnet", "from_index": "0", "limit": 10}'`
`near view sub.tset-nft.testnet nft_tokens_for_owner '{"account_id": "tset-nft.testnet", "from_index": "0", "limit": 10}'`
this will give the detail list of all nft against given account

- #### detail of single nft by nft id:
`near view sub.tset-nft.testnet nft_token '{"token_id": "token-1"}'`
`near view sub.tset-nft.testnet nft_token '{"token_id": "token-5"}'`
this will give the nft detail against the given nft id

- #### give approvel to other user account:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-1", "account_id": "contract-example.testnet"}' --depositYocto 360000000000000000000`
we can give the approvel to other user account and that can sale our nft
we pay 360000000000000000000 Yocto(0.00036 NEAR) for space
and when we revoke we get 0.00025933 NEAR as space refund

- #### check the approval:
`near view sub.tset-nft.testnet nft_is_approved '{"token_id": "token-1", "approved_account_id": "contract-example.testnet"}'`
this will give true or false against the nft approval

- #### revoke approval from specific account:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_revoke '{"token_id": "token-1", "account_id": "contract-example.testnet"}' --depositYocto -1`
this will revoke the approval of specific nft for specified account and refund the space fee

- #### revoke approval for all:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_revoke_all '{"token_id": "token-1"}' --depositYocto -1`
this will revoke the approval from all account of given token and refund the space fee 

- #### nft_transfer_call, nft_on_transfer and nft_resolve_transfer:
`nft_transfer_call` call cross contract to `nft_on_transfer` and resolve promise using `nft_resolve_transfer` 
then refund users funds using `refund_approved_account_ids`

- #### transfer:
`near call --accountId tset-nft.testnet sub.tset-nft.testnet nft_transfer '{"receiver_id": "test-market.testnet", "token_id": "token-1", "memo": "Go Team :)"}' --depositYocto 1`
transfer nft to other address

- #### transfer from approval account:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_transfer '{"receiver_id": "tset-nft.testnet", "token_id": "token-1", "approval_id": 2, "memo": "Go Team :)"}' --depositYocto 1`
approval accout transfer the nft to other accout address
we can get approval id using `nft_token` and can transfer with out giving the `approval_id`

- #### check the royalty distribution:
`near view sub.tset-nft.testnet nft_payout '{"token_id": "token-5", "balance": "5000000000000000000000000", "max_len_payout": 7}'`
this will give the all accounts and their distribution

- #### royalty test:
**mint:**
`near call --accountId example-status-msg.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-5", "metadata": {"title": "My NFT Token 5", "description": "Mint NFT token for test Roylty on example-status-msg.testnet ;)", "media": "https://miro.medium.com/max/775/0*rZecOAy_WVr16810"}, "receiver_id": "example-status-msg.testnet", "perpetual_royalties": {"example-status-msg.testnet":50}}' --amount 0.1`

**pay storage fee**
#### 1stowner:
`near call --accountId example-status-msg.testnet sub.test-market.testnet storage_deposit '{"account_id": "example-status-msg.testnet"}' --depositYocto 10000000000000000000000`

**add sale or give approval to nft contract:**
`near call --accountId example-status-msg.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-5", "account_id": "sub.test-market.testnet", "msg": "{\"sale_conditions\": \"5000000000000000000000000\"}"}' --depositYocto 360000000000000000000`

**verify sale or list:**
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "example-status-msg.testnet", "from_index": "0", "limit": 10}'`

**update sale price:**
`near call --accountId example-status-msg.testnet sub.test-market.testnet update_price '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5", "price": "5000000000000000000000000"}' --depositYocto 1`

**check account status:**
`near state example-status-msg.testnet` royalty account

**buy nft:**
`near call --accountId contract-example.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5"}' --amount 5 --gas 200428073043512`

**check account status:**
`near state example-status-msg.testnet` royalty account


#### 2nd owner:
**pay storage fee**
`near call --accountId contract-example.testnet sub.test-market.testnet storage_deposit '{"account_id": "contract-example.testnet"}' --depositYocto 10000000000000000000000`

**add sale or give approval to nft contract:**
`near call --accountId contract-example.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-5", "account_id": "sub.test-market.testnet", "msg": "{\"sale_conditions\": \"5000000000000000000000000\"}"}' --depositYocto 360000000000000000000`

**verify sale or list:**
`near view sub.test-market.testnet get_supply_sales ''`
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "contract-example.testnet", "from_index": "0", "limit": 10}'`

**check royalty distribution:**
`near view sub.tset-nft.testnet nft_payout '{"token_id": "token-5", "balance": "5000000000000000000000000", "max_len_payout": 7}'`

**check account status:**
`near state example-status-msg.testnet` royalty account
`near state contract-example.testnet`

**buy nft:**
`near call --accountId test-market.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5"}' --amount 5 --gas 200428073043512`

**check account status:**
`near state example-status-msg.testnet` royalty account
`near state contract-example.testnet`


#### 3rd owner:
**pay storage fee**
`near call --accountId test-market.testnet sub.test-market.testnet storage_deposit '{"account_id": "test-market.testnet"}' --depositYocto 10000000000000000000000`

**add sale or give approval to nft contract:**
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-5", "account_id": "sub.test-market.testnet", "msg": "{\"sale_conditions\": \"5000000000000000000000000\"}"}' --depositYocto 360000000000000000000`

**verify sale or list:**
`near view sub.test-market.testnet get_supply_sales ''`
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "test-market.testnet", "from_index": "0", "limit": 10}'`

**check royalty distribution:**
`near view sub.tset-nft.testnet nft_payout '{"token_id": "token-5", "balance": "5000000000000000000000000", "max_len_payout": 7}'`

**check account status:**
`near state example-status-msg.testnet` royalty account
`near state contract-example.testnet`
`near state test-market.testnet`
`near state tset-nft.testnet`

**buy nft:**
`near call --accountId tset-nft.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5"}' --amount 5 --gas 200428073043512`

**check account status:**
`near state example-status-msg.testnet` royalty account
`near state contract-example.testnet`
`near state test-market.testnet`
`near state tset-nft.testnet`




## Explain nft-contract:
- ### `.rs` files:
1. `approval.rs`:
2. `enumeration.rs`:
3. `events.rs`:
4. `internal.rs`:
5. `lib.rs`:
6. `metadata.rs`:
7. `mint.rs`:
8. `nft_core.rs`:
9. `royalty.rs`:
10. `tests.rs`:


- ### imports:
`use std::collections::HashMap;` // use std collections for Deserialize, Serialize because near-sdk::collection don't implement serde
`use near_sdk::borsh::{self, BorshDeserialize, BorshSerialize};` // use for binary serialization and deserialization
`use near_sdk::collections::{LazyOption, LookupMap, UnorderedMap, UnorderedSet};` // near-sdk::collection array and map
`use near_sdk::json_types::{Base64VecU8, U128};` // use for get large integers as JSON string
`use near_sdk::serde::{Deserialize, Serialize};` // use for JSON serialization and deserialization
`use near_sdk::{env, near_bindgen, AccountId, Balance, CryptoHash, PanicOnDefault, Promise, PromiseOrValue,};` // some utility methods and properties

- ### traits:

- ### struct:

- ### functions:


## Explain market-contract:
- ### `.rs` files:
1. `external.rs`:
2. `internal.rs`:
3. `lib.rs`:
4. `nft_callbacks.rs`:
5. `sale_views.rs`:
6. `sale.rs`:
7. `tests.rs`:


- ### imports:
`use near_sdk::borsh::{self, BorshDeserialize, BorshSerialize};` // use for binary serialization and deserialization
`use near_sdk::collections::{LookupMap, UnorderedMap, UnorderedSet};` // near-sdk::collection array and map
`use near_sdk::json_types::{U128, U64};` // use for get large integers as JSON string
`use near_sdk::serde::{Deserialize, Serialize};` // use for JSON serialization and deserialization
`use near_sdk::{assert_one_yocto, env, ext_contract, near_bindgen, AccountId, Balance, Gas, PanicOnDefault, Promise, CryptoHash, BorshStorageKey,};` // some utility methods and properties
`use std::collections::HashMap;` // use std collections for Deserialize, Serialize because near-sdk::collection don't implement serde

- ### traits:

- ### struct:

- ### functions:
