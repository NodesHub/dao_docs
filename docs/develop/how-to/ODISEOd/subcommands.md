# Subcommands

This section describes the subcommands available from `achillesd`.

## `debug addr`

Changes an address from hex encoding to bech32.

**Syntax**
```bash
terrad debug addr <address>
```

**Example**
```bash
achillesd debug addr ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm
```

## `debug pubkey`

Decodes a pubkey from proto JSON and displays the address.

**Syntax**
```bash
terrad debug pubkey <pubkey>
```

**Example**
```bash
terrad debug pubkey '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AurroA7jvfPd1AadmmOvWM2rJSwipXfRf8yD6pLbA2DJ"}'
```

## `debug raw-bytes`

Changes raw bytes to hex.

**Syntax**
```bash
terrad debug raw-bytes <raw-bytes>
```

**Example**
```bash
terrad debug raw-bytes [72 101 108 108 111 44 32 112 108 97 121 103 114 111 117 110 100]
```

## `keys add`

Generates a public and private key pair for an account so that you can receive funds, send funds, create bonding transactions, and so on.

::: {tip}
For security purposes, run this command on an offline computer.
:::

**Syntax**
```bash
terrad keys add <your-key-name>
```

where `<your-key-name>` is the name of the account. It references the account number used to derive the key pair from the mnemonic. When you want to send a transaction, you will use this name to identify your account.

To specify the path \(`0`, `1`, `2`, ...\) you want to use to generate your account, you can append the optional `--account` flag. By default, account `0` is generated.

The command generates a 24-word mnemonic and saves the private and public keys for account `0` simultaneously. You are prompted to specify a passphrase that is used to encrypt the private key of account `0` on disk. Each time you want to send a transaction, this password is required. If you lose the password, you can always recover the private key by using the mnemonic phrase.

::: {danger}
To prevent theft or loss of funds, ensure that you keep multiple copies of your mnemonic and store it in a secure place and that only you know how to access it. If someone is able to gain access to your mnemonic, they are able to gain access to your private keys and control the accounts associated with them.
:::

::: {tip}
After you have triple-checked your mnemonic and safely stored it, you can delete bash history to ensure no one can retrieve it.

```bash
history -c
rm ~/.bash_history
```
:::

To generate more accounts from the same mnemonic, run:

```bash
terrad keys add <your-key-name> --recover --account 1
```

You are prompted to specify a passphrase and your mnemonic. To generate a different account, change the account number.

::: {danger}
- Do not use the same passphrase for multiple keys. Do not lose or share your mnemonic with anyone.
:::

**Example**
```bash
terrad keys add myAccount
```

In some cases, you might need to recover your key. If you have the mnemonic that was used to generate your private key, you can recover it and re-register your key. Issuing the following command will prompt you to enter your 24-word mnemonic.

**Syntax**
```bash
terrad keys add <yourKeyName> --recover
```

For information about generating multisignature accounts and signing transactions, see [Sign with a multisig account](../sign-with-multisig.md).

## `keys show`

Retrieves an address for a specified account. The address is prefixed by `ODISEO-`, for example `ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc`. To receive funds, you must give an account address to the sender.

**Syntax**
```bash
terrad keys show <account-name>
```

**Example**
```bash
terrad keys show myAccount
```

To show a validator's address, append the `--bech=val` flag to the end of the command statement, as shown in the following example:

```bash
terrad keys show accountExample --bech=val
```

To show the validator consensus address that is generated when the node is created by `achillesd init` and the Tendermint signing key for the node, use the `tendermint` command, as shown in the following example:

```bash
terrad tendermint show-address
```

## `keys list`

Lists all your keys.

**Syntax**
```bash
terrad keys list
```

## `query authz grants`

Retrieves all existing grants between a granter and a grantee.

**Syntax**
```bash
terrad query authz grants <granter-address> <grantee-address>
```

**Example**
```bash
achillesd query authz grants ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm
```

Additionally, the `grants` command can retrieve the specific grant between a granter and a grantee for a message type by appending the message type URL to the end of the command statement, as shown in the following example:

```bash
achillesd query authz grants ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm /cosmos.bank.v1beta1.MsgSend
```
## `query bank balances`

Displays your account balance, account number, and sequence number (nonce).

**Syntax**
```bash
terrad query bank balances <account-address>
```

