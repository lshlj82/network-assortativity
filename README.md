# Assortativity, live (동류성, 유유상종)

An interactive, single-page web demo of **network assortativity**: the tendency of similar nodes to link to each other ("birds of a feather"). It is meant for teaching an introductory network-science course.

> Created by **Claude Opus 5.5** (Anthropic), based on lecture slides by **Sang Hoon Lee** for an introductory network science course.

## Live demo

Open `index.html` in any modern browser. Nothing needs to be installed and there is no build step.

To host it with GitHub Pages:

1. Push `index.html` and `README.md` to a repository.
2. Go to **Settings → Pages**, choose **Deploy from a branch**, and select `main` / root.
3. The demo will be served at `https://<user>.github.io/<repo>/`.

## What you can do

### By degree (degree assortativity)

- **Rewire a network live** while keeping every node's degree fixed, then watch the degree-assortativity coefficient *r* respond.
- Choose between two degree-preserving rewiring rules:
  - **Metropolis ERG (Noh 2007).** This is the exponential random graph model with Hamiltonian
    *H* = −(*J*/2) Σ<sub>ij</sub> *a<sub>ij</sub> k<sub>i</sub> k<sub>j</sub>* = −*J* Σ<sub>edges</sub> *k<sub>i</sub>k<sub>j</sub>*.
    A swap of edges (a,b),(c,d) → (a,c),(b,d) is accepted with probability min(1, e<sup>−Δ*H*</sup>). *J* > 0 gives assortative networks, *J* < 0 disassortative ones, and *J* = 0 gives randomized, uncorrelated ones.
  - **Biased swaps (Xulvi-Brunet & Sokolov 2004).** With probability |*p*|, the four endpoints are sorted by degree and paired either assortatively or disassortatively. Otherwise they are paired at random.
- **See the correlation in three ways:**
  - the ⟨*k*<sub>nn</sub>(*k*)⟩ vs *k* plot (log-log), with the uncorrelated reference ⟨*k*²⟩/⟨*k*⟩;
  - a scatter of the degrees at both ends of every edge, which is the cloud whose Pearson correlation is *r*;
  - a history trace of *r* as the network relaxes.
- **Click a node** to see *k*<sub>nn</sub>(*i*) = (1/*k<sub>i</sub>*) Σ<sub>j</sub> *a<sub>ij</sub>k<sub>j</sub>* worked out with real numbers.
- **Generated networks:** Barabási–Albert (m = 1, 2) and Erdős–Rényi with Poisson degrees ⟨*k*⟩ = 4, at 100, 200 or 400 nodes.

### Example networks

| Example | Type | *r* |
|---|---|---|
| Noh 2007, *J* = −1 (Poisson, ⟨k⟩ = 4) | disassortative | ≈ −0.85 |
| Noh 2007, *J* = 0 | neutral | ≈ 0 |
| Noh 2007, *J* = +1 | assortative | ≈ +0.86 |
| Zachary karate club | real, disassortative | −0.476 |
| Les Misérables co-appearance | real, disassortative | −0.165 |
| Florentine families | real, disassortative | −0.375 |
| Davis southern women (bipartite) | real, disassortative | −0.337 |
| Star (24 leaves) | toy | −1 |
| Chain of stars | toy, disassortative | −0.794 |
| Ring of cliques (sizes 3–9) | toy, assortative | +0.856 |
| Core with tendrils | toy, assortative | +0.711 |

Any example can be rewired afterward. For instance, you can push the karate club toward the assortative regime without changing a single node's degree. The star is the exception: it can't be rewired, because there's only one way to connect those degrees.

### By type (categorical assortativity)

- This tab uses a two-type network (purple and green). Cross-type edges are drawn dashed.
- Sliders control the within-type linking probability, the group sizes and the mean degree.
- The mixing matrix *e<sub>ij</sub>*, its row sums *a<sub>i</sub>*, and
  *r* = (Σ<sub>i</sub>*e<sub>ii</sub>* − Σ<sub>i</sub>*a<sub>i</sub>b<sub>i</sub>*) / (1 − Σ<sub>i</sub>*a<sub>i</sub>b<sub>i</sub>*)
  update live, with the numbers filled in.

## Implementation notes

- Everything is in a single self-contained file: plain HTML, CSS and JavaScript on `<canvas>`, with no frameworks. The only external resources are fonts from Google Fonts, and the page falls back to system fonts without them.
- The layout is a simple force-directed simulation (O(N²) repulsion), which is fine up to a few hundred nodes.
- *r* is computed as the Pearson correlation of the degrees at the two ends of each edge, following Newman (2003). This is identical to `nx.degree_assortativity_coefficient(G)`. Using excess degree (*k* − 1) gives the same value.
- "MC sweeps" counts attempted swaps divided by the number of edges, the Monte Carlo time unit used by Noh (2007).
- The real-network edge lists are the versions bundled with NetworkX 3.x, embedded directly in the page.
- The page supports light and dark mode and adapts to mobile screens.

## References

- M. E. J. Newman, "Assortative mixing in networks," *Phys. Rev. Lett.* **89**, 208701 (2002).
- M. E. J. Newman, "Mixing patterns in networks," *Phys. Rev. E* **67**, 026126 (2003).
- J. D. Noh, "Percolation transition in networks with degree-degree correlation," *Phys. Rev. E* **76**, 026116 (2007).
- R. Xulvi-Brunet and I. M. Sokolov, "Reshuffling scale-free networks: From random to assortative," *Phys. Rev. E* **70**, 066102 (2004).
- S. Maslov and K. Sneppen, "Specificity and stability in topology of protein networks," *Science* **296**, 910 (2002).
- S. H. Lee, P.-J. Kim, and H. Jeong, "Statistical properties of sampled networks," *Phys. Rev. E* **73**, 016102 (2006).
- F. Menczer, S. Fortunato, and C. A. Davis, *A First Course in Network Science*, Cambridge University Press (2020), ch. 2.

Please double-check these citations before reusing them; they were written from memory.

## Credits

- Demo created by **Claude Opus 5.5** (Anthropic).
- Based on lecture slides on assortativity by **Sang Hoon Lee**, prepared for an introductory network science course. The slides' explanations, formulas and figure ideas shaped the demo's content.
- Metropolis rewiring algorithm from J. D. Noh, *Phys. Rev. E* **76**, 026116 (2007).
