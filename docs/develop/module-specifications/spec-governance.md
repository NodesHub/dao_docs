# Governance <img src="/img/Governance.svg" height="40px">

:::{Important}
ODISEO's Governance module inherits from Cosmos SDK's [`gov`](https://docs.cosmos.network/master/modules/gov/) module. This document is a stub, and covers mainly important ODISEO-specific notes about how it is used.
:::

Governance is the process through which members of the DAODISEO community can effect change on the protocol by submitting petitions known as "proposals" and arriving at a popular consensus when a threshold amount of support has been reached. The proposal structure is versatile and allows for holders of staked Luna (those who have an interest in the long-term viability of the network) to voice their opinion on both blockchain parameter updates as well as the future development of the ODISEO protocol.

Check the [Governance section of the `achillesd` Reference](../how-to/achillesd/subcommands.md#tx-gov-submit-proposal) to see examples of how to participate in the Governance process.

To learn how to vote with your staked Luna or submit proposals, visit the [ODISEO Station governance guide](../../learn/ODISEO-station/governance.md).

## Concepts

The following is the governance proposal procedure:

### Deposit Period

After a proposal is submitted, it enters the deposit period, where it must reach a total minimum deposit of 50 Luna within 2 weeks from the time of its submission. The deposit threshold is reached when the sum of the initial deposit (from the proposer) and the deposits from all other interested network participants meet or exceed 50 Luna.

Deposits protect against unnecessary proposals and spam.

Deposits get refunded if all of the following conditions are met:
- The minimum deposit of 50 Luna is reached within the two-week deposit period.
- `Quorum` is met: the number of total votes is greater than 40% of all staked Luna
- The total number of `NoWithVeto` votes is less than 33.4% of the total vote.
- A vote returns a majority of `Yes` or `No` votes.

Deposits are burned under any of the following conditions:
- The minimum deposit of 50 Luna is not reached within the two-week deposit period.
- `Quorum` is not met: the number of total votes after the one-week voting period is less than 40% of all staked Luna.
- the number of `NoWithVeto` votes is above 33.4% of the total vote.

### Voting Period

If the minimum deposit has been reached before the end of the deposit period, then the proposal goes into a one-week voting period. While the proposal is in voting, holders of staked Luna can cast votes for the proposal. The 4 voting options available are:

- `Yes` - in favor
- `No` - not in favor
- `NoWithVeto` - veto
- `Abstain` - does not influence vote

Voting is done by holders of bonded LUNA on a 1 bonded LUNA = 1 vote basis. As such, validators hold the most influence over the outcome of voting, and delegators by default inherit the vote of their validator if they don't vote.

### Tallying

For a proposal to pass, the following conditions must be met:

1. Voter participation must be at least `quorum` $Q$:

$$\frac{Yes + No + NoWithVeto}{Stake} \ge Q$$

2. The ratio of `NoWithVeto` votes must be less than `veto` $V$:

$$\frac{NoWithVeto}{Yes + No + NoWithVeto} \lt V$$

3. The ratio of `Yes` votes must be greater than `threshold` $T$:

$$\frac{Yes}{Yes + No + NoWithVeto} \gt T$$

If any of the previous conditions are not met, the proposal is rejected. Proposals that get rejected with veto do not get their deposits refunded. The parameters `quorum`, `veto`, and `threshold` exist as blockchain parameters within the Governance module.

::: {warning}
Deposits will not be refunded for proposals that are rejected with veto, do not meet quorum, or fail to reach the minimum deposit during the deposit period. Non-refunded deposits are burned.
:::

### Proposal Implementation

Once a governance proposal passes, the changes described are put into effect by the proposal handler. Generic proposals such as a `TextProposal` must be reviewed by ODISEO protocol developers and the community for decisions on how to manually implement.

Although parameter changes get updated immediately, they generally are not put into effect until the next epoch operation. Epochs occur every 100800 blocks or roughly every 7.7 days, given a 6.6-second block time.


## Data

### Proposal

```go
type Proposal struct {
	Content `json:"content" yaml:"content"` // Proposal content interface

	ProposalID       uint64         `json:"id" yaml:"id"`                                 //  ID of the proposal
	Status           ProposalStatus `json:"proposal_status" yaml:"proposal_status"`       // Status of the Proposal {Pending, Active, Passed, Rejected}
	FinalTallyResult TallyResult    `json:"final_tally_result" yaml:"final_tally_result"` // Result of Tallys

	SubmitTime     time.Time `json:"submit_time" yaml:"submit_time"`           // Time of the block where TxGovSubmitProposal was included
	DepositEndTime time.Time `json:"deposit_end_time" yaml:"deposit_end_time"` // Time that the Proposal would expire if deposit amount isn't met
	TotalDeposit   sdk.Coins `json:"total_deposit" yaml:"total_deposit"`       // Current deposit on this proposal. Initial value is set at InitialDeposit

	VotingStartTime time.Time `json:"voting_start_time" yaml:"voting_start_time"` // Time of the block where MinDeposit was reached. -1 if MinDeposit is not reached
	VotingEndTime   time.Time `json:"voting_end_time" yaml:"voting_end_time"`     // Time that the VotingPeriod for this proposal will end and votes will be tallied
}

```

A `Proposal` is a data structure representing a petition for a change that is submitted to the blockchain alongside a deposit. Once its deposit reaches a certain [`MinDeposit`](#mindeposit), the proposal is confirmed and voting opens. Bonded Luna holders can then send `TxGovVote` transactions to vote on the proposal. ODISEO currently follows a simple voting scheme of 1 Bonded Luna = 1 Vote.

The `Content` of a proposal is the interface that contains the information about the `Proposal`, such as the `title`, `description`, and any notable changes. A `Content` type can be implemented by any module. The `ProposalRoute` of the `Content` returns a string which must be used to route the handler of the `Content` in the Governance keeper. This process allows the governance keeper to execute proposal logic implemented by any module. If a proposal passes, the handler is executed. Only if the handler is successful does the state get persisted and the proposal finally passes. Otherwise, the proposal is rejected.

## Message Types

### MsgSubmitProposal

```go
type MsgSubmitProposal struct {
	Content        Content        `json:"content" yaml:"content"`
	InitialDeposit sdk.Coins      `json:"initial_deposit" yaml:"initial_deposit"` //  Initial deposit paid by sender. Must be strictly positive
	Proposer       sdk.AccAddress `json:"proposer" yaml:"proposer"`               //  Address of the proposer
}
```

### MsgDeposit

```go
type MsgDeposit struct {
	ProposalID uint64         `json:"proposal_id" yaml:"proposal_id"` // ID of the proposal
	Depositor  sdk.AccAddress `json:"depositor" yaml:"depositor"`     // Address of the depositor
	Amount     sdk.Coins      `json:"amount" yaml:"amount"`           // Coins to add to the proposal's deposit
}
```

### MsgVote

```go
type MsgVote struct {
	ProposalID uint64         `json:"proposal_id" yaml:"proposal_id"` // ID of the proposal
	Voter      sdk.AccAddress `json:"voter" yaml:"voter"`             //  address of the voter
	Option     VoteOption     `json:"option" yaml:"option"`           //  option from OptionSet chosen by the voter
}
```

## Proposals

### Text Proposal

```go
type TextProposal struct {
	Title       string `json:"title" yaml:"title"`
	Description string `json:"description" yaml:"description"`
}
```

Text Proposals are used to create general-purpose petitions, such as asking core developers or community members to implement a specific feature. The community can reference a passed Text Proposal to the core developers or community members to indicate that a feature that potentially requires a soft or hard fork is in significant demand.

### Parameter Change Proposals

```go
type ParameterChangeProposal struct {
	Title       string        `json:"title" yaml:"title"`
	Description string        `json:"description" yaml:"description"`
	Changes     []ParamChange `json:"changes" yaml:"changes"`
}

type ParamChange struct {
	Subspace string `json:"subspace" yaml:"subspace"`
	Key      string `json:"key" yaml:"key"`
	Subkey   string `json:"subkey,omitempty" yaml:"subkey,omitempty"`
	Value    string `json:"value" yaml:"value"`
}
```

::: {important}
Parameter Change Proposals are actually located in the Params module, an internal module. It is shown here for your convenience.
:::

Parameter Change Proposals are a special type of proposal which, once passed, will automatically go into effect by directly altering the network's specified parameter.

### Software Upgrade Proposals

::: {danger}
Software upgrade proposals exist because they are inherited from the Cosmos SDK, but they are temporarily unavailable because they have not been implemented yet. Therefore, they share the same semantics as a simple text proposal. If you submit this type of proposal, you might lose your Luna deposit.
:::

## Transitions

### End-Block

> This section was taken from the official Cosmos SDK docs and placed here for your convenience to understand the Governance process.

`ProposalProcessingQueue` is a queue (`queue[proposalID]`) containing all the `ProposalID`s of proposals that reach `MinDeposit`. At the end of each block, all the proposals that have reached the end of their voting period are processed. To process a finished proposal, the application tallies the votes, computes the votes of each validator, and checks if every validator in the validator set has voted. If the proposal is accepted, deposits are refunded. Finally, the proposal content `Handler` is executed.

```GO
for finishedProposalID in GetAllFinishedProposalIDs(block.Time)
	proposal = load(Governance, <proposalID|'proposal'>) // proposal is a const key

	validators = Keeper.getAllValidators()
	tmpValMap := map(sdk.AccAddress)ValidatorGovInfo

	// Initiate mapping at 0. This is the amount of shares of the validator's vote that will be overridden by their delegator's votes
	for each validator in validators
	tmpValMap(validator.OperatorAddr).Minus = 0

	// Tally
	voterIterator = rangeQuery(Governance, <proposalID|'addresses'>) //return all the addresses that voted on the proposal
	for each (voterAddress, vote) in voterIterator
	delegations = stakingKeeper.getDelegations(voterAddress) // get all delegations for current voter

	for each delegation in delegations
		// make sure delegation.Shares does NOT include shares being unbonded
		tmpValMap(delegation.ValidatorAddr).Minus += delegation.Shares
		proposal.updateTally(vote, delegation.Shares)

	_, isVal = stakingKeeper.getValidator(voterAddress)
	if (isVal)
		tmpValMap(voterAddress).Vote = vote

	tallyingParam = load(GlobalParams, 'TallyingParam')

	// Update tally if validator voted they voted
	for each validator in validators
	if tmpValMap(validator).HasVoted
		proposal.updateTally(tmpValMap(validator).Vote, (validator.TotalShares - tmpValMap(validator).Minus))

	// Check if proposal is accepted or rejected
	totalNonAbstain := proposal.YesVotes + proposal.NoVotes + proposal.NoWithVetoVotes
	if (proposal.Votes.YesVotes/totalNonAbstain > tallyingParam.Threshold AND proposal.Votes.NoWithVetoVotes/totalNonAbstain  < tallyingParam.Veto)
	//  proposal was accepted at the end of the voting period
	//  refund deposits (non-voters already punished)
	for each (amount, depositor) in proposal.Deposits
		depositor.AtomBalance += amount

	stateWriter, err := proposal.Handler()
	if err != nil
		// proposal passed but failed during state execution
		proposal.CurrentStatus = ProposalStatusFailed
		else
		// proposal pass and state is persisted
		proposal.CurrentStatus = ProposalStatusAccepted
		stateWriter.save()
	else
	// proposal was rejected
	proposal.CurrentStatus = ProposalStatusRejected

	store(Governance, <proposalID|'proposal'>, proposal)
```

## Parameters

The subspace for the Governance module is `gov`.

```go
type DepositParams struct {
	MinDeposit       sdk.Coins     `json:"min_deposit,omitempty" yaml:"min_deposit,omitempty"`
	MaxDepositPeriod time.Duration `json:"max_deposit_period,omitempty" yaml:"max_deposit_period,omitempty"` //  Maximum period for Atom holders to deposit on a proposal. Initial value: 2 months
}

type TallyParams struct {
	Quorum    sdk.Dec `json:"quorum,omitempty" yaml:"quorum,omitempty"`
	Threshold sdk.Dec `json:"threshold,omitempty" yaml:"threshold,omitempty"`
	Veto      sdk.Dec `json:"veto,omitempty" yaml:"veto,omitempty"`
}

type VotingParams struct {
	VotingPeriod time.Duration `json:"voting_period,omitempty" yaml:"voting_period,omitempty"`
}
```

### MinDeposit

- type: `Coins`
- denom: `uluna`
- amount: `50000000`


The minimum deposit amount for a proposal to enter a voting period. Currently 50 Luna.

### MaxDepositPeriod

- type: `time.Duration` (seconds)
- `"max_deposit_period": "1209600s"`

Maximum period for Luna holders to deposit on a proposal. Currently 2 weeks.

### Quorum

- type: `Dec`
- `"quorum": "0.400000000000000000",`

Minimum percentage of total stake needed to vote for a result to be considered valid.

### Threshold

- type: `Dec`
- default value: 50%

Minimum proportion of Yes votes for a proposal to pass.

### Veto

- type: `Dec`
- default value: `0.33`

Minimum value of Veto votes to Total votes ratio for proposal to be vetoed.

### VotingPeriod

- type: `time.Duration` (seconds)
-  `"voting_period": "604800s"`

Length of the voting period. Currently 1 week.
