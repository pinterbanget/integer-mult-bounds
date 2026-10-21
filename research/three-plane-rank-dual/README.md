# Three-plane COPY mass: the rank dual of #4365, for every plane triple

**Claim type:** exact lower-bound certificates for every plane triple, plus an exact `F₂` search
where those certificates are weak. **No new κ.** This answers part of the open question of
CubeJerry's [#4365](https://github.com/CrocSwap/integer-mult-bounds/pull/4365), which asks about a
"genuinely joint three-plane decoder". It uses #4365's contract unchanged:

- a scalar message `u vᵀ` is charged `t(S, U) = rank_{F₂}` of the binary label pairing on the
  whole-plane closures of `u` and `v`;
- charges add;
- payload coefficients lie in any field.

## Certificate

For a triple of planes and weights `w ≥ 0` on the seven plane unions `X`, define the functional
`L(M) = Σ_X w_X rank(M_XX)`. It is subadditive. On an atom `u vᵀ` it equals
`Σ_X w_X·[S meets X]·[U meets X]`. Suppose `L(u vᵀ) ≤ t(S, U)` for all 49 closure pairs. Then
every finite decomposition of `I₆`, over **any** field, costs at least `L(I₆) = Σ_X 2|X| w_X`.
This is #4365's two-plane functional `(p−c)rk M_AA + (q−c)rk M_BB + c·rk M`, extended from three
unions to seven. The coordinate decomposition always costs exactly 12.

## Results

The 56 matched planes of the selected archive give 27,720 triples in 486 tariff classes.

**All 27,720 triples have three-plane COPY floor 12, proven over every field.** For every triple of
planes, mixing cannot lower the COPY mass below separate coordinate messages, whatever the coefficients
or the number of terms. The 12 is sharp, since the coordinate decomposition costs exactly 12. Three
certificate families cover the triples.

- **26,977 triples: block-rank certificates.** These are integer weights `w_X`; for example,
  `w_A = w_AB = w_C = w_BC = 1` gives `L(I) = 2 + 4 + 2 + 4 = 12`.
- **418 triples: zeroed-block certificates.**
  - *Why block-rank fails here.* In these triples a cross pair `A, B` has binary rank 0, so a message
    from `A` to `B` costs **0**. Every block-rank functional counts such free atoms, which caps that
    family at 8, 10 or 11.
  - *The fix.* `Φ_Z` zeroes a set `Z` of off-diagonal plane blocks. `rank(Φ_Z(M)_XX)` is still
    subadditive, because it is the rank of a linear image. Free atoms map to 0, and `Φ_Z(I) = I`.
  - *The atom bound.* Restricted to `X`, `Φ_Z(u vᵀ)` is a sum over touched row planes of
    `u_P (Σ_allowed v_Q)ᵀ`. So its rank is at most the number of distinct allowed column (or row)
    plane sets, a combinatorial bound valid over every field.
- **325 triples: mixed zeroed-block certificates.**
  - *Why a single zero set fails here.* These 6 classes have no free atom, but they do have cost-1
    cross atoms: a binary cross rank of 1. No single `Z` works.
  - *The fix.* A sum over several zero sets at once does. For the cost-1 pair `A, B`, an example is

        L(M) = rank(M) + rank(Φ_{AB,BA}(M)).

    A cost-1 atom from `A` to `B` scores `1 + 0`, and `L(I) = 6 + 6 = 12`.
  - *How it was found.* An LP over all `64 × 7` pairs `(Z, X)` found these certificates.
  - *Corroboration.* The exact `F₂` minimum, from a parity ILP over all 3,969 binary atoms, is also 12
    on these classes. A real-coefficient alternating-least-squares search over every multiset of
    maximal closure patterns of mass at most 11 found nothing either. Its best residual stalls at
    exactly 1.

## Not covered

- **Native realization, coverage and hosting.** These are #4365's caveats, unchanged.
- **Decoders that mix four or more planes.**

## Evidence and reproduction

`generate.py` uses scipy to write `CERTIFICATES.json`: block-rank, zeroed-block and mixed weights,
and the `F₂` ILP minima. `verify.py` uses only the standard library. It recomputes every triple's tariffs
from the pinned archive and matching, then checks every certificate exactly. The ILP minima are
recorded local evidence, not re-solved.

```sh
python3.12 -B scripts/research_package.py --package research/three-plane-rank-dual --check-only
python3.12 -B scripts/research_package.py --package research/three-plane-rank-dual
```

The second command prints `PASS_THREE_PLANE_RANK_DUAL`. To regenerate the certificates, which
needs scipy 1.13 and numpy:

```sh
python3.12 -B research/three-plane-rank-dual/generate.py
```

## Open next questions

1. **Does a genuinely joint decoder over four or more planes stay at `Σ p_i²` under this contract?**
   - *Why it matters:* pair-local (#4365) and triple-local mixing are now both closed.
   - *First step:* the same LP over zero sets for plane quadruples. There are 367,290 quadruples, so
     group them into tariff classes first.
2. **Does a triple-local decoder improve the complete budget?** No: the COPY mass equals the
   separate-message mass on every triple. Any gain would need a changed contract.

## Credit

The contract, the two-plane theorem and the open question are CubeJerry's (#4365). The labels and
matching come from PR1825/2126 (Cinna Davis) and the retained matching certificate. Predecessor
screens are Gonzalo Mier's PR3990/3997.

Prepared by Ryanson Jonathan with substantial Claude Code (Claude Opus 5.5) assistance.
Apache-2.0.
