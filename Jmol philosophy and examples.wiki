This is a tutorial for AI how to help with making 3D figures in Jmol. If it helps humans too, so much the better.

==My take on high-quality figures==

This is paraphrased from [[Image:Proteopedia_rubrics.pdf]].

A 3D figure should be content-rich, visually clear, fun to explore, and understandable. If it is part of a series of figures, they should work together well.

*Content-rich: If you can, tell a complete story in a single figure rather than having to click through multiple figures. Journal articles sometimes limit the number of figures to as low as 5, and so they are information-packed.

*Visually clear: There should be a clear sense of what the focus of the figure is (the foreground), and how this focal point connects to the whole (the background). There are multiple aspects that can help achieve a clear image (choice of initial zoom level, color, of representation, slabbing and fogging, line widths, initial orientation and center of rotation)

*Fun to explore: The viewer should be invited to rotate the figure and to zoom in and out. Interactive elements (hiding and displaying structural features, buttons to get back to the original view or to other standard views, animation, morphs) can guide this exploration, as well as [[Help:Viewing_pages|reminders]] about Proteopedia's and Jmol's built-in features (measure distances and angles, invitation to view in stereo, pop-out the image to maximize viewing area).

*Understandable: The viewer should be able to understand the visual elements (color coding, which representation for what) and the source of the data (PDB ID, typically) through the caption, legends on the viewing window, permanent and hover labels, and the text around the green link.

==Annotated scripts==
Here are some scripts. If you click on the title, you can see the resulting figure in the Jmol window above.

===Detailed views===
These have a focal point which should be centered and visible in the foreground. The focal point could be a ligand, an (empty) ligand binding site, an interface (e.g. dimer interface), a single interaction (e.g. disulfide bridge), or a single molecule. The background could be the entire remainder of the structure, or carefully selected parts of it, or blank (e.g. for a single molecule).

Typically, the foreground is shown at the detail of single atoms (represented by wireframe, spacefill, both "ball-and-stick", surfaces), and it is the center of rotation. Often, the background is shown with thinner lines, uniform color, or not showing single atoms but groups (represented by backbone, cartoon, meshribbon, ribbon, strand, trace, or surface). If the viewer is encouraged to rotate 360 degrees, the background should not interfere with seeing the foreground (use thin lines, slabbing, fading/fogging "zshade"). Transparency should be used with caution in Jmol because it can add visual clutter (spacefill and thick wireframe as well as cartoons add clutter, strands of constant thickness work well).

The <scene name='85/857774/Chymotrypsin/4'>initial scene</scene>: Chymotrypsin active site


 <nowiki>
load =7GCH.cif                                                # Chymotrypsin with covalently bound inhibitor, mmcif file from RCSB
restrict none                                                 # clean slate
define ~focal 57, 102, 195, LPF                               # foreground: catalytic triad and inhibitor
select LPF.C2 or 195.CB; bondOrder single;                    # ensure covalent bond
select ~focal; Spacefill 25%; wireframe 0.36                  # ball-and-stick for foreground
center selected; zoom 300;                                    # 
select LPF.CP1, LPF.CP2, LPF.CP3, LPF.CP4, LPF.CP5, LPF.CP6;  # aromatic ring of inhibitor
color atoms opaque [xda70d6];                                 # magenta
select LPF and not selected; color atoms opaque [xe6e6fa];    # rest of inhibitor: lavender???
select not ~focal; color chain;                               # default chain colors (pastel tones)
select protein and not ~focal; Spacefill 100%;                # background as spacefill
select 57.CD2, 102.OD1, 195.CB; label "%n %r";                # labelled atoms
color label [x000000]; font label 13 SansSerif Bold;          # white labels, chose font
select 57.CD2; set labelOffset -4 4;                          # each label has a different offset
select 102.OD1; set labelOffset 1 1;                          # 
select 195.CB; set labelOffset 0 -1;                          #
background black; set zshade on                               # the background is almost not visible affects zshade
moveto 0.0 { -97 855 -510 89.56} 300.0 0.0 0.0 {38.343720000000005 74.45217999999998 85.45410000000003} 34.78371098010064 {0 0 0} 0 0 0 3.0 0.0 0.0;
</nowiki>

<scene name='10/1076814/Etransfer/1'>Electron transfer</scene>

 <nowiki>
load "=1JRO" filter "biomolecule 2"                                   # The structure contains 2 dimers, we are loading the second dimer
restrict none
define ~focal FES:E, FAD:E, MTE:F, MOS:F                              # Ligands involved in electron transfer (in chain E, F, not G, H)
select (sulfur and MTE:F) or MOS:E.MO; bondOrder single               # show bond between sulfur atoms and molybdenum
select ~focal; wireframe 0.36; Spacefill 25%                          # ball-and-stick (atom colors default to CPK upon loading)
select 3001:E.S2 or 3002:E.FE1; label "Fe2S2"; color label [xffa500]; # label electron carriers
select FAD:E.O2; label "FAD"                                          # label electron donor (color as atom)
select MOS:F.MO; label "MoCo"                                         # label electron acceptor (color as atom)
select ~focal; set labelOffset 4 4;                                   # offset for all labels
center visible; zoom 400; spin on                                     # there is no good single view, so spin 
set measurementUnits Angstroms                                        # units for the measurements below
measure ([FAD]3005:E.C7M) ([FES]3002:E.S1)                            # shortest path for electrons
measure ([FES]3002:E.FE2) ([FES]3001:E.S2)
measure ([FES]3001:E.FE1) ([MTE]3003:F.C2) 
</nowiki>


