# NumPy 2.0 Migration - Completed

## Summary
PySPLIT has been successfully migrated to NumPy 2.x compatibility. The migration involved both source code changes and dependency updates.

## Changes Made

### 1. Source Code Fixes (19 string comparison fixes)

#### Critical: String Identity Comparison Bug Fixes
Replaced `is`/`is not` operators with `==`/`!=` for string comparisons. Using `is` for string comparison is a Python anti-pattern that can fail unpredictably.

**Files modified:**
- `pysplit/traj.py` - 6 fixes (lines 393, 397, 465, 497, 535, 627)
- `pysplit/mapmaker.py` - 7 fixes (lines 75, 82, 89, 91, 95, 263, 316)
- `pysplit/mapdesigner.py` - 5 fixes (lines 258, 272, 287, 298, 379)
- `pysplit/maplabeller.py` - 1 fix (line 256)

### 2. NumPy dtype Specifications (6 additions)

Added explicit `dtype=float` to `np.empty()` calls to ensure consistent behavior across NumPy versions.

**Files modified:**
- `pysplit/hyfile_handler.py` - 5 additions (lines 154, 155, 156, 276, 277)
- `pysplit/hypath.py` - 1 addition (line 131)

### 3. Dependency Upgrades

Updated all dependencies to NumPy 2.x compatible versions:

```bash
# Core dependencies
numpy: 1.26.4 → 2.4.2
pandas: 2.2.3 → 3.0.0
geopandas: upgraded with NumPy 2.x support
shapely: upgraded to 2.1.2

# Additional dependencies
pyarrow: 14.0.2 → 23.0.0
numexpr: 2.8.7 → 2.14.1
bottleneck: 1.3.7 → 1.6.0
cartopy: installed 0.25.0
matplotlib: 3.10.1 → 3.10.8
```

## Verification

All changes verified:
- ✅ Syntax validation - all modified files compile without errors
- ✅ Import test - PySPLIT imports successfully with NumPy 2.4.2
- ✅ Functionality test - key modules (Trajectory, TrajectoryGroup, hyfile_handler) work correctly

## The Original Issue

The original error:
```
numpy.core.multiarray failed to import
A module that was compiled using NumPy 1.x cannot be run in NumPy 2.x
```

This was caused by binary incompatibility between NumPy 1.x and 2.x. The solution required:
1. Upgrading NumPy to 2.x
2. Reinstalling all C-extension dependencies (pandas, geopandas, shapely, pyarrow, etc.) to get NumPy 2.x compatible binaries
3. Fixing source code compatibility issues

## Files Changed

```
pyproject.toml            |  2 +-  (already had numpy>=2.0.0)
pysplit/hyfile_handler.py | 10 +++++-----
pysplit/hypath.py         |  2 +-
pysplit/mapdesigner.py    | 10 +++++-----
pysplit/maplabeller.py    |  2 +-
pysplit/mapmaker.py       | 14 +++++++-------
pysplit/traj.py           | 12 ++++++------
7 files changed, 26 insertions(+), 26 deletions(-)
```

## Testing After Migration

To verify PySPLIT works with NumPy 2.x:

```python
import numpy as np
import pysplit

print(f'NumPy version: {np.__version__}')  # Should be >= 2.0.0
print(f'PySPLIT version: {pysplit.__version__}')

# Test imports
from pysplit import Trajectory, TrajectoryGroup
from pysplit.hyfile_handler import load_hysplitfile
print('✓ All imports successful')
```

## Notes

- The string comparison fixes are critical Python bug fixes, not just NumPy-related improvements
- The explicit dtype specifications ensure consistent behavior and better code clarity
- All NumPy functions used (np.unique, np.argsort, np.split, etc.) remain compatible with NumPy 2.x
- The `np.float64` usage found in the codebase is still valid in NumPy 2.x

## Date Completed
2026-02-05
