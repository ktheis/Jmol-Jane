# Jmol Scripting Language Cheat Sheet

## Basic Syntax

- Commands are separated by semicolons or newlines
- Comments follow the `#` character
- Commands generally follow the pattern: `command parameter value`
- Drawing commands (wireframe, spacefill, cartoon, etc.) act on the most recent selection
- Color and label commands also act on the most recent selection

## Selection Commands

| Command | Description | Example |
|---------|-------------|---------|
| `select [expression]` | Creates a new selection | `select protein` |
| `define ~name [expression]` | Creates a named selection (~ reduces naming conflicts) | `define ~actsite (47,92,93,134)` |
| `selected` | References the current selection | `select within(4.0, selected)` |

## Selection Syntax

| Syntax | Description | Example |
|--------|-------------|---------|
| `[residue numbers]` | Select by residue number | `select 47,92,93,134` |
| `[residue range]` | Select a range of residues | `select 10-20` |
| `*:A` | Select chain A | `select *:A` |
| `*.CA` | Select all alpha carbon atoms | `select *.CA` |
| `_C` or `carbon` | Select by element | `select not _C` |
| `sidechain` | Select side chain atoms | `select sidechain` |
| `backbone` | Select backbone atoms | `select backbone` |
| `protein` | Select all protein atoms | `select protein` |
| `ligand` | Select all ligand atoms | `select ligand` |
| `water` | Select all water molecules | `select water` |
| `within(distance, atomSet)` | Select atoms within distance of atomSet | `select within(4.0, ligand)` |
| `within(group, atomSet)` | Expand selection to complete residues | `select within(group, selected)` |
| `[ligand code]:A` | Select specific ligand in chain A | `select a2g:A` |
| `and`, `or`, `not` | Logical operators | `select protein and not backbone` |

## Visualization Commands

| Command | Description | Example |
|---------|-------------|---------|
| `wireframe [thickness]` | Display bonds as lines | `wireframe 0.3` |
| `spacefill [radius]` | Display atoms as spheres | `spacefill 100%` |
| `backbone [thickness]` | Display alpha carbons | `backbone 0.4` |
| `cartoon` | Display protein/nucleic as cartoon | `cartoon` |
| `cartoon only` | Switch to cartoon, remove other representations | `cartoon only` |
| `trace [thickness]` | Display C-alpha trace | `trace 10` |
| `color bonds [color]` | Change wireframe color | `color bonds palegreen` |
| `color cpk` | Color by atom type | `color cpk` |

## Label and Hover Commands

| Command | Description | Example |
|---------|-------------|---------|
| `label "[format]"` | Add labels to selected atoms | `label "%m%r"` |
| `set labelOffset [x] [y]` | Adjust label position | `set labelOffset 0 0` |
| `color label [color]` | Set label color | `color label yellow` |
| `font label [size]` | Set label font size | `font label 14` |
| `hover "[format]"` | Set hover text | `hover "%n %r, Chain=%c"` |
| `set fontScaling [ON/OFF]` | Toggle dynamic font sizing | `set fontScaling ON` |

## Format Variables for Labels/Hover

| Variable | Description |
|----------|-------------|
| `%m` | Residue name |
| `%r` | Residue number |
| `%c` | Chain identifier |
| `%n` | Atom name |
| `%e` | Element symbol |

## Display Settings

| Command | Description | Example |
|---------|-------------|---------|
| `color background [color]` | Set background color | `color background white` |
| `slab [value]` | Set front clipping plane | `slab 60` |
| `depth [value]` | Set back clipping plane | `depth 20` |
| `slab on` | Enable slabbing | `slab on` |
| `set zShade [true/false]` | Enable depth cueing | `set zShade true` |
| `set ssbondsbackbone [true/false]` | Set disulfide bond display | `set ssbondsbackbone false` |
| `calculate HBONDS` | Calculate hydrogen bonds | `calculate HBONDS` |
| `set hbondsRasmol [TRUE/FALSE]` | Control H-bond calculation method | `set hbondsRasmol FALSE` |

## File and Model Commands

| Command | Description | Example |
|---------|-------------|---------|
| `load =[PDB ID]` | Load structure from RCSB| `load =3HG5` |
| `load =[PDB ID].cif` | Load mmcif structure from RCSB| `load =8Z2I.cif` |
| `load [path]` | Load structure from disk | `load 3HG5.pdb` |
| `load files "[path1]" "[path2]"` | Load multiple structures | `load files "=3HG5" "=3H54"` |
| `model [number]` | Show specified model | `model 1` |
| `center [expression]` | Center view on selection | `center selected` |

## Structure Comparison

| Command | Description | Example |
|---------|-------------|---------|
| `compare {model1} {model2} SUBSET{atomSet} ATOMS{set1}{set2} ROTATE TRANSLATE` | Superimpose structures | `compare {2.1} {1.1} SUBSET{*.CA} ATOMS{~actnagal and *.CA}{~actgal and *.CA} ROTATE TRANSLATE` |

## Common Colors

- `cpk` - Standard atom colors gray (C), white (H), red (O), blue (N) etc
- `green`
- `cornflowerblue`
- Hexadecimal colors: `[x800080]` (purple)


## Tips and Best Practices

1. Use semicolons or newlines to separate commands for clarity
2. Add comments after `#` to document your scripts (and don't use semicolon in comment)
3. Build selections step by step rather than using deeply nested expressions
4. Use "selected" to refer to most recent atom selection
5. Use named selections with `define ~name` for complex or frequently used selections
6. Prefer explicit selection expansion with separate commands rather than nested syntax:
   ```
   # Prefer this:
   select within(4.0, ligand); 
   select within(group, selected);
   
   # Over this:
   select within(group(within(4.0, ligand)));
   ```
7. Set font scaling before drawing labels: `set fontScaling ON`
8. When a molecule is loaded, it is shown in some default representation. Use `restrict none` to clear the canvas
9. Atom colors dynamically determine the color of spacefill. C-alpha atom colors dynamically determine the color of residue-level representations such backbone, cartoon, ribbon or trace. Atom colors also dynamically determine the color of wireframe, unless overridden by a "color bonds [color specification]" statement.


