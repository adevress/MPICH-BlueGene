# MPICH Release %VERSION%

MPICH is a high-performance and widely portable implementation of the
MPI-3.0 standard from the Argonne National Laboratory.  This release
has all MPI 3.0 functions and features required by the standard with
the exception of support for the "external32" portable I/O format and
user-defined data representations for I/O.

This README file should contain enough information to get you started
with MPICH. More extensive installation and user guides can be found
in the doc/installguide/install.pdf and doc/userguide/user.pdf files
respectively. Additional information regarding the contents of the
release can be found in the CHANGES file in the top-level directory,
and in the RELEASE_NOTES file, where certain restrictions are
detailed. Finally, the MPICH web site, http://www.mpich.org, contains
information on bug fixes and new releases.


1.  Getting Started
2.  Reporting Installation or Usage Problems
3.  Compiler Flags
4.  Alternate Channels and Devices
5.  Alternate Process Managers
6.  Alternate Configure Options
7.  Testing the MPICH installation
8.  Fault Tolerance
9.  Developer Builds
10. Multiple Fortran compiler support
11. ABI Compatibility


*********************************************************************

## 1. Getting Started


The following instructions take you through a sequence of steps to get
the default configuration (ch3 device, nemesis channel (with TCP and
shared memory), Hydra process management) of MPICH up and running.

(a) You will need the following prerequisites.

- REQUIRED: This tar file mpich-%VERSION%.tar.gz

- REQUIRED: A C compiler (gcc is sufficient)

- OPTIONAL: A C++ compiler, if C++ applications are to be used (g++,
  etc.). If you do not require support for C++ applications, you can
  disable this support using the configure option `--disable-cxx`
  (configuring MPICH is described in
  step 1(d) below).

- OPTIONAL: A Fortran compiler, if Fortran applications are to be used
  (gfortran, ifort, etc.). If you do not require support for Fortran
  applications, you can disable this support using --disable-fortran
  (configuring MPICH is described in step 1(d) below).

Also, you need to know what shell you are using since different shell
has different command syntax. Command "echo $SHELL" prints out the
current shell used by your terminal program.

(b) Unpack the tar file and go to the top level directory:

      tar xzf mpich-%VERSION%.tar.gz
      cd mpich-%VERSION%

If your tar doesn't accept the z option, use

      gunzip mpich-%VERSION%.tar.gz
      tar xf mpich-%VERSION%.tar
      cd mpich-%VERSION%

(c) Choose an installation directory, say
`/home/<USERNAME>/mpich-install`, which is assumed to non-existent or
empty. It will be most convenient if this directory is shared by all of
the machines where you intend to run processes. If not, you will have to
duplicate it on the other machines after installation.

(d) Configure MPICH specifying the installation directory:

for csh and tcsh:

        ./configure --prefix=/home/<USERNAME>/mpich-install |& tee c.txt

for bash and sh:

        ./configure --prefix=/home/<USERNAME>/mpich-install 2>&1 | tee c.txt

Bourne-like shells, sh and bash, accept "2>&1 |".  Csh-like shell, csh
and tcsh, accept "|&". If a failure occurs, the configure command will
display the error. Most errors are straight-forward to follow. For
example, if the configure command fails with:

  > "No Fortran compiler found. If you don't need to build any
  >  Fortran programs, you can disable Fortran support using
  >  --disable-fortran. If you do want to build Fortran programs,
  >  you need to install a Fortran compiler such as gfortran or
  >  ifort before you can proceed."

... it means that you don't have a Fortran compiler :-). You will need
to either install one, or disable Fortran support in MPICH.

If you are unable to understand what went wrong, please go to step (2)
below, for reporting the issue to the MPICH developers and other users.

(e) Build MPICH:

  for csh and tcsh:

       make |& tee m.txt

  for bash and sh:

       make 2>&1 | tee m.txt

  This step should succeed if there were no problems with the
  preceding step. Check file m.txt. If there were problems, do a
  "make clean" and then run make again with V=1.

       make V=1 |& tee m.txt       (for csh and tcsh)

  OR

       make V=1 2>&1 | tee m.txt   (for bash and sh)

  Then go to step (2) below, for reporting the issue to the MPICH
  developers and other users.