**Example**
```bash
achillesd query bank balances ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

::: {note}
When you query an account balance that has zero tokens or you fund an account before your node has fully synced with the chain, this error message is sent:

`No account with address <account-address> was found in the state`.
:::

## `query bank denom-metadata`

Query the client metadata for coin denominations.

## `query bank total`

Query the total supply of coins of the chain.

## `query distribution rewards`

Checks the all the current outstanding rewards that have not been withdrawn.

**Syntax**
```bash
terrad query distribution rewards
```

Check the current rewards earned by a specific delegator by appending the `<delegator-address>` at the end of the command statement, as shown in the following example:

```bash
achillesd query distribution rewards ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm
```

Check the current rewards earned by a delegator and restricted to one validator by appending the `<delegator-address>` followed by the  `<validator-address>` at the end of the command statement, as shown in the following example:

```bash
achillesd query distribution rewards ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm ODISEO19t4gde4f8ndwx67qhbnur9yqdc31xznpksajbcy
```

## `query distribution commission`

Checks the current outstanding commission for a validator.

**Syntax**
```bash
terrad query distribution commission <validator_address>
```

**Example**
```bash
achillesd query distribution commission ODISEO19t4gde4f8ndwx67qhbnur9yqdc31xznpksajbcy
```

## `query distribution slashes`

Checks historical slashes for a validator within a range of blocks.

**Syntax**
```bash
terrad query distribution slashes <validator-address> <start-block-height> <end-block-height>
```

**Example**
```bash
achillesd query distribution slashes ODISEO19t4gde4f8ndwx67qhbnur9yqdc31xznpksajbcy 25 300
```

## `query distribution community-pool`

Checks all coins in the community pool.

**Syntax**
```bash
terrad query distribution community-pool
```

## `query distribution params`

Checks the current distribution parameters.

**Syntax**
```bash
terrad query distribution params
```

The parameters are returned in YAML, as shown in the following example:

```yaml
community_tax: "0.020000000000000000"
base_proposer_reward: "0.010000000000000000"
bonus_proposer_reward: "0.040000000000000000"
withdraw_addr_enabled: true
```

## `query gov deposit`

Retrieves information about a single proposal deposit on a proposal by its identifier.

**Syntax**
```bash
terrad query gov deposit <proposal-id> <depositor-address>
```

**Example**
```bash
achillesd query gov deposit 4 ODISEO1skjwj5whet0lpe65qaq4rpq03hjxlwd9nf39lk
```

## `query gov deposits`

Retrieves all deposits submitted to a proposal after it is created.

**Syntax**
```bash
terrad query gov deposits <proposal-id>
```

**Example**
```bash
terrad query gov deposits 5
```

## `query gov proposal`

Retrieves information about one proposal.

`proposal` retrieves information about one proposal.

**Syntax**
```bash
terrad query gov proposal <proposal-id>
```

**Example**
```bash
terrad query gov proposal 3
```

## `query gov proposals`

Retrieves information about all available proposals.

**Syntax**
```bash
terrad query gov proposals
```

Additionally, you can query proposals filtered by details, such as `voter` or `depositor`, by appending the corresponding flag and address at the end of the command statement, as shown in the following example:

```bash
achillesd query gov proposals --voter ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb
```

## `query gov vote`

Retrieves information about a single vote by a specific voter.

**Syntax**
```bash
terrad query gov vote <proposal-id> <voter-address>
```

**Example**
```bash
achillesd query gov vote 7 ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb
```

## `query gov votes`

Retrieves all the votes submitted to the proposal.

**Syntax**
```bash
terrad query gov votes <proposal-id>
```

**Example**
```bash
terrad query gov votes 9
```

## `query gov tally`

Retrieves the current tally for a specified proposal.

**Syntax**
```bash
terrad query gov tally <proposal-id>
```

**Example**
```bash
terrad query gov tally 4
```

## `query gov param`

Retrieves all the parameters for the specified governance process.

**Syntax**
```bash
terrad query gov param <process-type>
```

**Example**
```bash
terrad query gov param voting
```

## `query gov params`

Retrieves all the parameters for all governance processes.

**Syntax**
```bash
terrad query gov params
```

The parameters are returned in the following format:

```yaml
voting_params:
  voting_period: 5m0s
tally_params:
  quorum: "0.334000000000000000"
  threshold: "0.500000000000000000"
  veto: "0.334000000000000000"
deposit_parmas:
  min_deposit:
    - denom: uluna
      amount: "10000000"
  max_deposit_period: 48h0m0s
