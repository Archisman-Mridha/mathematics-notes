# Advanced Cryptography

Main references :

- [MIT 6.5630 Advanced Topics in Cryptography, Fall 2023](https://www.youtube.com/playlist?list=PLUl4u3cNGP61EZllk7zwgvPbI4kbnKhWz)

- [ChatGPT's answers to all my questions](https://chatgpt.com/share/67e4bd82-1e00-8000-9328-d044ee2f0fe7)

Summarization of the course :

## Interactive Proofs (IPs)

In Cryptography, a `language` is defined as a set of strings. And the set of languages for which, membership can be proved in polynomial time, are called `Non-deterministic Polynomial time (NP) languages`.

The lecturer starts introducing us to `Interactive Proofs (IPs)`. For IPs :
- we allow repeated interactions (>> 1) between the prover and the verifier.
- there is a very small probability that the verifier may accept false proofs.

> There's a concept in-between Non-interactive deterministic Proofs and IPs : **Non-interactive Proofs were the verifier V is randomized**.
>
> Suppose, we want to prove the matrix multiplication operation of two matrices n x n matrices A and B. The best known deterministic algorithm for doing this has a complexity of O(n^2.36).
>
> But we can improve the time complexity to O(n^2) by taking help of randomization. The downside however will be that there will be a probability of the verifier accepting a false proof. We can diminish the probability by chosing the matrix elements from a very large finite field.

A formal definition of IPs is then provided : An IP is **a protocol between an all powerful prover P and a polynomial time verifier V such that for some language L, there should be completeness and soundness**. `Completeness` is the minimum (large) probability with which V may accept a valid proof. And `soundness` is the maximum (small) probability with which V may reject a false proof.

## The Sum Check Protocol

1. We have a multivariate polynomial f in field F and atmost degree d in each variable.
2. Pick a subset (H) of elements from F.
3. Evaluate the polynomial f for all possible combinations of elements from H.
4. And then sum up all the evaluations.

We want to prove that the sum is β.

That's the problem statement and sum check protocol is the solution.

> Remember, the actual intention of V is to catch a false prover (P*).
>
> **During the first interaction, P\* will send a wrong polynomial g\*(x)** which may pass V's checks. If so, V will try to make P* admit that g*(x) and g(x) are seperate polynomials in F. This is where the Schwartz Zippel Lemma gets applied.
>
> REFERENCE : [The intuition behind the sum-check protocol in 5 minutes](https://www.cryptologie.net/article/577/the-intuition-behind-the-sum-check-protocol-in-5-minutes/).

Sum check protocol is a type of `public coin protocol` where V doesn't keep any state and sends truly random messages (public coins) back to P in each interaction.

Further readings :

- [ ] [The Unreasonable Power of the Sum-Check Protocol](https://zkproof.org/2020/03/16/sum-checkprotocol/)

- [ ] [Sum Check Protocol - part 3: Benchmark](https://katat.me/blog/Sum+check+-+benchmarks)

- [ ] [Rust Guide: Sum-Check protocol](https://medium.com/yearofzk/rust-guide-sum-check-protocol-18ceb8affdb2)

- [ ] [Breaking down the Sum-Check Protocol](https://mtteo.dev/posts/understanding-sumcheck-protocol/understanding-sumcheck-protocol/)

- [ ] [Understanding Interactive Proof Systems and Sum Check Protocol: Part 2](https://blog.rachitasrivastava.com/understanding-interactive-proof-systems-and-sum-check-protocol-part-2-a2eef4a1e061)

## Circuit Arithmetization and application of Sum Check Protocol in #SAT

The lecturer introduces us to circuit arithmetization via the application of Sum Check Protocol in #SAT.

She converts **a boolean circuit with boolean gates into (a polynomial) an arithmetic circuit with (arithmetic) addition and multiplication gates**. Once we have polynomials, we know we can use the Sum Check Protocol, after we show that the **soundness << 1**.

## Using Low Degree Polynomial Extension theorem to apply Sum Check Protocol

For DEIPs, **we have polynomial complexity for P** and **V's complexity reduced to O(n)**. The Sum Check protocol is a DEIP.

Consider the following problem statement : P needs to prove to V, that there are β triangles in a graph G(V, E) where |V| = n.

> Niece and nephew, don't get confused about the mathematical representation of a vertex V : {0, 1}^(log n)!
> We're representing a vertex V as a binary string of length log n.

Yael represents the statement using an equation involving summation of a function. Now, the problem is that, the **function doesn't have any algebraic structure**.

We'll first prove that any function which takes inputs from H (which doesn't have any algebraic structure), can be extended to a function f~ which :
- takes inputs from a finite field F.
- agrees with f in H.

This is caled the `Low Degree Extension Theorem`.

> f~ is called a `multi-linear extension`, if each of it's variable has a degree of 1.

Notice that f~ is a multivariate polynomial. Instead of f, **we use f~ in the equation involving summation**. And then apply the Sum Check Protocol to prove that equation.

## The GKR protocol