<scene name='80/804504/Dna/5'>Base stacking</scene>


 <nowiki>
load *4c64                                                             # DNA structure, from RCSB
restrict none
define ~focal [DC]21:B or [DG]4:A                                      # G:C base pair 
select ~focal; wireframe 0.3;                                          # show entire nucleotide (with backbone)
select sidechain and 5:A, 3:A, 20:B, 22:B; wireframe 0.2; color gray   # stacking G:C and A:T base pair, just nucleobases
select 20:B or 5:A; color atoms opaque [xff7f50]; # coral              # A:T base pair in coral

# The following shows the focal base pair as surface, with coloring indicating distance of closest stacked atom
set defaultVDW JMOL                                                    # sets Van der Waals radii
contact ID "contact1" ({(21:B or 4:A) and sidechain}) ({sidechain and (5:A, 3:A, 20:B ,22:B)}) surface;
contact ID "contact1" fill noMesh noDots notFrontOnly frontlit;        # surface representation parameters
color $"contact1""roygb" range -0.5 1;                                 # color scheme rainbow, with overlap in VdW radii in red

# Here is how to find the neighboring nucleobases (which are given explicitly in the "contact" command above)
# select within(3.8, ~focal and sidechain) and not (~focal or water)
# define ~neighbors within(group, selected) and sidechain

center visible; zoom 250

</nowiki>


<scene name='10/1076814/Intercalation/1'>Intercalation</scene>

 <nowiki>
load =3k5y                                                                 # RNA bound to protein
restrict none
select protein and sidechain and within(3.5, RNA and sidechain);           # protein side chain atoms that are close to nucleobases
select within(group, selected) and (alpha or sidechain); wireframe 0.3;    # entire side chain plus alpha carbon shown
select selected and (alpha or *.CB); color bond gray;                      # fix bond color between alpha and beta carbon to uniform gray
select alpha; backbone on; color chain                                     # entire protein shown as thin backbone
select RNA and sidechain; Spacefill 100%; color atoms red;                 # Nucleobases shown as red spacefilling (this is the focus of the figure)
select RNA and mainchain; Spacefill 0.5;                                   # rest of RNA shown as ball...
select RNA; wireframe 0.3;                                                 # and stick (it's fine to show nucleobases as wireframe, too, gets occluded) 
center RNA; zoom 200                                                       # center of rotation is RNA so it stays in view
</nowiki>

<scene name='10/1076814/Nagal_galsa_superposition/1'>Superposition</scene>


<jmol>
  <jmolCheckbox>
    <scriptWhenUnChecked>animation off; model 0
       </scriptWhenUnChecked>
    <scriptWhenchecked>animation mode loop; animation on
       </scriptWhenchecked>
    <checked>false</checked> 
    <text>animation</text>
  </jmolCheckbox>
</jmol>


 <nowiki>
load files "=3H54" "=3LX9"                                                            # load 2 files, they are selected as 1.1 and 2.1 below
restrict none                                                                         # display is cleared
define ~act2 2.1 and (47,92,93,134,142,168,170,172,203,206,207,227,231) and *:A       # active site residues of 3LX9
define ~act1 1.1 and (33,78,79,119,127,154,156,158,188,191,192,213,217) and *:A       # active site residues of 3H54
compare {2.1} {1.1} SUBSET{*.CA} ATOMS{~act2}{~act1} ROTATE TRANSLATE                 # superposition (different sequences, so use alpha carbon)
set ssbonds SIDECHAIN                                                                 # disulfide bonds are anchored on SG, not on CA

set traceAlpha TRUE
set zshade on
set sheetSmoothing 0.3

# drawing 3H54 active site
select 1.1 and ~act1 and (sidechain or *.CA); wireframe 60;                           # show the active site as wireframe
color bonds cornflowerblue                                                            # 3H54 is shown in blue (just the bonds)
select selected and not _C; spacefill 80; color cpk                                   # spacefill, atoms other than carbon shown using CPK color scheme
select 1.1 and (32-34, 77-80, 118-120, 126-128, 153-159, 187-193, 212-218) and *:A.CA;# some main chain context for the active site residues 
trace 10; color cornflowerblue                                                        # show as trace in blue
select 1.1 and 1000:A; wireframe 60;                                                  # selecting the sugar in 3H54
select selected and _C; color skyblue;                                                # carbons in skyblue