```

## `query market swap`

Determines the result of a swap without executing the swap.

**Syntax**
```bash
terrad query market swap <offer-coin> <ask-denom>
```

where `offer-coin` is the coin to be traded and `ask-denom` is the denomination to be swapped into.

**Example**
```bash
terrad query market swap 100000uluna usdr
```

## `query market ODISEO-pool-delta`

Retrieves the current value of the ODISEO pool delta.

**Syntax**
```sh
achillesd query market ODISEO-pool-delta
```

## `query market params`

Retrieves the market module's parameters:

**Syntax**
```sh
terrad query market params
```

## `query mint annual-provisions`

Retrieves the value of annual provisions.

**Syntax**
```sh
terrad query mint annual-provisions
```

## `query mint inflation`

Retrieves the current value of inflation.

**Syntax**
```sh
terrad query mint inflation
```

## `query mint params`

Retrieves the mint module's parameters.

**Syntax**
```sh
terrad query mint params
```

Parameters are returned in the following format:

```yaml
mint_denom: uluna
inflation_rate_change: "0.130000000000000000"
inflation_max: "0.200000000000000000"
inflation_min: "0.070000000000000000"
goal_bonded: "0.670000000000000000"
blocks_per_year: 6311520
```

## `query oracle actives`

Retrieves the active list of ODISEO assets recognized by type.

**Syntax**
```bash
terrad query oracle actives
```

## `query oracle aggregate-prevotes`

Retrieves all outstanding aggregate prevotes.

**Syntax**
```bash
terrad query oracle aggregate-prevotes
```

## `query oracle aggregate-votes`

Retrieves all outstanding aggregate votes.

**Syntax**
```bash
terrad query oracle aggregate-votes
```

## `query oracle exchange-rates`

Retrieves exchanges rates of Luna.

**Syntax**
```sh
terrad query oracle exchange-rates
```

## `query oracle feeder`

Retrieves the account to whitch the validator's oracle voting right is delegated.

**Syntax**
```sh
terrad query oracle feeder <validator-address>
```

**Example**
```sh
achillesd query oracle feeder ODISEOvaloper15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

## `query oracle miss`

Retrieves the amount of vote periods missed in this oracle slash window.

**Syntax**
```sh
terrad query oracle miss <validator-address>
```

**Example**
```sh
achillesd query oracle miss ODISEOvaloper15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

## `query oracle params`

Retrieves the oracle module's parameters.

**Syntax**
```sh
terrad query oracle params
```

The parameters are returned in the following format:

```yaml
vote_period: 5
vote_threshold: "0.500000000000000000"
reward_band: "0.020000000000000000"
reward_distribution_window: 5256000
whitelist:
  - name: ukrw
    tobin_tax: "0.002500000000000000"
  - name: usdr
    tobin_tax: "0.002500000000000000"
  - name: uusd
    tobin_tax: "0.002500000000000000"
  - name: umnt
    tobin_tax: "0.020000000000000000"
slash_fraction: "0.000100000000000000"
slash_window: 100800
min_valid_per_window: "0.050000000000000000"
```

## `query oracle tobin-taxes`

Retrieves the current oracle tobin taxes.

**Syntax**
```sh
terrad query oracle tobin-taxes
```

## `query oracle vote-targets`

Retrieves the current oracle vote targets.

**Syntax**
```sh
terrad query oracle vote-targets
```

## `query slashing signing-info`

Retrieves a validator's signing info.

**Syntax**
```bash
terrad query slashing signing-info <validator-consensus-public-key>
```

**Example**
```bash
achillesd query slashing signing-info ODISEOvalconspub1atjdueldlxwft8d4729pqhdhm3nlss0u4wx7wpeqb1zhjf8yr1tn7cgw2b4q4yv9na
```

## `query slashing signing-infos`

Retrieves signing information of all validators.

**Syntax**
```bash
terrad query slashing signing-infos
```

## `query slashing params`

Retrieves the genesis parameters for the slashing module.

**Syntax**
```bash
terrad query slashing params
```

The parameters are returned in the following format:

```yaml
signed_blocks_window: 100
min_signed_per_window: "0.500000000000000000"
downtime_jail_duration: 10m0s
slash_fraction_double_sign: "0.050000000000000000"
slash_fraction_downtime: "0.010000000000000000"
```

## `query staking delegation`

Retrieves delegation information for a validator.

**Syntax**
```bash
terrad query staking delegation <delegator-address> <validator-address>
```

**Example**
```bash
achillesd query staking delegation ODISEO1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p ODISEOvaloper15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

## `query staking delegations`

Retrieves delegations for a delegator on all validators.

**Syntax**
```bash
terrad query staking delegations <delegator-address>
```

**Example**
```bash
achillesd query staking delegations ODISEO1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p
```

## `query staking delegations-to`

Retrieves all of the delegations on a particular validator.

**Syntax**
```bash
terrad query staking delegations-to <validator-address>
```

