libkcapi -- Linux Kernel Crypto API User Space Interface Library
================================================================

The Linux kernel exports a Netlink interface of type AF_ALG to allow user
space to utilize the kernel crypto API.

libkcapi uses this Netlink interface and exports easy to use APIs so that
a developer does not need to consider the low-level Netlink interface handling.

The library does not implement any cipher algorithms. All consumer requests
are sent to the kernel for processing. Results from the kernel crypto API
are returned to the consumer via the library API.

The kernel interface and therefore this library can be used by unprivileged
processes.

API Documentation
=================

See the source code comments in kcapi-kernel-if.c.

Compilation
===========

The Makefile compiles libkcapi as a shared library.

The "install" Makefile target installs libkcapi under /usr/local/lib or
/usr/local/lib64. The header file is installed to /usr/local/include.

A consumer has to use the kcapi.h header file to link with libkcapi.

In case a consumer does not want a shared library, the libkcapi C file and
header file can also just copied to the consumer code and compiled along
with it.

Use the "scan" Makefile target to use the CLANG static code analyzer to
verify the quality of the source code. 

Memory Allocation
=================

The library libkcapi uses the data structure struct kcapi_handle as the
cipher handle that allows the consumer to operate with the various function
calls of this library.

Unlike other crypto libraries, libkcapi does not allocate any memory or
performs operations that implies memory allocation. struct kcapi_handle only
holds pointers to the consumer-provided buffers with sensitive data. That means
that the buffers holding sensitive data like keys are under full control of the
consumer. Therefore, this library does not offer any memory allocation or
secure memory clearing functions.

The consumer must ensure that the memory is appropriately sanitized. The caller
does not need to sanitize struct kcapi_handle as it does not contain any
sensitive data.

Unimplemented Interfaces
========================

Depending on the version of your kernel, some of the kernel interfaces
the library depends on are not available. When using the respective library
API functions, an error is returned during initialization of the cipher
handle. The following interfaces are available:

	kcapi_md_*	since kernel version 3.0
	kcapi_cipher_*	since kernel version 3.0
	kcapi_aead_*	kernel interface under review
	kcapi_rng_*	kernel interface under review

Test cases
==========

The test/ directory contains test cases to verify the correct operation of
this library. In addition it allows the verification of the correct operation
of the kernel crypto API.

The test cases are documented in test/README.

Kernel Patches
==============

The following kernel patches are distributed with this library. There is no
need to add them to the kernel unless you want to use the following
functionality that is already supported by this library:

	* AEAD cipher

	* Random number generator

Use the latest patch set in the kernel-patch/ directory.

Author
======
Stephan Mueller <smueller@chronox.de>