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
    **Near: tset-nft.testnet**
    monkey view oppose asthma shock border crisp across since strategy gaze clown

    **Near: test-market.testnet**
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


# test marketplace-contract using near-cli:

### init:
`near call sub.test-market.testnet new '{"owner_id": "sub.test-market.testnet"}' --accountId sub.test-market.testnet` <br>
this will initiate the `marketplace-contract`

###  get total sale:
`near view sub.test-market.testnet get_supply_sales ''` <br>
this will give the total number of sale

### get number of nft on sale by single users:
`near view sub.test-market.testnet get_supply_by_owner_id '{"account_id": "any-account.testnet"}'` <br>
this will give the total number of sale against the given user

### get list of nft on sale by single users:
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "test-market.testnet", "from_index": "0", "limit": 10}'` <br>
this will give the list of nft against the given user address

### get number of nft belong to a nft contract:
`near view sub.test-market.testnet get_supply_by_nft_contract_id '{"nft_contract_id": "nft-contract.testnet"}'` <br>
this will give the total number of sale against the given nft-contract address

### get list of nft on sale by single nft-contract address:
`near view sub.test-market.testnet get_sales_by_nft_contract_id '{"nft_contract_id": "sub.tset-nft.testnet", "from_index": "0", "limit": 10}'` <br>
this will give the list of nft against the given nft-contract address

### get specific sale:
`near view sub.test-market.testnet get_sale '{"nft_contract_token": "contract_id.token_id"}'` <br>
this will give the specific sale details we will give the `nft-contract-address.token-id` <br>
we can get the aproval id from this method

### pay and get storage:
`near call --accountId test-market.testnet sub.test-market.testnet storage_deposit '{"account_id": "test-market.testnet"}' --depositYocto 10000000000000000000000` <br>
if we want to list  an nft in marketplace then first we will pay some storage fee using this method

### refund storage:
`near call --accountId test-market.testnet sub.test-market.testnet storage_withdraw '' --depositYocto 1` <br>
we can get funds back after remove the sale 

### get storage fee:
`near view sub.test-market.testnet storage_minimum_balance ''` <br>
get the storage fee for list or sale nft using nft-contract

### get current storage balance:
`near view sub.test-market.testnet storage_balance_of '{"account_id": "test-market.testnet"}'` <br>
get current balane which we pay to the nft contract for the storage

### add new sale or list nft:
we can add new sale using `nft_on_approve` and this will call from `nft-contract` method `nft_approve` <br>
we will give the approval to the `marketplace-contract` and pass a `msg` that msg will hold the nft price. <br>
for list or sale see the `nft_approve` method

### remove sale:
`near call --accountId sub.test-market.testnet sub.test-market.testnet remove_sale '{"nft_contract_id": "nft-contract.testnet", "token_id": "nay-token-id"}' --depositYocto 1` <br>
we can remove the sale using this method

### update sale price:
`near call --accountId test-market.testnet sub.test-market.testnet update_price '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-1", "price": "2000000000000000000000000"}' --depositYocto 1` <br>
we can update the sale price using this method

### buy nft:
`near call --accountId tset-nft.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-1"}' --amount 2 --gas 200428073043512` <br>
if we want to buy the nft then we will use the offer method <br>
this method get a veri high gass fee because in this method some `cross contract call` happens like `nft_transfer_payout` and `nft_approve`


# test nft-contract using near-cli:

### init:
`near call sub.tset-nft.testnet new_default_meta '{"owner_id": "sub.tset-nft.testnet"}' --accountId sub.tset-nft.testnet` <br>
this will initiate the `nft-contract`, we can also use `new` instead of `new_default_meta`

### view:
`near view sub.tset-nft.testnet nft_metadata` <br>
this will show the metedata values

### mint:
`near call --accountId tset-nft.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-1", "metadata": {"title": "My NFT Token 1", "description": "The Team Most Certainly Goes :)", "media": "https://bafybeiftczwrtyr3k7a2k4vutd3amkwsmaqyhrdzlhvpt33dyjivufqusq.ipfs.dweb.link/goteam-gif.gif"}, "receiver_id": "tset-nft.testnet"}' --amount 0.1`

