# Cryptography

## Basis

$$
\begin{align}
\text{cipher} &= E_{\text{key}_e}(\text{msg})\\
\text{msg} &= D_{\text{key}_d}(\text{cipher}) \\
E_{\text{key}_e}( D_{\text{key}_d}(\text{msg}) ) &= \text{msg}
\end{align}
$$

### Symmetric-key Algorithm

$\text{key}_e = \text{key}_d$

e.g. DES, RCx, AES

### Public-key Cryptography (Asymmetric Cryptography)

$\text{key}_e \ne \text{key}_d$

e.g. RSA

### Three Features

1. Transportation or Substitution
2. Number of keys (symmetric or asymmetric)
3. Block or Stream cipher

### Security

Unconditional secure

Computationally secure



