DEVELOPER TOOLS README
======================

Introduction
------------

This directory contains files primarily for test requiring deployment
orchestration:

* bringing deployments up with various configurations

* reconfiguring deployments during testing to test failover
  and other dynamic behaviors

Contents
--------

Subdirectories include:

* bin — harnesses and other helper programs
* config — general-purpose deployment definition files in YAML format
* lib - Perl modules for orchestration
* t-dynamic — Perl test files that spin up own deployment for testing
* t-special — Perl test files that should only be run manually
* stale -- legacy test files not yet adapted for new orchestration

The `t-dynamic` directory may also contain deployment definition YAML files
if these are so specialized that the regular test suite would fail.

The `t-special` directory has files with special prerequisites that must
be satisfied before tests will pass (e.g. compiling with SASL support).

The files in `config` should be limited to those for which the
test suite can succeed.

Configuration
-------------

The orchestration tool will search MONGOPATH and PATH (in that order) for
mongod or mongos of specific versions to fulfill a deployment definition
file specification.

For example, if there is a `~/mongodb` directory containing various mongodb
versions (e.g. from downloaded tarballs), each with a `bin` directory, this
command would set MONGOPATH to include all of them:

    export MONGOPATH=$(find ~/mongodb -type d -name bin | sort -r \
        | perl -wE 'say join(":",map {chomp; $_} <>)')

Sample usage
------------

Running a t-dynamic test:

    make test TEST_FILES=devel/t-dynamic/CAP-386-bulk-mixed-auth.t

Running all regular test files under a specific configuration:

    ./devel/bin/harness.pl devel/config/sharded-2.6.yml -- \
        make test 

Running a test file for major versions and topology types:

    ./devel/bin/test-all.pl make test TEST_FILES=t/bulk.t
