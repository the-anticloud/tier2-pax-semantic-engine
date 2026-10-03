# Deploy Guide — PAX_SEMANTIC_ENGINE
**Stack:** Python 3.11, spaCy, PAX 27B, networkx, AIOSS_FORMAT | Air-gap capable

## Prerequisites
Anticloud core stack installed. PAX 27B weights (pax-27b-q4.gguf). AIOSS_FORMAT.

## Install
```bash
pip install anticloud-pax-semantic-engine
```

## AIOSS Integration
```bash
aioss init --module PAX_SEMANTIC_ENGINE --output ./pax_semantic_engine.aioss
```

## Air-Gap
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(model_path="./pax-27b-q4.gguf", module="PAX_SEMANTIC_ENGINE",
                     aioss_chain="./pax_semantic_engine.aioss",
                     classification="L5_NARROW_L2_GENERAL")
```

## Verification
```bash
aioss verify --chain ./pax_semantic_engine.aioss --verbose
python -m pax_semantic_engine.tests.smoke
```
