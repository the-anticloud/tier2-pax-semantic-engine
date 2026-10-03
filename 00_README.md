# PAX Semantic Engine — 321+ Pattern Detector System

**Status:** Production | **Version:** 2.0.0 | **Author:** PAX Research Team  
**Domain:** 0-1.gg/pax/semantic-engine

---

## What Is PAX Semantic Engine?

PAX Semantic Engine is a hand-coded symbolic reasoner with 321+ verified pattern detectors for abstract reasoning tasks. It operates without any LLM inference, achieving 51.75% on ARC-1 eval and 33.90% on ARC-2 training set through pure structural pattern matching.

The engine represents the open-source, non-LLM frontier of abstract visual reasoning and serves as a reference implementation for pattern-based problem solving.

```
ARC Task Input (grid, transformation rules)
    ↓
Pattern Detection Engine (321+ verified detectors)
    ↓
Rule Extraction → Hypothesis Generation → Output Grid
    ↓
ARC Leaderboard Submission
```

---

## Key Specifications

| Aspect | Details |
|--------|---------|
| **Detection Coverage** | 321+ verified pattern types |
| **LLM Inference Used** | 0 (pure symbolic reasoning) |
| **ARC-1 eval score** | 51.75% (207/400 tasks) |
| **ARC-2 train score** | 33.90% (339/1000 tasks) |
| **Average solve time** | 50-500ms per task |
| **Architecture** | Rule-based FSM + grid algebra |
| **Verification** | Cross-validated on historical test sets |
| **Completeness** | All basic transformations (rotation, reflection, scaling, color mapping) |

---

## Architecture

### Layer 1: Grid Parser
- 30x30 grid normalization (variable-size inputs)
- Color space standardization (0-9 color palette)
- Coordinate system canonicalization

### Layer 2: Pattern Detection (321+ detectors)

**Geometric Patterns (40+ detectors):**
- Rotation (90°, 180°, 270°)
- Reflection (vertical, horizontal, diagonal)
- Translation (fixed offset)
- Scaling (2x, 3x, fractional)
- Shearing and perspective

**Structural Patterns (60+ detectors):**
- Object isolation and extraction
- Bounding box operations
- Connected component analysis
- Boundary tracing
- Symmetry detection

**Color & Texture Patterns (50+ detectors):**
- Color flood-fill
- Gradient mapping
- Color histogram normalization
- Palette reordering
- Chromatic filtering

**Rule Inference Patterns (100+ detectors):**
- Arithmetic operations (addition, subtraction, multiplication)
- Logical operations (AND, OR, XOR on grids)
- Conditional transformations
- Sequence extrapolation
- Multi-step rule composition

**Learning Patterns (71+ detectors):**
- Analogy detection
- Schema extraction
- Inductive generalization
- Abductive hypothesis generation
- Cross-validation against train set

### Layer 3: Rule Engine
- Hypothesis ranking by confidence
- Conflict resolution (multiple matching patterns)
- Recursive rule application
- Output generation and validation

---

## Performance (Verified from PAX_RESULTS.md)

### ARC Benchmark Results
| Benchmark | Score | Config |
|-----------|-------|--------|
| **ARC-1 eval** | 51.75% (207/400) | All 321+ detectors active |
| **ARC-1 train** | 37.00% (148/400) | Test-time adaptation |
| **ARC-2 train** | 33.90% (339/1000) | Larger hypothesis space |
| **ARC-2 eval** | 1.67% (2/120) | Distribution shift (unseen patterns) |

### Key Achievement
- **Above best pure-symbolic open model** (Verantyx 16.1%)
- **Comparable to open non-LLM frontier** (ARChitects 8B TTT: 53.5%)
- **0% LLM dependency** (all reasoning is symbolic)

---

## Quick Start

### Installation
```bash
git clone https://github.com/0-1-gg/pax-semantic-engine.git
cd pax-semantic-engine
pip install -e .
```

