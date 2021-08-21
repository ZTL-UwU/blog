# GAME THEORY

## BASH GAME

### Definition

- Two numbers: $N, K \ (1 \leq K \leq N)$
- A pile of $N$ stones
- $2$ players (**Player A** and **Player B**)
- Rules:
    1. Two players **take turns** taking away stones, **Player A** starts first.
    2. $1 \leq x \leq K$ stones can be taken each time.
    3. If no stone remains, the game ends.
    4. The **last** player who took stones wins.

With the value of $N$ and $K$ given, who will win?

#### Example

If $N = 4, K = 2$:

**Red** numbers represent the number of stones **Player A** chose each turn.  
**Blue** numbers represent the number of stones **Player B** chose each turn.

$$
\begin{aligned}
    &\quad \text{Steps} \quad \text{Winner}\\
    &①\ \color{#EA6C6D}{1} \ \color{#3199E1}{1} \ \color{#EA6C6D}{2} \qquad \ \ \color{default}{A}\\
    &②\ \color{#EA6C6D}{1} \ \color{#3199E1}{2} \ \color{#EA6C6D}{1} \qquad \ \ \color{default}{A}\\
    &③\ \color{#EA6C6D}{2} \ \color{#3199E1}{2} \quad\ \qquad \color{default}{B}
\end{aligned}
$$

As we can see, if and if only **Player A** chose $1$ stone the first step, **Player A** wins.