(f) Install the MPICH commands:

  for csh and tcsh:

       make install |& tee mi.txt

  for bash and sh:

       make install 2>&1 | tee mi.txt

  This step collects all required executables and scripts in the bin
  subdirectory of the directory specified by the prefix argument to
  configure.

(g) Add the bin subdirectory of the installation directory to your
    path in your startup script (.bashrc for bash, .cshrc for csh,
    etc.):

  for csh and tcsh:

       setenv PATH /home/<USERNAME>/mpich-install/bin:$PATH

  for bash and sh:
  
       PATH=/home/<USERNAME>/mpich-install/bin:$PATH ; export PATH

  Check that everything is in order at this point by doing:

       which mpicc
       which mpiexec

  These commands should display the path to your bin subdirectory of
    your install directory.

  IMPORTANT NOTE: The install directory has to be visible at exactly the
  same path on all machines you want to run your applications on. This
  is typically achieved by installing MPICH on a shared NFS file-system.
  If you do not have a shared NFS directory, you will need to manually
  copy the install directory to all machines at exactly the same
  location.

(h) MPICH uses a process manager for starting MPI applications. The
    process manager provides the "mpiexec" executable, together with
    other utility executables. MPICH comes packaged with multiple
    process managers; the default is called Hydra.

    Now we will run an MPI job, using the mpiexec command as specified
    in the MPI standard. There are some examples in the install
    directory, which you have already put in your path, as well as in
    the directory mpich-%VERSION%/examples. One of them is the classic
    CPI example, which computes the value of pi by numerical
    integration in parallel.

    To run the CPI example with 'n' processes on your local machine,
    you can use:

       mpiexec -n <number> ./examples/cpi

    Test that you can run an 'n' process CPI job on multiple nodes:

       mpiexec -f machinefile -n <number> ./examples/cpi

    The 'machinefile' is of the form:

       host1
       host2:2
       host3:4   # Random comments
       host4:1

    'host1', 'host2', 'host3' and 'host4' are the hostnames of the
    machines you want to run the job on. The ':2', ':4', ':1' segments
    depict the number of processes you want to run on each node. If
    nothing is specified, ':1' is assumed.

    More details on interacting with Hydra can be found at
    http://wiki.mpich.org/mpich/index.php/Using_the_Hydra_Process_Manager

If you have completed all of the above steps, you have successfully
installed MPICH and run an MPI example.

-------------------------------------------------------------------------

## 2. Reporting Installation or Usage Problems

[VERY IMPORTANT: PLEASE COMPRESS ALL FILES BEFORE SENDING THEM TO
US. DO NOT SPAM THE MAILING LIST WITH LARGE ATTACHMENTS.]

The distribution has been tested by us on a variety of machines in our
environments as well as our partner institutes. If you have problems
with the installation or usage of MPICH, please follow these steps:

1. First see the Frequently Asked Questions (FAQ) page at
http://wiki.mpich.org/mpich/index.php/Frequently_Asked_Questions to
see if the problem you are facing has a simple solution. Many common
problems and their solutions are listed here.