### Basic Usage
```python
from pax_semantic import SemanticEngine

engine = SemanticEngine()

# Load ARC task
task = engine.load_arc_task("data/training/00d62c1b.json")

# Detect patterns
patterns = engine.detect_patterns(task["train"])
print(f"Found {len(patterns)} candidate patterns")

# Generate solution
solution = engine.solve(task, patterns)
print(f"Output grid: {solution}")
```

### CLI
```bash
pax-semantic solve data/training/00d62c1b.json
pax-semantic benchmark data/training/
pax-semantic stats --report=coverage
```

### Detector Selection
```python
# Use specific detector suite
engine = SemanticEngine(
    detectors=['geometric', 'color', 'rule_inference'],
    exclude=['expensive_recursive']  # For speed
)

# Fine-grained control
engine.enable_detector('pattern_scaling', confidence_threshold=0.8)
```

---

## Integration Points

### Primary Consumers (Tier 2)
- **PAX_ARC_SOLVER** — One of three lanes (semantic lane)
- **PAX_WORLD_MODEL** — Pattern hierarchy learning
- **PAX_INFERENCE_CORE** — Fallback for semantic pre-processing

### Complementary (Tier 1 & 3)
- **KASTERAN** — Type-safe pattern DSL compilation
- **KAZCADE** — GPU-accelerated grid operations
- **api-oss-labs** — Benchmark harness integration
- **ANTICODE_AGENT** — Code generation from patterns

### Deployment (Tier 3)
- **api-oss-hub** — Task storage and versioning
- **PAX_MONITOR_SYSTEM** — Pattern detection telemetry
- **PAX_BENCHMARK_SUITE** — Continuous evaluation

---

## Architecture: Multi-Detector Ensemble

```
ARC Task Input
    ├─→ Geometric Detectors (40 patterns) ─→ Score: 0.95
    ├─→ Color Detectors (50 patterns) ────→ Score: 0.87
    ├─→ Structural Detectors (60) ────────→ Score: 0.75
    ├─→ Rule Inference (100 patterns) ───→ Score: 0.92
    └─→ Learning Patterns (71 detectors) ─→ Score: 0.88
            ↓
    Hypothesis Ranking (Borda count)
            ↓
    Top 3 Candidates → Apply → Validate → Output
```

---

## Detector Library

### How to add new detectors
```python
from pax_semantic import Detector

class MyCustomDetector(Detector):
    name = "custom_spiral_pattern"
    confidence = 0.8
    
    def detect(self, grid):
        """Return True if pattern found, else False"""
        # Pattern logic here
        return self.is_spiral(grid)
    
    def apply(self, grid):
        """Apply transformation"""
        return self.spiral_transform(grid)

engine.register_detector(MyCustomDetector())
```

---

## Validation & Testing

- **Test suite:** 400 ARC-1 training tasks (verified against solutions)
- **Cross-validation:** 5-fold on training set
- **Detector verification:** Each detector validated independently
- **Regression suite:** Historical performance baseline

```bash
pytest tests/detectors/ -v --coverage
pytest tests/integration/arc_benchmark.py
```

---

## Roadmap

- **Q4 2026:** Multi-scale detector composition (hierarchical patterns)
- **Q1 2027:** GPU acceleration for grid operations (10x speedup)
- **Q2 2027:** Hybrid integration with language models (semantic + LLM blend)
- **Q3 2027:** Online learning from human feedback

---

## References

- **Benchmark Evidence:** `/pax-one-evidence/arc12/pax_semantic_arc1.json`
- **PAX_RESULTS.md:** Verification and SOTA comparison
- **ARC Dataset:** https://kaggle.com/c/arc-agi
- **GitHub:** github.com/0-1-gg/pax-semantic-engine
- **Docs:** 0-1.gg/pax/semantic-engine

---

**Next:** See APPENDIX/ for integration architectures and related systems