`near call --accountId tset-nft.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-2", "metadata": {"title": "My NFT Token 2", "description": "Mint new image by account 1 :)", "media": "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQocUexY6mRJgooChOVC1HA3nG-O5FzARd6HQFgTyajYw&s"}, "receiver_id": "tset-nft.testnet"}' --amount 0.1`

`near call --accountId test-market.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-3", "metadata": {"title": "My NFT Token 3", "description": "Mint NFT token for test on 2nd account ;)", "media": "https://media.sproutsocial.com/uploads/2017/02/10x-featured-social-media-image-size.png"}, "receiver_id": "test-market.testnet"}' --amount 0.1`

`near call --accountId test-market.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-4", "metadata": {"title": "My NFT Token 4", "description": "Mint NFT token for test on 2nd account ;)", "media": "https://cdn.pixabay.com/photo/2015/04/23/22/00/tree-736885__480.jpg"}, "receiver_id": "test-market.testnet"}' --amount 0.1` <br>

we can mint the new nft using mint method and pass the 0.1 Near for minting fee and storage fee whic will use the nft

### mint with royalty:
`near call --accountId example-status-msg.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-5", "metadata": {"title": "My NFT Token 5", "description": "Mint NFT token for test Roylty on example-status-msg.testnet ;)", "media": "https://miro.medium.com/max/775/0*rZecOAy_WVr16810"}, "receiver_id": "example-status-msg.testnet", "perpetual_royalties": {"example-status-msg.testnet":50}}' --amount 0.1` <br>
we can add the royalty with mint method as a perameter

### view token:
`near view sub.tset-nft.testnet nft_token '{"token_id": "token-1"}'` <br>
this will give the nft token details

### number of total nfts:
`near view sub.tset-nft.testnet nft_total_supply ''` <br>
this will give the total count of minted nfts

### list of all nfts:
`near view sub.tset-nft.testnet nft_tokens '{"from_index": "0", "limit": 10}'` <br>
this will gige the detail of all minted nfts

### number of against given account:
`near view sub.tset-nft.testnet nft_supply_for_owner '{"account_id": "test-market.testnet"}'`
`near view sub.tset-nft.testnet nft_supply_for_owner '{"account_id": "tset-nft.testnet"}'` <br>
this will give the number of nft against the given account

### list of against given account:
`near view sub.tset-nft.testnet nft_tokens_for_owner '{"account_id": "test-market.testnet", "from_index": "0", "limit": 10}'`
`near view sub.tset-nft.testnet nft_tokens_for_owner '{"account_id": "tset-nft.testnet", "from_index": "0", "limit": 10}'` <br>
this will give the detail list of all nft against given account

### detail of single nft by nft id:
`near view sub.tset-nft.testnet nft_token '{"token_id": "token-1"}'` <br>
`near view sub.tset-nft.testnet nft_token '{"token_id": "token-5"}'` <br>
this will give the nft detail against the given nft id

### give approvel to other user account:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-1", "account_id": "contract-example.testnet"}' --depositYocto 360000000000000000000` <br>
we can give the approvel to other user account and that can sale our nft <br>
we pay 360000000000000000000 Yocto(0.00036 NEAR) for space <br>
and when we revoke we get 0.00025933 NEAR as space refund <br>

### check the approval:
`near view sub.tset-nft.testnet nft_is_approved '{"token_id": "token-1", "approved_account_id": "contract-example.testnet"}'` <br>
this will give true or false against the nft approval

### revoke approval from specific account:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_revoke '{"token_id": "token-1", "account_id": "contract-example.testnet"}' --depositYocto -1` <br>
this will revoke the approval of specific nft for specified account and refund the space fee

### revoke approval for all:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_revoke_all '{"token_id": "token-1"}' --depositYocto -1` <br>
this will revoke the approval from all account of given token and refund the space fee 

