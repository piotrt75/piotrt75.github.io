## FXFEL Project Documentation
# Welcome to FXFEL Project Documentation.
The FXFEL project consists of small number of handshaking Pythons scripts are a set of routines that alter the format of data in well-defined ways, without changing the information content. These routines are included in the UKFEL suite for two separate reasons:
* The suite contains simulator programs from various sources. ASTRA [2], ELEGANT [3] and VISIT [4] were developed externally. MASP and PUFFIN were developed ‘in-house’ by the UKFEL team. In general, these programs all use different, non-standard data formats for their input and output, and it would not be appropriate for us to modify them to conform to a common format. Some of the hand-shaking routines do the conversions needed to connect the output of one program (say ASTRA) to the input of another, such as MASP.
* All graphics and visualisations generated by the UKFEL suite are displayed by VISIT, a general-purpose graphics program. To obtain suitable displays, the data supplied to VISIT from the other modules must be conditioned in various ways, depending on the type of display needed. For example, certain types of display may need averages or cumulative totals, and these can be provided by routines that do the calculation and modify the data accordingly, before it is supplied to VISIT.
The UKFEL suite includes a library of conditioning routines which will serve for most purposes. We also supply instructions and examples to show how to write new routines, using PYTHON. We have adopted HDF5 as a ‘base’ format. When representing an electron beam, each record represents for a macro-particle, which is a group of electrons with identical properties. The properties are:
* Position in three orthogonal space coordinates X, Y and Z. The principal direction of motion is in the Z axis.
* Momentum in the three directions : P x, P y and P z. Electrons are usually travelling at near-light speeds, and the momenta take account of the consequent increase in mass.
* The number of electrons in the particle.
The values of length are in SI units (meters) but momentum is in scaled units (divided over (m · c)).


# Conversion Scripts and SU5 file format – user manual The purpose of the scripts

The main aim of creating the scripts was to allow converting particle sets between different software packages dealing with FEL and accelerator simulations (i.e. Astra, Elegant, Puffin, Vsim). For maximum portability the scripts are written in Python and don’t use system specific commands. The script use SU5 format as ’common format’ for exchange. The scripts don’t transfer data directly between packages but they convert data either into SU5 or from SU5
format – such attempt reduced number of possible configuration and in future allows easier expanding of format converters library.

# Requirements
Scripts are written in Python 2.7 with use of numpy and table modules. In order to use them you need to install Python 2.7 (Windows, Linux) and above modules. The other requirement is SDDSToolkit which is used by script dealing with Elegant and Puffin data manipulation. The scripts (except SU2CDF) use one core only and sometimes lot of memory – the size of the file you want to convert usually determine the amount of necessary memory.

# Installation
The installation is simple – just copy the scripts to directory where you intend to use them or create access path in your system.

# SU5 file format
The SU5 file format is HDF5 file which contains particle positions and momentum in common array (the dataset has class array). The table is built of 7 columns: x, px, y, py, z, pz and N e. N e column defines number of electrons per each record (macroparticle). All used units are SI (m, kg, s) but momentum is scaled i.e. divided over (m · c) – such approach reduces the problem of having tiny values which may cause problem. There is no reference particle and the
coordinates are in absolute space. The class of the array is ’table’ and this is due to restrictions from VisIt which is unable to handle properly other classes (fails to open). There is also just one dataset in the file by default. If user wants to pass more datasets then they should have
different name and the user is also supposed to take extra care about metadata. The below table show how the SU5 file should like when opened in ’hdfview’ or other similar program:

| X | Px | Y | Py | Z | Pz | Ne |
| -- | -- | -- | -- | -- | -- | -- |
| 1 | 0.1 | 2 | 0.1 | 1 | 10 | 1 |
| ... | ... | ... | ... | ... | ... | ... |
| 1 | 0.15 | 2 | 0.1 | 1.2 | 11 | 1.3 |
