# Definition

The `derivative` of a function $f \colon \mathbb{R}^m \to \mathbb{R}^n$ at $x \in \mathbb{R}^m$ is
the unique linear function $\frac{df}{dx} \colon \mathbb{R}^m \to \mathbb{R}^n$ such that

$$
\lim_{h \to 0} \frac{||(f(x + h) - f(x)) - \frac{df}{dx}(h)||}{||h||} = 0
$$

## Intuition

Let $X$, $W$ be normed vector spaces, let $f \colon X \to W$ be a not-necessarily-linear function,
and let $x$ be an element of $X$. We define the `variance function` (my terminology, not standard)
of $f$ at $x$ by

$$
V(f,x) \colon X \backslash \{0\} \to W; \quad h \mapsto (f(x+h) - f(x))/||h||
$$

$V(f,x)$ describes how $f$ varies around $x$, scaling by the perturbation from $x$. If $f$ is
linear, then of course $V(f,x)$ simply sends $h$ to $f(h)/||h||$ and does not even depend on $x$.

Now, $\frac{df}{dx}$ approximates $f$ at $x$ in the sense that the difference between the their
variance functions at $x$ approaches $0$ as the perturbation of $x$ approaches $0 \in \mathbb{R}^m$.

## Notation

Denote by $df$ the variation $V(f,x)$ of $f$ around $x$. Let $dx \in \mathbb{R}^m$ be small (so that
$x + dx$ is a small perturbation of $x$). Then, for small enough $dx$, the definition of the
derivative tells us that

$$
\frac{df}{dx}(dx) = df
$$

which gives us the rationale for the $\frac{df}{dx}$ notation.