### nft_transfer_call, nft_on_transfer and nft_resolve_transfer:
`nft_transfer_call` call cross contract to `nft_on_transfer` and resolve promise using `nft_resolve_transfer` 
then refund users funds using `refund_approved_account_ids`

### transfer:
`near call --accountId tset-nft.testnet sub.tset-nft.testnet nft_transfer '{"receiver_id": "test-market.testnet", "token_id": "token-1", "memo": "Go Team :)"}' --depositYocto 1` <br>
transfer nft to other address

### transfer from approval account:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_transfer '{"receiver_id": "tset-nft.testnet", "token_id": "token-1", "approval_id": 2, "memo": "Go Team :)"}' --depositYocto 1` <br>
approval accout transfer the nft to other accout address <br>
we can get approval id using `nft_token` and can transfer with out giving the `approval_id`

### check the royalty distribution:
`near view sub.tset-nft.testnet nft_payout '{"token_id": "token-5", "balance": "5000000000000000000000000", "max_len_payout": 7}'` <br>
this will give the all accounts and their distribution

# royalty test:
### 1st owner:
#### mint:
`near call --accountId example-status-msg.testnet sub.tset-nft.testnet nft_mint '{"token_id": "token-5", "metadata": {"title": "My NFT Token 5", "description": "Mint NFT token for test Roylty on example-status-msg.testnet ;)", "media": "https://miro.medium.com/max/775/0*rZecOAy_WVr16810"}, "receiver_id": "example-status-msg.testnet", "perpetual_royalties": {"example-status-msg.testnet":50}}' --amount 0.1`

#### pay storage fee:
`near call --accountId example-status-msg.testnet sub.test-market.testnet storage_deposit '{"account_id": "example-status-msg.testnet"}' --depositYocto 10000000000000000000000`

#### add sale or give approval to nft contract:
`near call --accountId example-status-msg.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-5", "account_id": "sub.test-market.testnet", "msg": "{\"sale_conditions\": \"5000000000000000000000000\"}"}' --depositYocto 360000000000000000000`

#### verify sale or list:
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "example-status-msg.testnet", "from_index": "0", "limit": 10}'`

#### update sale price:
`near call --accountId example-status-msg.testnet sub.test-market.testnet update_price '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5", "price": "5000000000000000000000000"}' --depositYocto 1`

#### check account status:
`near state example-status-msg.testnet` royalty account

#### buy nft:
`near call --accountId contract-example.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5"}' --amount 5 --gas 200428073043512`

#### check account status:
`near state example-status-msg.testnet` royalty account


### 2nd owner:
#### pay storage fee:
`near call --accountId contract-example.testnet sub.test-market.testnet storage_deposit '{"account_id": "contract-example.testnet"}' --depositYocto 10000000000000000000000`

#### add sale or give approval to nft contract:
`near call --accountId contract-example.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-5", "account_id": "sub.test-market.testnet", "msg": "{\"sale_conditions\": \"5000000000000000000000000\"}"}' --depositYocto 360000000000000000000`

#### verify sale or list:
`near view sub.test-market.testnet get_supply_sales ''`<br>
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "contract-example.testnet", "from_index": "0", "limit": 10}'`

#### check royalty distribution:
`near view sub.tset-nft.testnet nft_payout '{"token_id": "token-5", "balance": "5000000000000000000000000", "max_len_payout": 7}'`

#### check account status:
`near state example-status-msg.testnet` royalty account <br>
`near state contract-example.testnet`

#### buy nft:
`near call --accountId test-market.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5"}' --amount 5 --gas 200428073043512`

#### check account status:
`near state example-status-msg.testnet` royalty account <br>
`near state contract-example.testnet`


### 3rd owner:
#### pay storage fee:
`near call --accountId test-market.testnet sub.test-market.testnet storage_deposit '{"account_id": "test-market.testnet"}' --depositYocto 10000000000000000000000`

#### add sale or give approval to nft contract:
`near call --accountId test-market.testnet sub.tset-nft.testnet nft_approve '{"token_id": "token-5", "account_id": "sub.test-market.testnet", "msg": "{\"sale_conditions\": \"5000000000000000000000000\"}"}' --depositYocto 360000000000000000000`

