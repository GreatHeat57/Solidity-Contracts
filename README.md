
## Install
```bash
$ npm install hanzo-solidity
```

## Usage
Import the contracts you want to use:

```solidity
import "hanzo-solidity/contracts/Versioned.sol";

contract Version {
    function version() public pure returns (string) {
        return "1.0.0";
    }

    function boolean() public pure returns (bool) {
        return true;
    }
}

contract Contract is Versioned {}
```

And interact with them as you would normally:


```javascript
let Contract = artifacts.require('./Contract.sol')
let Version  = artifacts.require('./Version.sol')

contract('Contract', async () => {
  let contract, version, proxy = null

  before(async () => {
    contract = await Contract.deployed()
    version  = await Version.deployed()
    await contract.setVersion(version.address)

    // Use current version's ABI and provide simple interface to a versioned
    // smart contract.
    proxy = await Version.at(contract.address)
  })

  it('should return result from proxied method', async () => {
    let v = await proxy.boolean.call()
    assert.equal(v, true, 'boolean() did not return expected boolean')
  })
})
```
