## Proxy access policy


### Proxy access policy overview

- Proxy allows users to act as other users in workday.
- Each environment can only have one proxy access policy and you can't configure proxy access policy on production. 
- You can configure multiple rules to control access.

- Only unconstrained security groups can proxy such as user based security groups
- Indicate security group that cannot proxy.


#### Proxy session

- Authorized users can act in behalf of target users.
- Proxy users perform all the same tasks as the target user.
- Use the "stop proxy" task to stop proxy.

Access limitations:
1. No support for connections to other devices
2. Cannot initiate a proxy as a delegate
3. No  support for delegated tasks in the proxy mode.

### Accessing multiple rows in the policy

#### Proxy access policy

- Workday assess the rules in order, so put the most restrictive rule at the top and the least restrictive rule on the bottom.
- You can run the "edit proxy access policy" task to edit the proxy access policy.