# Discrete-Logarithm Based Polynomial Commitments #

## Background ##

### 1. The Discrete Logarithm Assumption ###

Given a multiplicative cyclic group of order $p$, $Z_p^{\*}$ for a prime $p$, and a generator $g \in Z_p^{\*}$, for some $x$, $1 < x < p-1$, if $y \equiv g^x \mod p$ then it is computationally infeasible to determine $x$ from $y$.

$G$ is called the *base group* while $G_T$ is called the *target group*.

### 2. The Diffie-Hellman Assumption ###

Let $Z_p^{\*}$ be a multiplicative group for some prime $p$. For a generator $g \in Z_p^{\*}$ and integers $x,y$ where $1 < x,y < p-1$, if $g^x$ and $g^y$ are given, it is computationally infeasible to obtain $g^{xy}$.

### 3. Bilinear Pairings ###

Consider a group $G$ and a *target* group $G_T$. Let $P, Q \in G$. For integers $x, y$ we define a *bilinear pairing* $e: G \times G \rightarrow G_T$ as follows: $$e(P^x,Q^y) = e(P,Q)^{xy}$$

Given two elements of $G$, viz. $g^x, g^y$, it should be possible to check for some element $h = g^{xy}$ without knowing $x$ and $y$.
How? Using the operator $e$, repeated application of the definition of $e$ yields the following:
$$e(g^x, g^y) = e(g, g)^{xy}$$
$$\textrm{and } e(g^{xy}, g) = e(g^{xy}, g^1) = e(g, g)^{(xy)(1)} = e(g, g)^{xy}$$
$$\therefore e(g^x, g^y) = e(g^{xy}, g)$$

### 4. BLS Signature Scheme using Bilinear Pairings (Boneh, Lynn, Shacham, 2001) ###

Given:
- A base multiplicative group $G$ of prime order $p$
- A target group $G_T$
- A generator $g \in G$, and
- A bilinear pairing operator $e$ (used for signature verification only)
- A message $m \in M$ to be signed, where $M$ is the message space
- A hash function $H:M \rightarrow G$

Three operations are involved.
- Public-Private Key-pair Generation
- Signature Generation
- Signature Verification

#### Public-Private Key-pair Generation ####

1. Choose an integer $x$ such that $1 < x < p-1$. This is the private key.
2. Compute the public key: $y = g^x \mod p$

#### Signature Generation, $\sigma$ ####
Compute:
$$\sigma = H(M)^x$$
This signs the (hash of the) message $M$ using private key $x$.

#### Signature Verification ####
Using the public key $g^x$, verify the signature on the (hash of the) message $m$ by establishing that the following equality holds:
$$e(H(m), g^x) = e(\sigma, g)$$

##### Check for correctness of algorithm #####
Using the definition of $e$, it is clearly seen that:
$$e(\sigma, g) = e(H(m)^x, g^1) = e(H(m), g)^{x\times 1} = e(H(m), g)^x$$