#### verify sale or list:
`near view sub.test-market.testnet get_supply_sales ''`<br>
`near view sub.test-market.testnet get_sales_by_owner_id '{"account_id": "test-market.testnet", "from_index": "0", "limit": 10}'`

#### check royalty distribution:
`near view sub.tset-nft.testnet nft_payout '{"token_id": "token-5", "balance": "5000000000000000000000000", "max_len_payout": 7}'`

#### check account status:
`near state example-status-msg.testnet` royalty account <br>
`near state contract-example.testnet`<br>
`near state test-market.testnet`<br>
`near state tset-nft.testnet`<br>

#### buy nft:
`near call --accountId tset-nft.testnet sub.test-market.testnet offer '{"nft_contract_id": "sub.tset-nft.testnet", "token_id": "token-5"}' --amount 5 --gas 200428073043512`

#### check account status:
`near state example-status-msg.testnet` royalty account <br>
`near state contract-example.testnet`<br>
`near state test-market.testnet`<br>
`near state tset-nft.testnet`<br>




# Explain nft-contract:
## `.rs` files:
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
    run test `cargo tset`, in this file 13 test function exists <br>
    1st we create context using `get_context` method and we can get metadata using `sample_token_metadata` method

    1. **test_default - should_panic:** access contract without initialize
    2. **test_new_account_contract:** init contract and access
    3. **test_mint_nft:** mint new nft and test minting
    4. **test_internal_transfer:** transfer nft from one account to other
    5. **test_nft_approve:** give approval to other account
    6. **test_nft_revoke:** give nft approval and then revoke using 'revoke' method
    7. **test_revoke_all:** give nft approval and then revoke using 'revoke_all' method 
    8. **test_internal_remove_token_from_owner:** test internal method for revoke token
    9. **test_nft_payout:** test the 'nft_payout' which use for royalty distribution
    10. **test_nft_total_supply:** check the totel minted nfts
    11. **events::tests::nep_format_mint**
    12. **events::tests::nep_format_vector**
    13. **events::tests::nep_format_transfer_all_fields**



## imports:
1. `use std::collections::HashMap;` <br>
use std collections for Deserialize, Serialize because near-sdk::collection don't implement serde

2. `use near_sdk::borsh::{self, BorshDeserialize, BorshSerialize};` <br>
use for binary serialization and deserialization

3. `use near_sdk::collections::{LazyOption, LookupMap, UnorderedMap, UnorderedSet};` <br>
near-sdk::collection array and map

4. `use near_sdk::json_types::{Base64VecU8, U128};` <br>
use for get large integers as JSON string

5. `use near_sdk::serde::{Deserialize, Serialize};` <br>
use for JSON serialization and deserialization

6. `use near_sdk::{env, near_bindgen, AccountId, Balance, CryptoHash, PanicOnDefault, Promise, PromiseOrValue,};` <br>
some utility methods and properties



# Explain market-contract:
## `.rs` files:
1. `external.rs`:
    In this file create a trait `trait ExtContract` which have 1 method defination `nft_transfer_payout` that use for initiate a cross contract call to the nft contract. This will transfer the token to the buyer and return a payout object used for the market to distribute royalty funds to the appropriate accounts.


2. `internal.rs`:
    This file have 2 methods:
    1. `hash_account_id`: used to generate a unique prefix in our storage collections
    2. `internal_remove_sale`: implement contract `impl Contract` and add 1 internal method `nternal_remove_sale` for removing a sale from the market. This returns the previously removed sale object


3. `lib.rs`:
    This file have the initialization methord and imports all external crates from `std` and `near_sdk` and also import all internal files in this file whic we use in all other files of thes project. <br>
    1. create 2 struct `struct Payout` use of royalty and `struct Contract` which is the main struct use for store all the information
    2. create enum `enum StorageKey` use for keys of the persistent collections
    3. implement the contract `impl Contract` and add 5 function in contract struct
        1. `init`: initialization function (can only be called once)
        2. `storage_deposit`: payable method that allows users to deposit storage. When user wants to add a new sale 1st he will pay for the storage and an optional account ID is for users can pay for storage for other people.
        3. `storage_withdraw`: payable method allows users to withdraw thier storage deposit
        4. `storage_minimum_balance`: return the minimum storage for sale
        5. `storage_balance_of`: return how much storage an account has paid for