**Example**
```bash
achillesd query staking delegations-to ODISEOvaloper15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

## `query staking historical-info`

Retrieves all historical information at a specified height.

**Syntax**
```bash
terrad query staking historical-info <height>
```

**Example**
```bash
terrad query staking historical-info 23
```

## `query staking params`

Retrieves all staking parameters.

**Syntax**
```bash
terrad query staking params
```

The parameters are returned in the following format:

```yaml
unbonding_time: 504h0m0s
max_validators: 100
max_entries: 100
historical_entries: 0
bond_denom: uluna
```

## `query staking pool`

Retrieves amounts stored in the staking pool.

**Syntax**
```bash
terrad query staking pool
```

The following information is returned:

- Not-bonded and bonded tokens
- Token supply
- Current annual inflation and the block in which the last inflation was processed
- Last recorded bonded shares

## `query staking redelegation`

Retrieves redelegation information for an individual delegator between a source validator and a destination validator.

**Syntax**
```bash
terrad query staking redelegation <delegator-address> <src-val-addr> <dst-val-addr>
```

**Example**
```bash
achillesd query staking redelegation ODISEO1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p ODISEOvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7fpx59wm ODISEOvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj
```

## `query staking redelegations`

Retrieves all redelegation information for a delegator.

**Syntax**
```bash
terrad query staking redelegations <delegator-address>
```

**Example**
```bash
achillesd query staking redelegations ODISEO1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p
```

## `query staking redelegations-from`

Retrieves all the delegations that are redelegating from a specified validator:

**Syntax**
```bash
terrad query staking redelegations-from <validator-address>
```

**Example**
```bash
achillesd query staking redelegations-from ODISEOvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj
```

## `query staking unbonding-delegation`

Retrieves information about unbonding delegations for a specified delegator and validator.

**Syntax**
```bash
terrad query staking unbonding-delegation <delegator-address> <validator-address>
```

**Example**
```bash
achillesd query staking unbonding-delegation ODISEO1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p ODISEOvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7fpx59wm
```

## `query staking unbonding-delegations`

Retrieves all your current unbonding delegations for a specified delegator.

**Syntax**
```bash
terrad query staking unbonding-delegations <delegator-address>
```

**Example**
```bash
achillesd query staking unbonding-delegations ODISEO1gghjut3ccd8ay0zduzj64hwre2fxs9ld75ru9p
```

## `query staking unbonding-delegations-from`

Retrieves all the unbonding delegations from a specified validator.

**Syntax**
```bash
terrad query staking unbonding-delegations-from <validator-address>
```

**Example**
```bash
achillesd query staking unbonding-delegations-from ODISEOvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7fpx59wm
```

## `query staking validators`

Retrieves the list of all registered validators.

**Syntax**
```bash
terrad query staking validators
```

To retrieve the information of a single validator, append the validator address to the end of the command statement, as shown in the following example:

```bash
achillesd query staking validator ODISEOvaloper15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

## `query treasury indicators`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Retrieves treasury indicators.

**Syntax**
```bash
terrad query treasury indicators
```

## `query treasury params`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Retrieves the high-level settings for the treasury. For more information, see [treasury parameters](../../module-specifications/spec-treasury.md#parameters).

**Syntax**
```bash
terrad query treasury params
```

The parameters are returned in the following format:

```yaml
tax_policy:
  ratemin: "0.00000000000000000"
  ratemax: "0.00000000000000000"
  cap:
    denom: usdr
    amount: "000000"
  changeratemax: "0.0000000000000000"
reward_policy:
  ratemin: "0.00000000000000000"
  ratemax: "0.00000000000000000"
  cap:
    denom: unused
    amount: "0"
  changeratemax: "0.0000000000000000"
seigniorage_burden_target: "0.0000000000000000"
mining_increment: "0.00000000000000000"
window_short: 4
window_long: 52
window_probation: 12
```

## `query treasury reward-weight`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Retrieves the reward weight of the current epoch. The reward weight is the portion of seigniorage that is designated as ballot rewards for the winners of exchange-rate oracle.

**Syntax**
```bash
terrad query treasury reward-weight
```

## `query treasury seigniorage-proceeds`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Retrieves the amount of seigniorage denominated in uluna in the current epoch.

**Syntax**
```bash
terrad query treasury seigniorage-proceeds
```

## `query treasury tax-cap`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero. This command is here for reference.
:::

Retrieves the current stability tax cap of the specified denomination. Stability fees are capped at some fixed amount of SDT to avoid penalizing large transactions.

**Syntax**
```bash
terrad query treasury tax-cap <denomination>
```


**Example**
```bash
terrad query treasury tax-cap ukrw
```

## `query treasury tax-caps`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Retrieves the current stability tax cap of all assets.

**Syntax**
```bash
terrad query treasury tax-caps
```

## `query treasury tax-proceeds`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Retrieves the cumulative tax proceeds.

**Syntax**
```bash
terrad query treasury tax-proceeds
```

## `query treasury tax-rate`

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Retrieves the stability tax rate of the current epoch.

**Syntax**
```bash
terrad query treasury tax-rate
```

