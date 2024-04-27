---
title: Idiomatic Ephemeris Sourcing and Parsing in Julia

tags:
  - astrodynamics
  - astronomy
  - astrophysics
  - ephemeris
  - Julia
  - SPICE
  - Horizons

authors:
  - name: Joseph D Carpinelli
    orcid: https://orcid.org/0000-0001-8655-8125
    email: joey@loopy.codes

date: 21 April 2024
bibliography: references.bib
---

## Summary

Students and professionals in astronomy, astrodynamics, astrophysics, and other
related fields often download and parse solar system ephemeris data from two
major providers:
[Generic JPL SPICE Kernels](https://naif.jpl.nasa.gov/pub/naif/generic_kernels/),
and [JPL Horizons](https://ssd.jpl.nasa.gov/horizons/). SPICE Kernels are
typically read through the SPICE Toolkit, which is available in a variety of
programming languages, include the C Programming Language with `CSPICE` [@cspice].
The Julia packages [`CSPICE_jll.jl`](https://github.com/JuliaBinaryWrappers/CSPICE_jll.jl)
and [`SPICE.jl`](https://github.com/JuliaAstro/SPICE.jl) expose many `CSPICE`
functions through Julia functions. Julia users can load and interact with SPICE
kernels through methods such as `SPICE.furnsh` and `SPICE.spkez`.
Horizons data is available through
a variety of methods, including email, command-line, graphical web interfaces,
and a [REST API](https://ssd-api.jpl.nasa.gov/doc/horizons.html) [@horizons].

This paper introduces several packages which allow users to download and process
ephemeris data idomatically, all from within Julia: `SPICEApplications.jl`,
`SPICEKernels.jl`, `SPICEBodies.jl`, `HorizonsAPI.jl` and `HorizonsEphemeris.jl`.
Through the use of these packages, users can share replicatable code which
automatically fetches publicly available ephemeris data, as opposed to manually
including ephemeris data files with their source code distribution.

## Statement of Need

While ephemeris users have all of the tools they need to fetch and parse ephemeris
data within Julia, they do not have the tools to do so _simply_ or
_idiomatically_. [@sec-need-horizons] and [@sec-need-spice] present the research
needs filled by each of the five packages introduced in this paper.

### JPL HORIZONS

The two HORIZONS-related packages presented in this paper ---
[`HorizonsAPI.jl`](https://github.com/cadojo/HorizonsAPI.jl)
and [`HorizonsEphemeris.jl`](https://github.com/cadojo/HorizonsEphemeris.jl) ---
are respectively the first Julia packages to precisely match the REST API with
tab-completion through static keyword arguments, and the first to offer
automatic response parsing into `NamedTuple` types. The `NamedTuple` output of
`HorizonsEphemeris.ephemeris`, the top-level method for fetching Cartesian state
vectors from the HORIZONS platform, allows for easy plotting, file-saving, and
`DataFrame` construction. Both `HorizonsAPI.jl` and `HorizonsEphemeris.jl` offer
users a simple, repeatable way to query and parse HORIZONS ephemeris data.

### JPL SPICE

The three SPICE-related packages presented in this paper ---
[`SPICEApplications.jl`](https://github.com/cadojo/SPICEApplications.jl),
[`SPICEKernels.jl`](https://github.com/cadojo/SPICEKernels.jl), and
[`SPICEBodies.jl`](https://github.com/cadojo/SPICEBodies.jl) --- provide
idiomatic kernel fetching, inspection, and caching from within Julia.
While SPICE Toolkit executables were _bundled_ in Julia through
[`CSPICE_jll`](https://github.com/JuliaBinaryWrappers/CSPICE_jll.jl), they have
not been previously _exposed_ through Julia functions. `SPICEApplications.jl`
wraps each executable with a Julia function, allowing users to easily call
SPICE Toolkit executables within their Julia programs, just as they can with
`CSPICE` routines wrapped in [`SPICE.jl`](https://github.com/JuliaAstro/SPICE.jl).

Julia users interact with SPICE kernels by downloading publicly-available
a[Generic Kernels](https://naif.jpl.nasa.gov/pub/naif/generic_kernels/), and
parsing the data using `SPICE.jl`, or another ephemeris parsing source. This
workflow requires that users know how to find the correct generic kernels for
their chosen application, and that they know how to use CSPICE functions to
retrieve their desired data. `SPICEKernels.jl` and `SPICEBodies.jl` offer
idiomatic interfaces to ephemeris fetching and parsing parsing respectively.
Continuous integration in the
[`SPICEKernels.jl` repository](https://github.com/cadojo/SPICEKernels.jl)
multiple times daily, and automatically exports all available generic kernels
as variables in Julia. SPICE Toolkit executables (provided by
`SPICEApplications.jl`) are used to retrieve a description of each kernel's
contents, and place that description in the Julia variable's docstring. As a
result, users can use tab-completion and Julia's built-in documentation tools
to inspect kernel contents, and download the correct kernel for their
application. Once each kernel is downloaded and loaded into the SPICE kernel pool
with `SPICE.jl`, users can use `SPICEBodies.jl` to idiomatically fetch data at
a provided instance in time.

## Usage

For detailed usage examples, consult the common
[documentation site](https://ephemeris.loopy.codes) for all of the packages
presented in this paper.

## External Packages

The packages presented in this paper which interact with the SPICE Toolkit
require users to use [`SPICE.jl`](https://github.com/JuliaAstro/SPICe.jl),
or another SPICE-compatible kernel loading tool. Support for other SPICE
kernel management packages, such as
[`Ephemerides.jl`](https://github.com/JuliaSpaceMissionDesign/Ephemerides.jl),
may be added in the future.

In addition to the packages in this paper which interface with the JPL HORIZONS
ephemeris platform, the [`HORIZONS.JL`](https://github.com/PerezHz/HORIZONS.jl)
package offers simplified interfaces for constructing and sending queries to the
JPL HORIZONS REST API.

## References