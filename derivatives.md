# Definition

The `derivative` of a function $f \colon \mathbb{R}^m \to \mathbb{R}^n$ at $a \in \mathbb{R}^m$ is
the unique linear function $f'(a) \colon \mathbb{R}^m \to \mathbb{R}^n$ such that

$$
\lim_{h \to 0} \frac{||(f(a + h) - f(a)) - (f'(a))(h)||}{||h||} = 0
$$

## Intuition

For $a \in \mathbb{R}^m$, consider the function

$$
f_a: \mathbb{R}^m \to \mathbb{R}^n, h \to f(a + h) - f(a)
$$

This function describes how $f$ varies around $a$. The derivative, $f'(a)$ of $f$ at $a$
approximates $f_a$ in the sense that the difference between the two, relative to $||h||$, approaches
$0$ as $||h||$ approaches $0$.
