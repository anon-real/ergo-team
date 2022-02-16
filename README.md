# ErgoTeam

## Motivation

The main goal of ErgoTeam is to be able to spend some boxes in an arbitrary complex transaction by voting. This generality enables ErgoTeam to be usable in many different domains. For example, fund management, parameter updating for dApps, etc.

These features can be obtained by staking — users will stake their voting tokens to indicate their vote. Staking has several drawbacks, including:

- Voting tokens are not necessarily issued for voting and may have other functionalities so it may not be practical to ask the user to stake his tokens for relatively a long time.
- It doesn’t allow the user to participate in several votings using the same tokens.
- For any arbitrary reason, user may need the tokens to be in his wallet instead of in some staking P2S. For example he may receive rewards in the wallet he is holding the tokens.

ErgoTeam is aiming to avoid staking and propose a novel approach for spending boxes by voting.

## Main Idea

The main point of staking is to avoid double votings by the user. ErgoTeam is proposing another approach to eliminate double voting without staking. The main idea in ErgoTeam is to let user keep the voting tokens in the same address he has used to cast his vote. The voting tokens must be kept in the same box as an NFT that is issued when he has casted his vote. This address should be his wallet’s change address to reduce the chance of tokens moving around different addresses within the same wallet.

This requirement may seem limiting since we not only require the voting tokens to remain in the same address but in the same box as an issued NFT, however:

- User can use his wallet without any limitations if the chosen address is set to be his change address. So he can move around anything he likes as long as he is not specifically moving the voting tokens or the NFT.
- He still will be able to use ErgoTeam and participate in other proposals using the same voting tokens.
- If for some non-malicious reasons this requirement is violated, ErgoTeam UI will let the user know and will offer him a transaction to fix the issue.

The last thing to take into account regarding this limitation is that we should compare this method to the alternative ones such as staking. In staking user can not use his tokens at all so we think that this requirement is not an impractical one.

## Method

As mentioned earlier, the main goal of ErgoTeam is to spend some boxes by voting. In the dApp implementation level, different usages like fund management or parameter updating may require different considerations. However, in the protocol level, all these applications are the same. That being said, we only consider fund management in the remaining sections for the ease of communication.

ErgoTeam is consisted of different components and stages which wee will discuss now.

### Config Box

This box contains parameters of some fund management such as number of required votes for funds to be spendable. The config box can be spent using the same protocol to update its parameters. For example, let’s say Ergo Foundation is using ErgoTeam to manage the Ergo treasury. If after some time another board member is joined with the team, the parameters needs to be updated which can happen upon consensus of the current board members using ErgoTeam.

This box also contains proposal tokens. Any proposal for spending funds should have one of these tokens.

### Proposal Box

This box is a proposal for spending funds. It contains information about how much of the funds will be spent, the target address and additional information to let the voters know about the goal of the proposal. If the proposal box receives enough votes, funds will be spendable based on its parameters.

### Vote Cast Box

This box represents the vote casted by some user. One of its registers specifies which proposal the vote is for, i.e., the proposal box ID. Upon the creation of this box, an NFT should be issued for the user as the proof of voting. The ID of this NFT should also be specified in the Vote Cast Box. The first input of the transaction issuing that NFT must contain the voting tokens and its proposition bytes must be the appropriate proxy contract. These two conditions make sure that only the user could have casted the vote and is verifiable in ergo script by providing the spent input box.

## Procedure

The way everything falls into place is as follows. Token holders can create a proposal box using the config box. Every other token holders can see the proposal in the UI and can create their Vote Cast Box if they are in favour of the proposal. After the voting period has finished, the counting stage will start. During this stage, the ErgoTeam bots will invalidate the double votes and put final validation on the Vote Cast Boxes that are not cheating. After this period finishes, if the enough votes have been casted based on the Config Box, the funds box can be spent according to the proposal.

## ErgoTeam bots

To achieve this kind of efficiency, we needed to introduce bots to this scheme that will help count the votes and get rid of invalid votes. ErgoTeam provides incentive for bot runners by introducing ErgoTeam token! This token will be issues and is the only token users can pay ErgoTeam fees by. Bot runners will receive ErgoTeam tokens for every operation they do. This provides incentive for bots to perform necessary operations in the network while not being able to cheat in any way.

## ErgoTeam token

This token is the only way users will be able to use the dApp and paying the necessary fees. Bots will be paid with this token as well. The amount of issuance is not clear yet but it will be issued right from the start. A small portion of the tokens will be held by the devs to incentivize further development. A portion will go through initial sell procedure. A portion will be put aside to start a liquidity pool in ErgoDex. And a portion will be used to pay extra compensation to bots in the beginning of the lunch since the tokenomic structure may take some time to stabilize.
