# LHAPDF5to6 Migration

This repository combines the last version of LHAPDF5 and the migration scripts from LHAPDF6. All credits go to the original developers of LHAPDF. 

There's a container avaliable (using CentOS 8) with older environments and a compiled LHAPDF5.9.1. 

## usage
for converting old `lhapdf5` pdfsets to `lhapdf6` format, modified for `GRV98` sets. migration scripts under `/root/lhapdf-5.9.1/migration/`. `GRV98` sets have been recorded in `tomigrate`, so using 
```
cat tomigrate | grep -E -v "^#" | grep GRV | while read l; do eval "./creategrids $l"; done
```
is fine. 

Note that `creategrids` only works with PDF sets under LHAPDFsets path, which is `/usr/local/share/lhapdf/PDFsets/` in this configuration. 

Instruction for installing the image (if you are using fedora, replace `docker` with `podman` or `dnf install podman-docker`. if you encounter any problem with root privilege, just prepend `sudo`): 
```
docker pull turgongondolin/centos8-lhapdf5
docker run -it centos8-lhapdf5 bash
```
for rootless setup, checkout [docker_rootless](https://docs.docker.com/engine/security/rootless/) or [podman_rootless](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md).

The Les Houche Accord Parton Density Function library - LHAPDF
==============================================================

Introdution.
===========
The Les Houches Accord Parton Density Function interface originated at the 
Les Houches 2001 workshop [1] in the PDF working group to enable the usage 
of Parton Density Functions with uncertainties in a uniform manner. 
When PDFs with uncertainties are considered, a ``fit'' to the data no longer 
is described by a single PDF. Instead in its most flexible implementation, 
a fit is represented by a PDF set consisting of many individual members. 
Calculating the observable for all the PDF members enables one to reconstruct 
the uncertainty on the observable. The LHAPDF interface was made with this 
in mind and manipulates PDF sets.

The LHAPDF interface can be viewed as a successor to PDFLIB [2]. 

Many improvements were added, to list some of the features:
    * The handling of PDF sets to enable PDF fits that include uncertainties.
    * All PDF sets are defined through external files in either parameterized or 
      grid form. The parameterized files are compact compared to the grid files, 
      but the latter allow faster usage when multiple PDF sets are called frequently 
      and also allow older PDF sets to be made available in LHAPDF. New PDF sets can 
      be defined by constructing the PDF defining parameter or grid files. The actual 
      LHAPDF code does not have to be changed unless, in the grid file case, the 
      authors of the PDFs use new interpolation methods.
    * The LHAPDF code is modular and default choices can be easily altered.

Note that the current ``best fit'' PDFs can be viewed as sets with one member 
PDF and can be easily defined through the PDF set external file definition. 
Alternatively one can group these ``fits'' is single sets (e.g. MRST98 set) as 
they often represent best fits given different model assumptions and as such 
reflect theoretical modeling uncertainties. 

Mailing List.
=============
A mailing list 'lhapdf-announce' has been set up to inform users of any 
changes and new versions of the LHAPDF library. If you would like to be on 
this list then please send a message to majordomo@cedar.ac.uk with the 
following words in the body of the message: 'subscribe lhapdf-announce'

You will receive by reply a message containing a subscribe string with a 
password which you should resend to majordomo@cedar.ac.uk to confirm the 
request.

(If you wish to subscribe a different mail address then use: 
'subscribe lhapdf-announce email@address ')

Downloading LHAPDF.
==================

From the LHAPDF web site ....http://projects.hepforge.org/lhapdf/

Installing LHAPDF.
=================

See the manual on the web site or the file INSTALL 
Simplest way is:

configure --prefix=/where/you/want/it/to/go/
make
make install

Interfacing LHAPDF with your code.
=================================

LHAPDF is written is standard Fortran77 with double precision variables and v5.3.0 
has been tested with the g77, f95 and gfortran compilers, and also with MacOSX. 

Interfacing can be done in one of two ways:

1) directly using the LHAPDF routines
2) using the LHAGLUE interace to LHAPDF which provides a PDFLIB type access.

The main LHAPDF routines are:

call InitPDFset(name)
call InitPDF(mem)
call evolvePDF(x,Q,f)
call evolvePDFp(x,Q2,P2,IP2,f)
function alphasPDF(Q)
call GetLam4(mem,qcdl4)
call GetLam5(mem,qcdl5)

The main LHAGLUE routines are:

call PDFSET(parm,value)
call STRUCTM(X,Q,UPV,DNV,USEA,DSEA,STR,CHM,BOT,TOP,GLU)
call STRUCTP(X,Q2,P2,IP2,UPV,DNV,USEA,DSEA,STR,CHM,BOT,TOP,GLU)
call PDFSTA

Details of how to use these and many others are available in the on line manual
on the LHAPDF web site -  http://projects.hepforge.org/lhapdf/


Interfacing LHAPDF with C++ code.
=================================

There is code within LHAPDF to interface with C++ programmes in the form of 
a wrapper LHAPDFWrap. The code is downloaded in the tarball and built into
a wrapper library in the make install procedure.  The wrapper code is also
available separately from the lhapdf web site http://projects.hepforge.org/lhapdf/
