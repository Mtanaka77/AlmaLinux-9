# AlmaLinux-9 with Siesta-4.1b

## Test Linux - AlmaLinux-9, with Siesta-4.1b ##

As the successor of CentOS 7, we download the software AlmaLinux-9 
on the laptop PC, and test the linux for the Siesta-4.1b code [1].
The installation is done with AlmaLinux-9.4-x86_64-dvd.iso
of 10 GB memory.  That is finished in 15 minutes and then reboot
for Linux windows.

At the beginning, we open the gfotran by typing # gfortran -V, which
is necessary on AlmaLinux-9. We also type # pip -V to open PIP.
The MPI of mpich4 is downloaded and installed for the Siesta-4.1 code.

Then for Siesta-4, we download the Siesta-4.1b code and unpack by:  
% tar -zxvf siesta-4.1b.tar.gz. Before working on the Siesta code, 
it is necessary to install OpenBLAS and Scalapack.
For OpenBLAS, it is straight forward after a while.
However, for Scalapack, the installation went wrong although 
we type # make -i, since VirtualBox was used for which 
the AlmaLinux-9 showed unnecessary errors without success.
So for the being, the import of ./lib/libscalapack.a is borrowed from 
the fading away CentOS 7.

The arch.make file for the MPI case mpifort is the following:  
  .SUFFIXES:  
  .SUFFIXES: .f .F .o .c .a .f90 .F90  
  SIESTA_ARCH = gfortran-MPI  

  CC = mpicc  
  FPP = $(FC) -E -P -x c  
  FC = mpifort  

  MPI_INTERFACE = libmpi_f90.a  
  MPI_INCLUDE = .   

  FFLAGS = -O2 -fPIC -ftree-vectorize -march=native -fallow-argument-mismatch  
 #FFLAGS = -O2 -fexpensive-optimizations -ftree-vectorize -fprefetch-loop-arrays -march=native -fPIC -fopenmp  
  FC_SERIAL = gfortran  

  AR = ar  
  RANLIB = ranlib  
  SYS = nag  

  SP_KIND = 4  
  DP_KIND = 8  
  KINDS   = $(SP_KIND) $(DP_KIND)   
  
  FPPFLAGS = -DMPI   
  LDFLAGS  =  
  INCFLAGS =  
  INSDIR = /opt  
  COMP_LIBS =     # libsiestaLAPACK.a libsiestaBLAS.a  
  LDFLAGS += -L$(INSDIR)/openblas/lib -Wl,-rpath=$(INSDIR)/openblas/lib  
  LIBS = -lgomp -L/opt/openblas/lib -lopenblas  
  LIBS += -L/opt/scalapack/lib -lscalapack  

The Siesta-4.1b is installed by "make -i" because the error stop is avoided.
The test is shown in the Siesta-4.1bTest.pdf file [2].


## References

1. J. M. Soler et al., J. Phys. Cond. Matt. 14, 2745 (2002).

2. This PDF file of Siesta-4.1bTest.pdf. 
