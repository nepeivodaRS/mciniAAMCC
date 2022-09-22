# mcini
# Data format to store information about initial and final stages of relativistic nucleus-nucleus collisions

HOW TO:

1. build the library:
```
$ cd mcini
$ mkdir build
$ cd build
$ cmake ..
$ make
```
2. set up the environment:
```
$ . ../macro/config.sh
```
3. convert UrQMD \*.f20 files (OSCAR1999A format):
```
root '../macro/convertUrQMD.C ("path/to/input/file","path/to/output/file")'
```
4. run demo analysis macro:
```
root '../macro/demo.C ("path/to/input/file")'
```

DATA STRUCTURE (not that most event generators provide only part of the information):

URun (main settings of the event generator):
  * fGenerator                       - Generator description
  * fComment                         - Run comment
  * fDecayer                         - Decayer description
  * fAProj                           - Projectile mass number
  * fZProj                           - Projectile charge
  * fPProj                           - Projectile momentum per nucleon (GeV)
  * fATarg                           - Target mass number
  * fZTarg                           - Target charge
  * fPTarg                           - Target momentum per nucleon (GeV)
  * fBMin                            - Minimum impact parameter
  * fBMax                            - Maximum impact parameter
  * fBWeight                         - Impact parameter weighting (0 for geometrical weights (bdb), 1 for flat distribution)
  * fPhiMin                          - Event plane minimum angle (rad)
  * fPhiMax                          - Event plane maximum angle (rad)
  * fSigma                           - Cross-section (mb)
  * fNEvents                         - Requested number of events

EventInitialState (initial state information):
  * nColl                            - Number of binary collisions
  * nPart                            - Number of participant nucleons
  * nucleons                         - vector of initial state nucleons):
    - id                             - Unique index
    - pdgId                          - PDG code
    - momentum                       - 4-momentum (GeV)
    - position                       - 4-coordinates (fm)
    - collisionType                  - Type of collision (none, elastic with initial nucleon, elastic with produced particle, inelastic with initial nucleon, inelastic with produced particle)
    - collidedNucleonIndices         - Indeces of particles which encountered collisions with this nucleon

UEvent (mostly final state information):
  * fEventNr                         - Event number
  * fB                               - Impact parameter (fm)
  * fPhi                             - Event plane angle (rad)
  * fNes                             - Number of event steps
  * fStepNr                          - Event step number
  * fStepT                           - Event step time
  * fNpa                             - Number of particles
  * fParticles                       - array of final state particles):
    - fIndex                         - Unique index
    - fPdg                           - PDG code
    - fStatus                        - Status
    - fParent                        - Index of parent
    - fParentDecay                   - Parent decay index
    - fMate                          - Index of last collision partner
    - fDecay                         - Decay index (-1 if not decayed)
    - fChild[2]                      - Index of first and last child
    - fPx                            - Px (GeV)
    - fPy                            - Py (GeV)
    - fPz                            - Pz (GeV)
    - fE                             - Energy (GeV)
    - fX                             - X (fm)
    - fY                             - Y (fm)
    - fZ                             - Z (fm)
    - fT                             - T (fm)
    - fWeight                        - Weight 
