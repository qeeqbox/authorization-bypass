<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/authorization-bypass.png"></p>

A threat actor may perform unauthorized functions by bypassing or abusing the target authorization mechanism

## Example #1
1. Developer forgets to remove an in-house debugging mechanism associated with user-agent
2. A threat actor finds out changing the user-agent header to debug grants different or higher privileges

## Impact
Vary

## Risk
- read & modify data
- execute commands

## Redemption
- validate access control

## ID
91f9b046-b802-425a-b71b-64c21c6b1c0f

## References
- [mitre](https://cwe.mitre.org/data/definitions/639.html