2. If you cannot find an answer on the FAQ page, look through previous
email threads on the discuss@mpich.org mailing list archive
(https://lists.mpich.org/mailman/listinfo/discuss). It is likely
someone else had a similar problem, which has already been resolved
before.

3. If neither of the above steps work, please send an email to
discuss@mpich.org. You need to subscribe to this list
(https://lists.mpich.org/mailman/listinfo/discuss) before sending an
email.

Your email should contain the following files.  ONCE AGAIN, PLEASE
COMPRESS BEFORE SENDING, AS THE FILES CAN BE LARGE.  Note that,
depending on which step the build failed, some of the files might not
exist.

       mpich-%VERSION%/c.txt (generated in step 1(d) above)
       mpich-%VERSION%/m.txt (generated in step 1(e) above)
       mpich-%VERSION%/mi.txt (generated in step 1(f) above)
       mpich-%VERSION%/config.log (generated in step 1(d) above)
       mpich-%VERSION%/src/openpa/config.log (generated in step 1(d) above)
       mpich-%VERSION%/src/mpl/config.log (generated in step 1(d) above)
       mpich-%VERSION%/src/pm/hydra/config.log (generated in step 1(d) above)
       mpich-%VERSION%/src/pm/hydra/tools/topo/hwloc/hwloc/config.log (generated in step 1(d) above)

    DID WE MENTION? DO NOT FORGET TO COMPRESS THESE FILES!

If you have compiled MPICH and are having trouble running an
application, please provide the output of the following command in
your email.

       mpiexec -info

Finally, please include the actual error you are seeing when running
the application, including the mpiexec command used, and the host
file. If possible, please try to reproduce the error with a smaller
application or benchmark and send that along in your bug report.

4. If you have found a bug in MPICH, we request that you report it at
our bug tracking system:
(https://trac.mpich.org/projects/mpich/newticket). Even if you believe
you have found a bug, we recommend you sending an email to
discuss@mpich.org first.


-------------------------------------------------------------------------

## 3. Compiler Flags

MPICH allows several sets of compiler flags to be used. The first
three sets are configure-time options for MPICH, while the fourth is
only relevant when compiling applications with mpicc and friends.

(a) `CFLAGS`, `CPPFLAGS`, `CXXFLAGS`, `FFLAGS`, `FCFLAGS`, `LDFLAGS` and `LIBS`
(abbreviated as xFLAGS): Setting these flags would result in the
MPICH library being compiled/linked with these flags and the flags
internally being used in mpicc and friends.

(b) `MPICHLIB_CFLAGS`, `MPICHLIB_CPPFLAGS`, `MPICHLIB_CXXFLAGS`,
`MPICHLIB_FFLAGS`, and `MPICHLIB_FCFLAGS` (abbreviated as
MPICHLIB_xFLAGS): Setting these flags would result in the MPICH
library being compiled with these flags.  However, these flags will
*not* be used by mpicc and friends.

(c) `MPICH_MAKE_CFLAGS`: Setting these flags would result in MPICH's
configure tests to not use these flags, but the makefile's to use
them. This is a temporary hack for certain cases that advanced
developers might be interested in, but which break existing configure
tests (e.g., -Werror). These are NOT recommended for regular users.

(d) `MPICH_MPICC_CFLAGS`, `MPICH_MPICC_CPPFLAGS`, `MPICH_MPICC_LDFLAGS`,
`MPICH_MPICC_LIBS`, and so on for MPICXX, MPIF77 and MPIFORT
(abbreviated as `MPICH_MPIX_FLAGS`): These flags do *not* affect the
compilation of the MPICH library itself, but will be internally used
by mpicc and friends.


    +--------------------------------------------------------------------+
    |                    |                      |                        |
    |                    |    MPICH library     |    mpicc and friends   |
    |                    |                      |                        |
    +--------------------+----------------------+------------------------+
    |                    |                      |                        |
    |     xFLAGS         |         Yes          |           Yes          |
    |                    |                      |                        |
    +--------------------+----------------------+------------------------+
    |                    |                      |                        |
    |  MPICHLIB_xFLAGS   |         Yes          |           No           |
    |                    |                      |                        |
    +--------------------+----------------------+------------------------+
    |                    |                      |                        |
    | MPICH_MAKE_xFLAGS  |         Yes          |           No           |
    |                    |                      |                        |
    +--------------------+----------------------+------------------------+
    |                    |                      |                        |
    | MPICH_MPIX_FLAGS   |         No           |           Yes          |
    |                    |                      |                        |
    +--------------------+----------------------+------------------------+


All these flags can be set as part of configure command or through
environment variables.


### Default flags

By default, MPICH automatically adds certain compiler optimizations
to MPICHLIB_CFLAGS. The currently used optimization level is -O2.

** IMPORTANT NOTE: Remember that this only affects the compilation of
the MPICH library and is not used in the wrappers (mpicc and friends)
that are used to compile your applications or other libraries.

This optimization level can be changed with the --enable-fast option
passed to configure. For example, to build an MPICH environment with
-O3 for all language bindings, one can simply do:

       ./configure --enable-fast=O3

Or to disable all compiler optimizations, one can do:

       ./configure --disable-fast

For more details of `--enable-fast`, see the output of `configure
--help`.

For performance testing, we recommend the following flags:

       ./configure --enable-fast=O3,ndebug --disable-error-checking --without-timing \
              --without-mpit-pvars


### Examples

Example 1:

       ./configure --disable-fast MPICHLIB_CFLAGS=-O3 MPICHLIB_FFLAGS=-O3 \
           MPICHLIB_CXXFLAGS=-O3 MPICHLIB_FCFLAGS=-O3

This will cause the MPICH libraries to be built with -O3, and -O3
will *not* be included in the mpicc and other MPI wrapper script.

Example 2:

       ./configure --disable-fast CFLAGS=-O3 FFLAGS=-O3 CXXFLAGS=-O3 FCFLAGS=-O3

This will cause the MPICH libraries to be built with -O3, and -O3
will be included in the mpicc and other MPI wrapper script.

Example 3:

There are certain compiler flags that should not be used with MPICH's
configure, e.g. gcc's -Werror, which would confuse configure and cause
certain configure tests to fail to detect the correct system features.
To use -Werror in building MPICH libraries, you can pass the compiler
flags during the make step through the Makefile variable
MPICH_MAKE_CFLAGS as follows:

       make MPICH_MAKE_CFLAGS="-Wall -Werror"

The content of MPICH_MAKE_CFLAGS is appended to the CFLAGS in all
relevant Makefiles.

-------------------------------------------------------------------------

## 4. Alternate Channels and Devices

The communication mechanisms in MPICH are called "devices". MPICH
supports ch3 (default), as well as many third-party devices that are
released and maintained by other institutes such as osu_ch3 (from Ohio
State University for InfiniBand and iWARP), ch_mx (from Myricom for
Myrinet MX), etc.

*************************************

### ch3 device

The ch3 device contains different internal communication options
called "channels". We currently support nemesis (default) and sock
channels.

#### nemesis channel
Nemesis provides communication using different networks (tcp, mx) as
well as various shared-memory optimizations. To configure MPICH with
nemesis, you can use the following configure option:

    --with-device=ch3:nemesis

The TCP network module gets configured in by default. To specify a
different network module such as MX, you can use:

    --with-device=ch3:nemesis:mx

If the MX include files and libraries are not in the normal search
paths, you can specify them with the following options:

    --with-mx-include= and --with-mx-lib=

... or the if lib/ and include/ are in the same directory, you can use
the following option:

    --with-mx=

If the MX libraries are shared libraries, they need to be in the
shared library search path. This can be done by adding the path to
/etc/ld.so.conf, or by setting the LD_LIBRARY_PATH variable in your
.bashrc (or .tcshrc) file.  It's also possible to set the shared
library search path in the binary. If you're using gcc, you can do
this by adding

    LD_LIBRARY_PATH=/path/to/lib

  (and)

    LDFLAGS="-Wl,-rpath -Wl,/path/to/lib"

... as arguments to configure.

By default, MX allows for only eight endpoints per node causing
ch3:nemesis:mx to give initialization errors with greater than 8
processes on the same node (this is an MX error and not an inherent
limitation in the MPICH/Nemesis design). If needed, this can be set
to a higher number when MX is loaded. We recommend the user to contact
help@myri.com for details on how to do this.

Shared-memory optimizations are enabled by default to improve
performance for multi-processor/multi-core platforms. They can be
disabled (at the cost of performance) either by setting the
environment variable MPICH_NO_LOCAL to 1, or using the following
configure option:

    --enable-nemesis-dbg-nolocal

The --with-shared-memory= configure option allows you to choose how
Nemesis allocates shared memory.  The options are "auto", "sysv", and
"mmap".  Using "sysv" will allocate shared memory using the System V
shmget(), shmat(), etc. functions.  Using "mmap" will allocate shared
memory by creating a file (in /dev/shm if it exists, otherwise /tmp),
then mmap() the file.  The default is "auto". Note that System V
shared memory has limits on the size of shared memory segments so
using this for Nemesis may limit the number of processes that can be
started on a single node.

#### mxm network module

The mxm netmod provides support for Mellanox InfiniBand adapters.  It
can be built with the following configure option:

    --with-device=ch3:nemesis:mxm

If your MXM library is installed in a non-standard location, you might
need to help configure find it using the following configure option
(assuming the libraries are present in /path/to/mxm/lib and the
include headers are present in /path/to/mxm/include):

    --with-mxm=/path/to/mxm

(or)

    --with-mxm-lib=/path/to/mxm/lib
    --with-mxm-include=/path/to/mxm/include

By default, the mxm library throws warnings when the system does not
enable certain features that might hurt performance.  These are
important warnings that might cause performance degradation on your
system.  But you might need root privileges to fix some of them.  If
you would like to disable such warnings, you can set the MXM log level
to "error" instead of the default "warn" by using:

    MXM_LOG_LEVEL=error
    export MXM_LOG_LEVEL


#### portals4 network module
The portals4 netmod provides support for the Portals 4 network
programming interface. To enable, configure with the following option:

    --with-device=ch3:nemesis:portals4

If the Portals 4 include files and libraries are not in the normal
search paths, you can specify them with the following options:

    --with-portals4-include= 
   and
    --with-portals4-lib=

... or the if lib/ and include/ are in the same directory, you can use
the following option:

    --with-portals4=

If the Portals libraries are shared libraries, they need to be in the
shared library search path. This can be done by adding the path to
/etc/ld.so.conf, or by setting the LD_LIBRARY_PATH variable in your
environment. It's also possible to set the shared library search path
in the binary. If you're using gcc, you can do this by adding

    LD_LIBRARY_PATH=/path/to/lib

  (and)

    LDFLAGS="-Wl,-rpath -Wl,/path/to/lib"

... as arguments to configure.

Currently, use of MPI_ANY_SOURCE and MPI dynamic processes are unsupported
with the portals4 netmod.


#### sock channel
sock is the traditional TCP sockets based communication channel. It
uses TCP/IP sockets for all communication including intra-node
communication. So, though the performance of this channel is worse
than that of nemesis, it should work on almost every platform. This
channel can be configured using the following option:

    --with-device=ch3:sock


### pamid device
This is the device used on the IBM Blue Gene/Q system.  The following
configure options can be used:

    ./configure --host=powerpc64-bgq-linux                              \
              --with-device=pamid:BGQ                                   \
              --with-file-system=bg+bglockless

The Blue Gene/Q cross compilers must either be in the $PATH, or
explicitly specified using environment variables, before configure.
For example:

    PATH=$PATH:/bgsys/drivers/ppcfloor/gnu-linux/bin

or

    CC=/bgsys/drivers/ppcfloor/gnu-linux/bin/powerpc64-bgq-linux-gcc
    CXX=...
    ...

There are several other configure options that are specific to building
on a Blue Gene/Q system.  See the wiki page for more information:

  https://wiki.mpich.org/mpich/index.php/BGQ

-------------------------------------------------------------------------

## 5. Alternate Process Managers

###  hydra
Hydra is the default process management framework that uses existing
daemons on nodes (e.g., ssh, pbs, slurm, sge) to start MPI
processes. More information on Hydra can be found at
http://wiki.mpich.org/mpich/index.php/Using_the_Hydra_Process_Manager

###  gforker
gforker is a process manager that creates processes on a single
machine, by having mpiexec directly fork and exec them. gforker is
mostly meant as a research platform and for debugging purposes, as it
is only meant for single-node systems.

###  slurm
SLURM is an external process manager not distributed with
MPICH. MPICH's default process manager, hydra, has native support
for slurm and you can directly use it in slurm environments (it will
automatically detect slurm and use slurm capabilities). However, if
you want to use the slurm provided "srun" process manager, you can use
the `--with-pmi=slurm --with-pm=no` option with configure. Note that
the "srun" process manager that comes with slurm uses an older PMI
standard which does not have some of the performance enhancements that
hydra provides in slurm environments.

-------------------------------------------------------------------------

## 6. Alternate Configure Options

MPICH has a number of other features. If you are exploring MPICH as
part of a development project, you might want to tweak the MPICH
build with the following configure options. A complete list of
configuration options can be found using:

    ./configure --help

-------------------------------------------------------------------------

## 7. Testing the MPICH installation

To test MPICH, we package the MPICH test suite in the MPICH
distribution. You can run the test suite using:

     make testing

The results summary will be placed in test/summary.xml

-------------------------------------------------------------------------

## 8. Fault Tolerance

MPICH has some tolerance to process failures, and supports
checkpointing and restart. 

### Tolerance to Process Failures

The features described in this section should be considered
experimental.  Which means that they have not been fully tested, and
the behavior may change in future releases. The below notes are some
guidelines on what can be expected in this feature:

 - ERROR RETURNS: Communication failures in MPICH are not fatal
   errors.  This means that if the user sets the error handler to
   MPI_ERRORS_RETURN, MPICH will return an appropriate error code in
   the event of a communication failure.  When a process detects a
   failure when communicating with another process, it will consider
   the other process as having failed and will no longer attempt to
   communicate with that process.  The user can, however, continue
   making communication calls to other processes.  Any outstanding
   send or receive operations to a failed process, or wildcard
   receives (i.e., with MPI_ANY_SOURCE) posted to communicators with a
   failed process, will be immediately completed with an appropriate
   error code.

 - COLLECTIVES: For collective operations performed on communicators
   with a failed process, the collective would return an error on
   some, but not necessarily all processes. A collective call
   returning MPI_SUCCESS on a given process means that the part of the
   collective performed by that process has been successful.

 - PROCESS MANAGER: If used with the hydra process manager, hydra will
   detect failed processes and notify the MPICH library.  Users can
   query the list of failed processes using MPIX_Comm_group_failed().
   This functions returns a group consisting of the failed processes
   in the communicator.  The function MPIX_Comm_remote_group_failed()
   is provided for querying failed processes in the remote processes
   of an intercommunicator.

   Note that hydra by default will abort the entire application when
   any process terminates before calling MPI_Finalize.  In order to
   allow an application to continue running despite failed processes,
   you will need to pass the -disable-auto-cleanup option to mpiexec.

 - FAILURE NOTIFICATION: THIS IS AN UNSUPPORTED FEATURE AND WILL
   ALMOST CERTAINLY CHANGE IN THE FUTURE!

   In the current release, hydra notifies the MPICH library of failed
   processes by sending a SIGUSR1 signal.  The application can catch
   this signal to be notified of failed processes.  If the application
   replaces the library's signal handler with its own, the application
   must be sure to call the library's handler from it's own
   handler.  Note that you cannot call any MPI function from inside a
   signal handler.

### Checkpoint and Restart

MPICH supports checkpointing and restart fault-tolerance using BLCR.

#### CONFIGURATION

First, you need to have BLCR version 0.8.2 or later installed on your
machine.  If it's installed in the default system location, you don't
need to do anything.

If BLCR is not installed in the default system location, you'll need
to tell MPICH's configure where to find it. You might also need to
set the LD_LIBRARY_PATH environment variable so that BLCR's shared
libraries can be found.  In this case add the following options to
your configure command:

    --with-blcr=<BLCR_INSTALL_DIR> 
    LD_LIBRARY_PATH=<BLCR_INSTALL_DIR>/lib

where <BLCR_INSTALL_DIR> is the directory where BLCR has been
installed (whatever was specified in --prefix when BLCR was
configured).

After it's configured compile as usual (e.g., make; make install).

Note, checkpointing is only supported with the Hydra process manager.


#### VERIFYING CHECKPOINTING SUPPORT

Make sure MPICH is correctly configured with BLCR. You can do this
using:

    mpiexec -info

This should display 'BLCR' under 'Checkpointing libraries available'.


#### CHECKPOINTING THE APPLICATION

There are two ways to cause the application to checkpoint. You can ask
mpiexec to periodically checkpoint the application using the mpiexec
option -ckpoint-interval (seconds):

    mpiexec -ckpointlib blcr -ckpoint-prefix /tmp/app.ckpoint \
      -ckpoint-interval 3600 -f hosts -n 4 ./app

Alternatively, you can also manually force checkpointing by sending a
SIGUSR1 signal to mpiexec.

The checkpoint/restart parameters can also be controlled with the
environment variables HYDRA_CKPOINTLIB, HYDRA_CKPOINT_PREFIX and
HYDRA_CKPOINT_INTERVAL.

To restart a process:

    mpiexec -ckpointlib blcr -ckpoint-prefix /tmp/app.ckpoint -f hosts -n 4 -ckpoint-num <N>

where `<N>` is the checkpoint number you want to restart from.

These instructions can also be found on the MPICH wiki:

  http://wiki.mpich.org/mpich/index.php/Checkpointing

-------------------------------------------------------------------------

## 9. Developer Builds

For MPICH developers who want to directly work on the svn, there are
a few additional steps involved (people using the release tarballs do
not have to follow these steps). Details about these steps can be
found here:
http://wiki.mpich.org/mpich/index.php/Getting_And_Building_MPICH

-------------------------------------------------------------------------

## 10. Multiple Fortran compiler support

If the C compiler that is used to build MPICH libraries supports both
multiple weak symbols and multiple aliases of common symbols, the
Fortran binding can support multiple Fortran compilers. The
multiple weak symbols support allow MPICH to provide different name
mangling scheme (of subroutine names) required by differen Fortran
compilers. The multiple aliases of common symbols support enables
MPICH to equal different common block symbols of the MPI Fortran
constant, e.g. MPI_IN_PLACE, MPI_STATUS_IGNORE. So they are understood
by different Fortran compilers.

Since the support of multiple aliases of common symbols is
new/experimental, users can disable the feature by using configure
option --disable-multi-aliases if it causes any undesirable effect,
e.g. linker warnings of different sizes of common symbols, MPIFCMB*
(the warning should be harmless).

We have only tested this support on a limited set of
platforms/compilers.  On linux, if the C compiler that builds MPICH is
either gcc or icc, the above support will be enabled by configure.  At
the time of this writing, pgcc does not seem to have this multiple
aliases of common symbols, so configure will detect the deficiency and
disable the feature automatically.  The tested Fortran compilers
include GNU Fortran compilers (gfortan), Intel Fortran compiler
(ifort), Portland Group Fortran compilers (pgfortran), Absoft Fortran
compilers (af90), and IBM XL fortran compiler (xlf).  What this means
is that if mpich is built by gcc/gfortran, the resulting mpich library
can be used to link a Fortran program compiled/linked by another
fortran compiler, say pgf90, say through mpifort -fc=pgf90.  As long
as the Fortran program is linked without any errors by one of these
compilers, the program shall be running fine.

-------------------------------------------------------------------------

## 11. ABI Compatibility

The MPICH ABI compatibility initiative was announced at SC 2014
(http://www.mpich.org/abi).  As a part of this initiative, Argonne,
Intel, IBM and Cray have committed to maintaining ABI compatibility
with each other.

As a first step in this initiative, starting with version 3.1, MPICH
is binary (ABI) compatible with Intel MPI 5.0.  This means you can
build your program with one MPI implementation and run with the other.
Specifically, binary-only applications that were built and distributed
with one of these MPI implementations can now be executed with the
other MPI implementation.

Some setup is required to achieve this.  Suppose you have MPICH
installed in /path/to/mpich and Intel MPI installed in /path/to/impi.

You can run your application with mpich using:

    % export LD_LIBRARY_PATH=/path/to/mpich/lib:$LD_LIBRARY_PATH
    % mpiexec -np 100 ./foo

or using Intel MPI using:

    % export LD_LIBRARY_PATH=/path/to/impi/lib:$LD_LIBRARY_PATH
    % mpiexec -np 100 ./foo

This works irrespective of which MPI implementation your application
was compiled with, as long as you use one of the MPI implementations
in the ABI compatibility initiative.
