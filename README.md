# ARISEN Ricardian contract parser

This library strips out the `{{ param }}` tags and replaces it with data from the transaction
itself.

## Installation

```
npm i -S @arisensdk/rc-parser 
```



## Usage

```js
const parser = require('@arisensdk/rc-parser');
 
from arisensdk signProvider ... signargs.transaction.actions.map(async action => {
    const abi = await eos.contract(contractAccountName);
    const data = abi.fc.fromBuffer(action.name, action.data);
    const actionAbi = abi.fc.abi.actions.find(fcAction => fcAction.name === action.name);
     
    
    const parsedRicardianContract = parser.parse(action.name, data, actionAbi.ricardian_contract);
     
    // Optional HTML formatting
    const htmlOptions = {h1:'b', h2:'div class="someclass"'};
    const parsedRicardianContract = parser.parse(action.name, data, ricardian, signer, true || htmlOptions);
}):
```

Or constitution:

```js
const ricardian = systemAbi.abi.ricardian_clauses[0].body;
const parsedRicardianContract = parser.constitution(ricardian, 'testaccount', {h1:'h1'});
```

## Examples results:

No HTML formatting

```js
parser.parse('bidname', {bid: '3 RIX', bidder: 'testaccount', newname: 'somename'}, ricardian, 'testaccount');
```
```
# Action - "bidname"

## Description

The "bidname" action places a bid on a premium account name, in the knowledge that the high bid will purchase the name.

As an authorized party I "testaccount" wish to bid on behalf of "testaccount" the amount of "3 RIX" toward purchase of the account name "somename".

```

HTML formatting.

```js
parser.parse('bidname', {bid: '3 RIX', bidder: 'testaccount', newname: 'somename'}, ricardian, 'testaccount', {h1:'b', h2:'i class="test"'});
```
```
<b>Action</b> - "bidname"<br><br><i class="test">Description</i><br><br>The "bidname" action places a bid on a premium account name, in the knowledge that the high bid will purchase the name.<br><br>As an authorized party I "testaccount" wish to bid on behalf of "testac
count" the amount of "3 RIX" toward purchase of the account name "somename".<br>

```