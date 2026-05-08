# Examples

---

## Hypothesis Smuggling (Rule 1 & 8)

The model added `hball` — a non-trivial regularity assumption about Hausdorff measure on balls — that is not part of the theorem statement being formalized. The theorem should hold under measurability and finiteness alone.

**Before:**

```lean
theorem theorem2_6_density_at_points_not_in_E
    {E : Set (Fin n → ℝ)}
    (hE_meas : MeasurableSet E)
    (hE_finite : (hausdorffMeasure s) E < ⊤)
    (hball : ∀ x : Fin n → ℝ,
      ∀ᶠ r in nhdsWithin (0 : ℝ) (Set.Ioi 0),
        (hausdorffMeasure s) (closedBall x r) = ENNReal.ofReal ((2 * r) ^ s)) :
    ∀ᵐ x ∂(hausdorffMeasure s).restrict Eᶜ,
      Tendsto (dimensional_density_ratio ((hausdorffMeasure s).restrict E) s x)
        (nhdsWithin (0 : ℝ) (Set.Ioi 0))
        (𝓝 (0 : ENNReal)) := by
  sorry
```

**After:**

```lean
theorem theorem2_6_density_at_points_not_in_E
    {E : Set (Fin n → ℝ)}
    (hE_meas : MeasurableSet E)
    (hE_finite : (hausdorffMeasure s) E < ⊤) :
    ∀ᵐ x ∂(hausdorffMeasure s).restrict Eᶜ,
      Tendsto (dimensional_density_ratio ((hausdorffMeasure s).restrict E) s x)
        (nhdsWithin (0 : ℝ) (Set.Ioi 0))
        (𝓝 (0 : ENNReal)) := by
  sorry
```

---

## Wrong Import Path (Rule 4)

The model wrote a plausible-looking import that does not exist in Mathlib. The module was reorganized; the real path is different.

**Before:**

```lean
import Mathlib.Topology.MetricSpace.Basic
```

**After:**

```lean
import Mathlib.Topology.EMetricSpace.Basic
```

---

## Hallucinated Lemma Name (Rule 3)

The model invented `MeasureTheory.hausdorffMeasure_ball_eq` from memory. No such lemma exists in Mathlib. The correct name, found via Loogle, is different.

**Before:**

```lean
rw [MeasureTheory.hausdorffMeasure_ball_eq]
```

**After:**

```lean
-- Use the Lean MCP to find lemmas
rw [MeasureTheory.hausdorffMeasure_closedBall]
```

---

## Heartbeat Override (Rule 6)

Raising `synthInstance.maxHeartbeats` papers over slow typeclass search instead of fixing it. The underlying problem is usually an ambiguous or expensive instance lookup that should be resolved explicitly.

**Before:**

```lean
set_option synthInstance.maxHeartbeats 20000

theorem foo [NormedAddCommGroup E] [InnerProductSpace ℝ E] : ... := by
  ...
```

**After:**

```lean
-- Removed the set_option. Instead, pin the slow instance explicitly
-- so synthesis does not have to search for it.
theorem foo [NormedAddCommGroup E] [InnerProductSpace ℝ E]
    (hfin : FiniteDimensional ℝ E) : ... := by
  haveI : ProperSpace E := FiniteDimensional.proper ℝ E
  ...
```
