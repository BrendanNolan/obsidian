# Definition

The `derivative` of a function $f \colon \mathbb{R}^m \to \mathbb{R}^n$ at $x \in \mathbb{R}^m$ is
the unique linear function $f'(x) \colon \mathbb{R}^m \to \mathbb{R}^n$ such that

$$
\lim_{h \to 0} \frac{||(f(x + h) - f(x)) - f'(x)(h)||}{||h||} = 0
$$

**Note:** Take care about the function $f'$; its domain is the points in $\mathbb{R}^m$ where $f$ is
differentiable and its codomain is the $mn$-dim vector space of linear maps
$\mathbb{R}^m \to \mathbb{R}^n$.

## Intuition

Let $X$, $W$ be normed vector spaces, let $g \colon X \to W$ be a not-necessarily-linear function,
and let $x$ be an element of $X$. We define the `variance function` (my terminology, not standard)
of $g$ at $x$ by

$$
V(g,x) \colon X \backslash \{0\} \to W; \quad h \mapsto g(x+h) - g(x)
$$

$V(g,x)$ describes how $g$ varies around $x$. If $g$ is linear, then of course $V(g,x)$ is just $g$
and does not even depend on $x$.

Now, by the definition of the derivative, $f'(x)$ approximates $f$ at $x$ in the sense that the
difference between the their variance functions at $x$, scaled by the perturbation of $x$,
approaches $0$ as the perturbation of $x$ approaches $0$.

## Notation

Let us fix some $x \in \mathbb{R}^m$. Denote $V(f,x)$ by $df$, so that
$df \colon h \mapsto f(x+h) - f(x)$. Let $dx \in \mathbb{R}^m$ be small. Now, the definition of the
derivative tells us that

$$
\frac{||df - f'(x)(dx)||}{||dx||} \approx 0
$$

Now, since $dx$ is small, we certainly have

$$
||df - f'(x)(dx)|| \approx 0
$$

i.e.

$$
f'(x)(dx) \approx df
$$

which gives us the rationale for the $\frac{df}{dx}$ notation, because we get

$$
\frac{df}{dx}(dx) \approx df
$$