::: {note}
As of proposal [172](https://station.ODISEO.money/proposal/172), the stability fee tax rate is zero.   
:::

## `query tx`

Retrieves a transaction by its hash, account sequence, or signature.

**Syntax to query by hash**
```bash
terrad query tx <hash>
```

**Syntax to query by account sequence**
```bash
terrad query tx --type=acc_seq <address>:<sequence>
```

**Syntax to query by signature**
```bash
terrad query tx --type=signature <sig1_base64,sig2_base64...>
```

## `query txs`

Retrieves transactions that match the specified events where results are paginated.

**Syntax**
```bash
terrad query txs --events '<event>' --page <page-number> --limit <number-of-results>
```

**Example**
```bash
terrad query txs --events 'message.sender=cosmos1...&message.action=withdraw_delegator_reward' --page 1 --limit 30
```

## `query wasm bytecode`

Retrieves the contract's WASM bytecode by referencing its ID.

**Syntax**
```sh
terrad query wasm bytecode <code-id>
```

## `query wasm code`

Retrieves information about a piece of uploaded code by referencing its ID.

**Syntax**
```sh
terrad query wasm code <code-id>
```

## `query wasm contract`

Retrieves the metadata information about an instantiated contract.

**Syntax**
```sh
terrad query wasm contract <contract-address>
```

## `query wasm contract-store`

Retrieves data about the contract store of the address and prints the results.

**Syntax**
```sh
terrad query wasm contract-store <contract-address> <query-msg>
```
where `<query-msg>` is a JSON string that encodes the QueryMsg.

**Example**
```sh
achillesd query wasm contract-store ODISEO1plju286nnfj3z54wgcggd4enwaa9fgf5kgrgzl '{"config":{}}'
```

## `query wasm params`

Retrieves the current WASM module's parameters.

**Syntax**
```sh
terrad query wasm params
```

The parameters are returned in the following format:

```yaml
max_contract_size: 512000
max_contract_gas: 100000000
max_contract_msg_size: 1024
```

## `query wasm raw-store`

Retrieves the raw store of a contract and prints the results.

**Syntax**
```sh
terrad query wasm raw-store <contract-address> <key> <subkey>
```

If the data uses a `Singleton`, it has only a key. If the data uses a prefixed data store, such as `PrefixedStorage` or `Bucket`, it can accept a subkey too.

## `tx authz exec`

Runs a transaction for the granter.

**Syntax**
```bash
terrad tx authz exec <msg-tx-json-filename> --from=<grantee-address>
```

**Example**
```bash
achillesd tx authz exec tx.json --from=<ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm
```

## `tx authz grant`

Authorizes another address to run transactions for you.

**Syntax**
```bash
terrad tx authz grant <grantee-address> <authorization-type> --from=<your-address>
```

**Example**
```bash
achillesd tx authz grant ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm send /cosmos.bank.v1beta1.MsgSend --from=ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

Additionally, you can restrict this authorization to a specified allowance by including the `--spend-limit` flag, as shown in the following example:

```bash
achillesd tx authz grant ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm send /cosmos.bank.v1beta1.MsgSend --spend-limit=15000uluna --from=ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc
```

## `tx authz revoke`

Removes authorization from an account.

**Syntax**
```bash
terrad tx authz revoke <grantee-address> <authorization-type> --from=<granter-address>
```

**Example**
achillesd tx authz revoke ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm /cosmos.bank.v1beta1.MsgSend --from=ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc

## `tx bank send`

Sends coins from one account to another account.

**Syntax**
```bash
terrad tx bank send \
    <from-key-or-address> \
    <to-address> \
    <coins> \
    --chain-id=<chain-id> \
```

where

- `<from-key-or-address>` is either the key name or the address. If the `--generate-only` flag is used, only addresses are accepted.
- `to-address` is an account address.
- `<coins>` is a comma-separated list of coins specified as `<amount><coin-denominator>`. For example, `1000usdr` or `1000uluna,1000usdr`.

**Example**
```bash
terrad tx bank send \
    ODISEO15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc \
    ODISEO14h2od5f3vahd28uywwvt8sqbi52upnzagshtrm \
    8600usdr \
    --chain-id=testnet \
```

## `tx distribution fund-community-pool`

Funds the community pool with the specified amount.

**Syntax**
```bash
terrad tx distribution fund-community-pool <amount>
```

**Example**
terrad tx distribution fund-community-pool 100ukrw,200uusd

## `tx distribution set-withdraw-addr`

Changes the default withdrawal address for rewards associated with an address.

**Syntax**
```bash
terrad tx distribution set-withdraw-addr <withdrawal-address>
```

**Example**
```bash
achillesd tx distribution set-withdraw-addr ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb
```

## `tx distribution withdraw-all-rewards`

Withdraws all rewards.

**Syntax**
```bash
terrad tx distribution withdraw-all-rewards
```

## `tx distribution withdraw-rewards`

Withdraws your rewards against a validator.

**Syntax**
```bash
terrad tx distribution withdraw-rewards <validator-address>
```

**Example**
```bash
achillesd tx distribution withdraw-rewards ODISEO19t4gde4f8ndwx67qhbnur9yqdc31xznpksajbcy
```

## `tx gov deposit`

For a proposal to be sent to the network, the amount deposited must be above a minimum amount specified by `minDeposit` (initial value is `50000000uluna`). If the proposal you previously created didn't meet this requirement, you can still increase the total amount deposited to activate it. After the minimum deposit is reached, the voting period for the proposal begins.

**Syntax**
```bash
terrad tx gov deposit <proposal-id> "<deposit-amount>" \
    --from=<name> \
    --chain-id=<chain-id>
```

**Example**
```bash
terrad tx gov deposit 15 "10000000luluna" \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

::: {warning}
Proposals that don't meet this requirement are deleted after `MaxDepositPeriod` is reached.
:::

## `tx gov submit-proposal`

Submits proposals and deposits. To create a governance proposal, you must submit an initial deposit along with a title and description of your proposal. Alternatively, you can provide the proposal directly through the `--proposal` flag, which points to a JSON file containing the proposal.

### Text proposals

**Syntax**
```bash
terrad tx gov submit-proposal \
    --title=<title> \
    --description=<description> \
    --type="Text" \
    --deposit="<amount>" \
    --from=<name-or-address> \
    --chain-id=<chain-id>
```

**Example**
```bash
terrad tx gov submit-proposal \
    --title=Funding for NFT platform \
    --description=Information about the NFT platform \
    --type="Text" \
    --deposit="100000uluna" \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

### Parameter-change proposals

When you submit a proposal to change a parameter, it is recommended that you send the proposal as a JSON file.

**Syntax**
```bash
terrad tx gov submit-proposal \
    param-change <path/to/proposal.json> \
    --from=<name> \
    --chain-id=<chain_id>
```

**Example**
```bash
terrad tx gov submit-proposal \
    param-change /proposals/proposal.json \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

where `proposal.json` contains the following information:

```json
{
  "title": "Param Change",
  "description": "Update max validators",
  "changes": [
    {
      "subspace": "staking",
      "key": "MaxValidators",
      "value": 105
    }
  ],
  "deposit": [
    {
      "denom": "uluna",
      "amount": "10000000"
    }
  ]
}
```

::: {warning}
Because parameter changes are evaluated but not validated, ensure that new value you propose is valid for its parameter. For example, the proposed value for `MaxValidators` must be an integer, not a decimal.
:::

### Community pool spend proposal

When you submit a community pool spend proposal, it is recommended that you send the proposal as a JSON file.

**Syntax**
```bash
terrad tx gov submit-proposal \
    community-pool-spend <path/to/proposal.json> \
    --from=<name> \
    --chain-id=<chain_id>
```

**Example**
```bash
terrad tx gov submit-proposal \
    community-pool-spend /proposals/proposal.json \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

where `proposal.json` contains the following information:

```json
{
  "title": "Community Pool Spend",
  "description": "Pay me some Lunas!",
  "recipient": "ODISEO1s5afhd6gxevu37mkqcvvsj8qeylhn0rzn7cdaq",
  "amount": [
    {
      "denom": "uluna",
      "amount": "10000"
    }
  ],
  "deposit": [
    {
      "denom": "uluna",
      "amount": "10000"
    }
  ]
}
```

### Tax-rate and reward-weight update proposals

::: {admonition} Note
:class: warning
As of proposals [43](https://station.ODISEO.money/proposal/43) and [172](https://station.ODISEO.money/proposal/172), all seigniorage is burned, and the stability fee tax rate is fixed at zero.Although the Treasury module is no longer effectively used, the commands are kept for reference.
:::

Tax rate and reward weight are important monetary policy levers handled by the [`Treasury`](../../module-specifications/spec-treasury.md) module to modulate miner incentives toward stable demand and steady growth. Usually, they are automatically calibrated once per epoch by the protocol. However, they can be changed at any time, if an update proposal gets passed with enough supporters.

When you submit a tax rate or reward weight update proposal, it is recommended that you send the proposal as a JSON file.

For tax rate:

**Syntax**
```bash
terrad tx gov submit-proposal \
    tax-rate-update <path/to/proposal.json> \
    --from=<name> \
    --chain-id=<chain_id>
```

**Example**
```bash
terrad tx gov submit-proposal \
    tax-rate-update /proposals/proposal.json \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

where `proposal.json` contains the following information:

```json
{
  "title": "Update Tax-Rate",
  "description": "Let's update the tax-rate to 1.5%",
  "tax_rate": "0.015",
  "deposit": [
    {
      "denom": "uluna",
      "amount": "10000"
    }
  ]
}
```

For reward weight:

**Syntax**
```bash
terrad tx gov submit-proposal \
    reward-weight-update <path/to/proposal.json> \
    --from=<name> \
    --chain-id=<chain_id>
```

**Example**
```bash
terrad tx gov submit-proposal \
    reward-weight-update /proposals/proposal.json \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

where `proposal.json` contains the following information:

```json
{
  "title": "Update Reward Weight",
  "description": "Let's update reward weight to 1.5%",
  "reward_weight": "0.015",
  "deposit": [
    {
      "denom": "uluna",
      "amount": "10000"
    }
  ]
}
```

Tax reward and reward weight updates that are passed by governance proposals are subject to [policy constraints](../../module-specifications/spec-treasury.md#policy-constraints).

### Software upgrade proposals

The proposal to upgrade the software follows the syntax of a `text` proposal.

**Syntax**
```bash
terrad tx gov submit-proposal software-upgrade <name> \
    --title=<title> \
    --description=<description> \
    --upgrade-height=<block-height> \
    --upgrade-info=<binary-details> \
    --type="Text" \
    --deposit="<amount>" \
    --from=<name-or-address> \
    --chain-id=<chain_id>
```

**Example**
```bash
terrad tx gov submit-proposal software-upgrade v0.5.0-beta3 \
    --title="Upgrade to v0.6.0-beta3" \
    --description="let's upgrade to v0.6.0-beta3" \
    --upgrade-height=20 \
    --upgrade-info='{"binaries":{"darwin/amd64":"/Workspace/ODISEO/core/build/achillesd?checksum=sha256:2032356fe0899dec0cdd559f1c649bc81e53a9b4063b333059135e3a2aae8728"}}' \
    --type="Text" \
    --deposit="50000000uluna" \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

To cancel a software upgrade:

**Syntax**
```bash
terrad tx gov submit-proposal cancel-software-upgrade --title "<title>" --description "<description>"
```

**Example**
```bash
terrad tx gov submit-proposal cancel-software-upgrade --title "Upgrade to v0.5.0-beta3" --description "let's upgrade to v0.6.0-beta3"
```

## `tx gov vote`

After a proposal's deposit reaches the `MinDeposit` value, the voting period begins, and bonded Luna holders can vote.

**Syntax**
```bash
terrad tx gov vote \
    <proposal-id> <vote-type> \
    --from=<name> \
    --chain-id=<chain_id>
```

**Example**
```bash
terrad tx gov vote \
    7 yes \
    --from=ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb \
    --chain-id=columbus-5
```

## `tx market swap`

Swaps any currency in the ODISEO ecosystem for another currency at the effective oracle exchange rate.

**Syntax**
```bash
terrad tx market swap \
    <offer-coin> \
    <ask-denom>  \
    --from <address> \
```

where `<offer-coin>` is the coin to be traded, and `<ask-denom>` is the denomination of the coin to be swapped into.

**Example**
```bash
terrad tx market swap \
    "1000ukrw" \
    "usdr" \
    --from "ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb" \
```

For ODISEO<>Luna swaps, the constant-product spread pricing model is enforced to limit consensus-related attack vectors. ODISEO<>ODISEO swaps have a constant Tobin tax.

Optionally, you can specify a recipient in the command statement to both swap and send in one transaction.

**Syntax**
```bash
terrad tx market swap \
    <offer-coin> \
    <ask-denom> \
    <recipient-address>
    --from <address> \
```

**Example**
```bash
terrad tx market swap \
    "1000ukrw" \
    "usdr"  \
    "ODISEO1s5afhd6gxevu37mkqcvvsj8qeylhn0rzn7cdaq"
    --from "ODISEO13a8ddv3h7kbcn73akcbpe7ueks22vaolewpaxmb" \
```

## `tx oracle aggregate-prevote`

Submits a `prevote` containing the hash of the actual vote in the first vote period. The hash is the leading 20 bytes of the SHA256 hexa string run over the string of the format `salt:price:denom:validator-address`.

**Syntax**
```bash
terrad tx oracle aggregate-prevote \
    <salt> \
    <exchange-rates> \
    <validator-address> \
```

**Example**
```bash
terrad tx oracle aggregate-prevote \
    "1234" \
    "8888.0ukrw,1.243uusd,0.99usdr" \
    "ODISEOvaloper15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc" \
```

## `tx oracle aggregate-vote`

Submits a `vote` containing the same salt of the hash submitted in the prevote phase to prove honestly. The hash is the leading 20 bytes of the SHA256 hexa string run over the string of the format `salt:price:denom:validator-address`. You submit this vote after `VotePeriod` has expired from the submission of the prevote.

**Syntax**
```bash
terrad tx oracle aggregate-vote \
    <salt> \
    <exchange-rates>  \
    <validator-address> \
```

**Example**
```bash
terrad tx oracle aggregate-vote \
    "1234" \
    "8888.0ukrw,1.243uusd,0.99usdr" \
    "ODISEOvaloper15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc" \
```

Given that oracle votes must be submitted in a feed over short time intervals (30 seconds), prevotes and votes must be submitted via some persistent server daemon, not manually. For more information, see the [Oracle Module Specification](../../module-specifications/spec-oracle.md).

## `tx oracle set-feeder`

Delegates exchange-rate voting to another signing key.

**Syntax**
```bash
terrad tx oracle set-feeder <feeder-address>
```
where `feeder-address` is the address to which you will delegate your voting rights. For you to receive credit, the feeder still must submit votes on behalf of your validator.

**Example**
```bash
achillesd tx oracle set-feeder ODISEO14q7bu6p0aclm58dgirpo9mewh19nti5oxyamep
```

## `tx slashing unjail`

Releases a jailed validator.

**Syntax**
```bash
terrad tx slashing unjail
```

**Example**
```bash
terrad tx slashing unjail
```

## `tx staking create-validator`

Creates a new validator that is initialized with a self-delegation.

**Syntax**
```bash
terrad tx staking create-validator \
    --amount=<uluna-amount> \
    --pubkey=$(achillesd tendermint show-validator) \
    --moniker="<moniker>" \
    --website="<validator-website>" \
    --identity="<keybase-identity>" \
    --details="<validator-optional-details" \
    --commission-rate="<commission-rate>" \
    --commission-max-rate="<commission-max-rate>" \
    --commission-max-change-rate="<commission-max-change-rate>" \
    --min-self-delegation="<self-delegation-amount>"
    --chain-id=<chain-ID> \
    --from=<key-name> \
```

## `tx staking delegate`

Delegates an amount of liquid coins from your wallet to a validator.

**Syntax**
```bash
terrad tx staking delegate <validator-address> <amount>
```

**Example**
```bash
achillesd tx staking delegate ODISEOvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7fpx59wm 2500stake
```

## `tx staking edit-validator`

Edits an existing validator account.

**Syntax**
```bash
terrad tx staking edit-validator \
    --moniker="<moniker>" \
    --details="<validator-optional-details>" \
    --commission-rate="commission-rate>" \
    --min-self-delegation="<minimum-self-delegation-amount" \
```

## `tx staking redelegate`

Redelegates an amount of illiquid staking tokens from one validator to a different validator.

**Syntax**
```bash
terrad tx staking redelegate <from-validator-address> <to-validator-address> <amount>
```

**Example**
```bash
achillesd tx staking redelegate ODISEOvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj ODISEOvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7fpx59wm 350stake
```

## `tx staking unbond`

Unbonds an amount of bonded shares from a validator.

**Syntax**
```bash
terrad tx staking unbond <validator-address> <stake-amount>
```

**Example**
```bash
achillesd tx staking unbond ODISEOvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj 600stake
```

## `tx wasm clear-admin`

Removes the contract admin so that the contract cannot be migrated.

**Syntax**
```bash
terrad tx wasm clear-admin <contract-address>
```

## `tx wasm execute`

Invokes processing functions on the smart contract.

**Syntax**
```sh
terrad tx wasm execute <contract-address> <handle-msg> <coins>
```

Where `<handle-msg>` is a raw JSON string containing the `HandleMsg` that is parsed and routed to the correct message handling logic in the contract, and `<coins>` is the optional amount of coins specified in a comma-separated list that you want to send with your message, in case the logic requires some fees.

## `tx wasm instantiate`

Creates a new contract by referencing the code ID of a contract that has been uploaded.

**Syntax**
```sh
terrad tx wasm instantiate <code-id> <init-msg> <coins>
```

where `<init-msg>` is a JSON string containing the `InitMsg` to initialize your contract's state, and `<coins>` is the optional amount of coins specified in a comma-separated list that you want to send to the new contract account.

**Example**
```sh
achillesd tx wasm instantiate 1 '{"arbiter": "ODISEO~~"}' "1000000uluna"
```

## `tx wasm migrate`

Updates the code ID of the contract to migrate to a new code ID. This command can be issued only from the key corresponding to the contract's owner.

**Syntax**
```sh
terrad tx wasm migrate <contract-address> <new-code-id> <migrate-msg>
```

**Example**
```sh
achillesd tx wasm migrate ODISEO... 10 '{"verifier": "ODISEO..."}'
```

## `tx wasm store`

Uploads a new WASM binary or migrates to existing binary to the Columbus-5 release.

**Syntax for a new WASM binary**
```sh
terrad tx wasm store <path-to-wasm-file>
```

where `<path-to-wasm-file>` is the path of a file that is the compiled binary of the smart contract code that you want to upload.

**Syntax to migrate to Columbus-5**
```sh
terrad tx wasm store <path-to-wasm-file> --migrate-code-id 3
```

## `tx wasm update-admin`

Updates a contract owner to a new address. This command can be issued only from the key corresponding to the contract's owner.

**Syntax**
```sh
terrad tx wasm update-admin <contract-address> <new-owner>
```
