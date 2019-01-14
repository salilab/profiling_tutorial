Profiling IMP applications {#mainpage}
==========================

[TOC]

# Introduction {#introduction}

In this tutorial we will look at CPU profiling of %IMP code using
Google's [gperftools](https://github.com/gperftools/gperftools) package.
This will show us where time is being spent during a modeling run, so that
we know where to focus our efforts in optimizing or otherwise improving the
code.

In this text we follow
[Google's own documentation](https://htmlpreview.github.io/?https://github.com/gperftools/gperftools/blob/master/docs/cpuprofile.html).

# Prerequisities {#prereq}

First, we need to install
[gperftools](https://github.com/gperftools/gperftools). It can be compiled
from source code, but it is generally easier to install a prebuilt package
for it. We recommend running it on a Linux box, although it should also work
on a Mac. To install on a Linux box, use something like

    sudo dnf install pprof gperftools-libs

On a Mac with [Homebrew](https://brew.sh), use

    brew install gperftools

# Compile IMP from source {#compile}

Next, we need to build %IMP from source code. See the
[installation instructions in the manual](@ref installation_source) for
more details. In order to use gperftools the %IMP libraries should be linked
against the profiler library (`-lprofiler`). This should be set up automatically
when you run `cmake`. If not, you can manually add this linkage by adding
something like `-DCMAKE_LD_FLAGS="-lprofiler"` to your `cmake` invocation.

# Run application and collect profiling info {#run}

There are several ways to collect profiling information; see the
[gperftools documentation](https://htmlpreview.github.io/?https://github.com/gperftools/gperftools/blob/master/docs/cpuprofile.html)
for more details. In this case, we simply collect profiling information for
the entire process by setting the `CPUPROFILE` environment variable to the
name of a profile output file. For this demonstration, we'll profile the
modeling script from the [RNAPII PMI tutorial](https://integrativemodeling.org/tutorials/rnapolii_stalk/) by running something like the following from a
terminal window:

    git clone https://github.com/salilab/imp_tutorial.git
    cd imp_tutorial/rnapolii/modeling
    env CPUPROFILE=out.prof ~/imp/release/setup_environment.sh python modeling.py --test

(This assumes that our %IMP source code checkout is in `~/imp/release/`; alter
the path appropriately if it is in a different directory.)

Once the script completes, a file `out.prof` should be produced.

# Analyze the profile {#analyze}

Analysis of the profile can be done using the `pprof` tool.