# drawing 3LX9 active site
select 2.1 and ~act2 and (sidechain or *.CA); wireframe 60;                           # show the active site as wireframe 
color bonds yellow                                                                    # 3LX9 is shown in yellow (just the bonds)
select selected and not _C; spacefill 80; color cpk                                   # spacefill, atoms other than carbon shown using CPK color scheme
select 2.1 and (46-48, 91-94, 133-135, 141-143, 167-173, 202-208, 226-232) and *:A.CA;# some main chain context for the active site residues
trace 10; color yellow                                                                # show as trace in yellow 
select 2.1 and 1000:A; wireframe 60;;                                                 # selecting the sugar in 3LX9 
select selected and _C; color beige;                                                  # carbons in beige
center selected; zoom 300                                                             # center and zoom in
</nowiki>

And here are the commands for turning animation on or off (used in the checkbox above):

 <nowiki>
animation off; model 0                 # show both models
animation mode loop; animation on      # animate between the models
</nowiki>

<scene name='10/1076814/Trypsin_ryan_nini/2'>Trypsin</scene>
 <nowiki>
load =2PTC
restrict none
# active site and pocket of enzyme as well as 
# lysine and cystine of inhibitor are featured here
define ~focal 14:I, 15:I, 38:I, 57:E, 102:E, 195:E, 189:E             
select ~focal and (sidechain or alpha); wireframe 0.2; spacefill 0.4; # ball-and-stick for focus
select protein and not (1-15 and chain=I); backbone 0.5               # leave out 15-16 in chain=I
select protein and (1-15 and chain=I); backbone 0.5                   # this is a hack to ...
connect (15:I.CA)(16:I.CA) radius 0.52 create                         # get one backbone segment
select 15:I.CA or 16:I.CA; color bond white                           # in a different color
select nitrogen; color atoms opaque [x002da6];                        # CPK-like palette for side chains
select oxygen; color atoms opaque [xa6243a];
select sulfur; color atoms opaque [xffff00];

# slightly more intense color than backbone for carbons in side chains
select alpha and chain=E; color atoms opaque [x5a91b8];
select alpha and chain=I; color atoms opaque [xfb8a8a];
select ~focal and chain=E and carbon and sidechain; color atoms opaque [x96e1fb]
select ~focal and chain=E and (*.CA or *.CB); color bond [x96e1fb]
select ~focal and chain=I and carbon and sidechain; color atoms opaque [xfa6563];
select ~focal and chain=I and (*.CA or *.CB); color bond [xfa6563]

#hydrogen bonds in catalytic triad
select (57:E or 195:E or 102:E) and sidechain
set hbondsRasmol FALSE; calculate HBONDS; hbonds 0.1

#the dark purple background in combination with zShade gives good depth feeling for this color palette
background [x3d2b49];
set zShadePower 2; set zShade true;

#slab removes a loop that interferes with view into active site in the initial orientation
moveto 0.0 { 124 277 953 167.49} 300.0 0.0 0.0 {11.051 74.132 19.27} 38.73 {0 0 0} 0 0 0 3.0 0.0 0.0;
slab 60;depth 0;slab on;
</nowiki>



===Overall views===
These don't have a focal point. They should be centered on everything displayed. If using a representation you can look through (e.g. backbone or cartoon rather than surface or spacefill), fading/fogging "zshade" may be helpful to distinguish front and back.


<scene name='78/780454/Domains/7'>Domains</scene>

 <nowiki>
load *1d9z                                        # UvrB, a helicase-like protein bound to ATP, loaded from PDBe, note lower-case letters
restrict none
select protein; color gold; cartoon on            # The gold domain has most segments, so make everything gold
select not helix and not sheet; cartoon 0.3       # Make coils a bit thicker than default for balance and visibility
select 415-595; color red           
select 157-244; color blue
select 91-116; color aqua                         # beta hairpin, unique to UvrB
select 245-324, 349-378; color lime               # UvrA interaction domain
select MG, ATP, ZN; color silver; spacefill 100%  # Ligands in spacefill (Zn is crystallization artifact)
</nowiki>


<scene name='10/1071246/Trypsin_5mop/5'>Rainbow plus surface</scene>

 <nowiki>
load =5mop                                                                                # trypsin
restrict none
select protein; cartoon on; color group                                                   # rainbow cartoon
select protein and (57,195) and sidechain and not hydrogen; color cpk; spacefill 25%      # ball ...
select protein and (57,195) and (sidechain or alpha) and not hydrogen; wireframe 0.36     # ...and stick
select protein; isosurface ID "1" SASURFACE                                               # solvent-accessible surface
color isosurface translucent 0.875 white                                                  # white and translucent
isosurface ID "1" fill noMesh noDots notFrontOnly frontlit                                # these settings help to see the cartoon
select all; set hoverLabel "%n %R";                                                       # residue numbers and name
select [CA]; set hoverLabel "Calcium";                                                    # spell out "calcium" to distinguish from alpha-carbon
</nowiki>


