# Definition

The `derivative` of a function $f \colon \mathbb{R}^m \to \mathbb{R}^n$ at $x \in \mathbb{R}^m$ is
the unique linear function $\frac{df}{dx} \colon \mathbb{R}^m \to \mathbb{R}^n$ such that

$$
\lim_{h \to 0} \frac{||(f(x + h) - f(x)) - \frac{df}{dx}(h)||}{||h||} = 0
$$

## Intuition

For $x \in \mathbb{R}^m$, consider the function

$$
f_x: \mathbb{R}^m \to \mathbb{R}^n, \quad h \mapsto f(x + h) - f(x)
$$

This function describes how $f$ varies around $x$. $\frac{df}{dx}$ approximates $f_x$ in the sense
that the difference between the two, relative to the input (an element of $\mathbb{R}^m$),
approaches $0$ as the input approaches $0 \in \mathbb{R}^m$.

## Notation

Let $dx \in \mathbb{R}^m$ be small (so that $x + dx$ is a small perturbation of $x$) and denote by
$df$ the corresponding variation $f_x(dx) = f(x + dx) - f(x)$ of $f$ around $x$. We get the natural
equation which gives us the rationale for the $\frac{df}{dx}$ notation:

$$
\frac{df}{dx}(dx) = df
$$
