In AD environment, Trust is a relationship between two domain or forest which allow users of one domain or forest to access resources in other domain for forest.

Trust can be automatic (parent-child, same forest etc) or established (forest, external).

Trusted Domain Objects (TDOs) represent the trust relationship in a domain.

### Trust Direction
1. One Way Trust : Unidirectional. Users in the trusted domain can access resources in the trusting domain but reverse is not true. ![[Pasted image 20221102174533.png]]
2. Two Way Trust: Bi-directional. Users of both domains and access resources in the other domain![[Pasted image 20221102174754.png]] 
### Trust Transitivity
1. Transitive : Can be extended to establish trust relationship with other domains. All the default intra-forest trust relationship ( Tree root, parent child ) between domains within the smae forest are transitive two way trust. ![[Pasted image 20221102175143.png]]
2. Non Transitive Trust: Cannot be extended to other domains in the forest . Can be two-way or one-way. This is the default trust (called external trust) between two different domain in two different forest when forest don't have a trust relationship.

## Types of Trust
### Default/Automatic Trust
- Parent-Child Trust : It is created automatically between the new domain and the domain that preceeds it in the namespace heirarchy, whenever a new domain is added in a tree. For example `subdomain.domain.local` in a child of `domain.local`. This trust is always two way transitive.
- Tree-Root Trust: It is created automatically between whenever a new domain tree is added to a forest root. This trust is always two-way transitive. ![[Pasted image 20221102213912.png]]
### Shortcut Trust
- Used to reduce access times in complex trust scenario.
- Can be one-way or two-way transitive.
- Reduce hops
- ![[Pasted image 20221102214107.png]]
### External Trust 
- Between two domain in different forets when forests do not have a trust relationship 
- Can be one-way or two-way and is non-transitive
- ![[Pasted image 20221102214402.png]]
### Forest Trusts
- Between forest root domain.
- Cannot be extended to a third forest (No implicit trust)
- Can be one-way or two-way and transitive and non-transitive

Trust Enumeration - [[Active Directory/Domain Enumeration/Powershell/Trusts]]