4. `nft_callbacks.rs`:
    This file have `nft_on_approve` method which use for add new sale (list new NFT). <br>
    1. This have 1 struct `struct SaleArgs` which use for get the sale nft price
    2. Have a trait `trait NonFungibleTokenApprovalsReceiver` and this trait have a methor defination `nft_on_approve`
    3. implemet that trat for contract ctruct `impl NonFungibleTokenApprovalsReceiver for Contract` and add 1 method in Contract. This method used as the callback from the NFT contract. When `nft_approve is` called, it will fire a cross contract call to this marketplace and this is the function `nft_on_approve` that is invoked

5. `sale_views.rs`:
    This file is about view related methods. In this file implement the `impl Contract` with 6 functions:
    1. `get_supply_sales`: return the number of sales that market have
    2. `get_supply_by_owner_id`: retun the total number of sale which belongs to the given account
    3. `get_sales_by_owner_id`: return the `Vec<Sale>` means retuen the list of listed nft which belongs to the given account
    4. `get_supply_by_nft_contract_id`: retun the total number of sale which belongs to the given contract
    5. `get_sales_by_nft_contract_id`: return the `Vec<Sale>` means retuen the list of listed nft which belongs to the given contract 
    6. `get_sale`: this function give the `Sale` detail of given NFT address(nft-contract-address.token-id)

6. `sale.rs`:
    Perfome all sale related functionality.
    1. This file have a struct `Sale` which holds important information about each sale on the market.
    2. And have a trait `trait ExtSelf` which have `resolve_purchase` method defination which use for resolve the promise when calling `nft_transfer_payout`. It's check that everything is fine otherwise it's refund.
    3. implement the `impl Contract` with 5 functions:
        1. `remove_sale`: remove NFT from sale
        2. `update_price`: update the prive of Listed NFT
        3. `offer`: use for buy an NFT this function use `process_purchase` and then use `resolve_purchase` for buying NFT
        4. `process_purchase`: private function call from `offer` this will remove the sale, transfer and get the payout from the nft contract, and then distribute royalties and use `resolve_purchase` method for resolve the promise
        5. `resolve_purchase`: private method used to resolve the promise when calling `nft_transfer_payout` this function call from `process_purchase` function

7. `tests.rs`:
    run test `cargo tset`, in this file 7 test function exists <br>
    1st we create context using `get_context` method
    1. **test_default - should_panic:** access contract without initialize
    2. **test_storage_deposit_insufficient_deposit:** attach some wrond amount for storage
    3. **test_storage_deposit:** init contract and test the storage
    4. **test_storage_balance_of:** test the amount that give by an account for storage
    5. **test_storage_withdraw:** give som eamount for storage and test
    6. **test_remove_sale:** add an NFT for sale and then remove it from sale
    7. **test_update_price:** add an NFT for sale and then update it's price


## imports:
1. `use near_sdk::borsh::{self, BorshDeserialize, BorshSerialize};` <br>
use for binary serialization and deserialization

2. `use near_sdk::collections::{LookupMap, UnorderedMap, UnorderedSet};` <br>
near-sdk::collection array and map

3. `use near_sdk::json_types::{U128, U64};` <br>
use for get large integers as JSON string

4. `use near_sdk::serde::{Deserialize, Serialize};` <br>
use for JSON serialization and deserialization


5. `use near_sdk::{assert_one_yocto, env, ext_contract, near_bindgen, AccountId, Balance, Gas, PanicOnDefault, Promise, CryptoHash, BorshStorageKey,};` <br>
 some utility methods and properties

6. `use std::collections::HashMap;` <br>
use std collections for Deserialize, Serialize because near-sdk::collection don't implement serde

