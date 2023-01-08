# ZK  whiteboard. SNARK vs. STARK

STARK is a zero-knowledge proving system, acronym stands for Scalable Transparent Argument of Knowledge. The "S" from STARK and SNARK claims for different things; STARK is for Scalable and SNARK is for Succint (small proof and fast to verify). 

In a STARK protocol do not necessarily need pro-processing or taking the original description of the program for the purposes of verifier to be able to verify it. 

Pre-processing means that we the description of the circuit or program and we compress it down to something that's much smaller that lets us verify proof that use that same. 
.
In STARK, transparent meaning that you don't need a trusted setup. In SNARK, we need a trusted setup. 

What is a trusted setup? When we want to build a ZK proof, requires at least one person in a ceremony to generate a secret and throw it away and if they don't then people can forge proof. Ideally we wanto to avoid this. 

The "N" from SNARK means non-interactive, however a STARK protocol could be interactive or no interactive. However, sometimes there is benefits to be having an interactive protocol, like potentially smaller proof sizes.

There are two components that are used in both protocols:

1. Arithmetization. Process which take a program and transform it into a statement about polynomials. 

SNARK use a Rank-1 Constraint System (R1CS) and STARK use Algebraic Intermediate Representation (AIR). In case of R1CS you can think about the program such as an electronic circuit where you have a bunch of gates that are connected by wires, you put inputs and those signals propagate throug the gates. In the approach of AIR, we think about the computation as a machine computation which starts in some initial state and you can think about the state as some set of variables, then you apply a transition function to the state and update those variables in some manner. 

2. Polynomial Commitment Scheme are used to make sure that the statements about polynomials that we obtain in arithmetization have bounded degrees of some sort.  

There are different types of commitment schemes. In SNARK, KZG and Inner Product Argument (IPA) are frequently used; KZG need a trusted setup and relies in elliptic curve pairings. IPA do not need a trusted setup but relies also in elliptic curves. KZG is preferred becasuse the proof size are very small. FRI which is frequently used in STARK because you do not want to rely on any kind of trusted setup and relies only on hash functions.  

Example using AIR.

Imagine our computation as a sequence of repeated states which result from applying a transition function to an input, this is called an execution trace. This is a matrix where each row represents the state of that computation in a given step. The transition function allow to define a relation between the rows in the matrix. For example, we can take two rows, apply some equations to them and if equations evaluate to zero, then we say that transition was correct. In otherwise, if any of the equations evaluate it is non-zero, then we say the equation is incorrect.

A Lower degree extension take some values of the matrix and asume that are "n" evaluations of some polynomial. But, how do we know that once we know this evaluations can really get the polynomial back in coefficient form? We can interpolate the polynomial of degree n-1 (n = to the size of the evaluations)    

Once we interpolate the evaluations, we get another matrix with the polynomials in coefficient form. The next step is to evaluate over a larger domain using solomon encoding, which reduces kind of this redundancy into the structure, once we do this result in coding we like increase the size of the domain and it is easy to catch the dishonest prover if they are trying to cheat.

After solomon encoding, we obtain a matrix with an extension. This matrix keeps the number of columns the same, but now we ave twice the number of rows, so. we increase te degree of our contraints. 

In this example they suggest a "2x" extension but in some cases we could need to do a larger extension. 

We can divide the polynomial to define how to "divide" the rows. We divide this using vanishing polynomial or constraint divisor.

f_0(x)^2 - f(x) = 0 constraint
(x^n -1) / (x - \omega ^n-1) rows where apply the constraint 

this values need to evaluate in every row

Composition polynomial: is a single polynomial use to merge all the polynomials together. This polynomial use a random coefficients.

What does FRI? Take the composition polynomial and allows us to verify it. Use an algorithm that reduces the size fo the evauations of a polynomials over the domain by half or more with every step

FRI is divide in two steps:
1. Commit phase: prover does "Degree Respecting projection" and commits to each successive layer and sends the commitment to the verifier.
2. Query phase: the verifier picks random points in the domaind field and asks the prover to send all the relevant values for each layer from this domain. After this, the verifier will be convince.




