# Vanetza

Vanetza is an open-source implementation of the ETSI C-ITS protocol suite.
This comprises the following protocols and features among others:

* GeoNetworking (GN)
* Basic Transport Protocol (BTP)
* Decentralized Congestion Control (DCC)
* Security
* Support for ASN.1 messages (Facilities) such as CAM and DENM

Though originally designed to operate on ITS-G5 channels in a Vehicular Ad Hoc Network (VANET) using IEEE 802.11p, Vanetza and its components can be combined with other communication technologies as well, e.g. GeoNetworking over IP multicast or C-V2X.

## Maneuver Coordination Message Extension

This fork includes extensions related to the Maneuver Coordination Message (MCM) for V2X-based maneuver sharing and coordination. The implementation is part of the maneuver coordination work by Daniel Maksimovski and is based on the MCM concept described in the ITSC 2023 paper "Decentralized V2X Maneuver Sharing and Coordination Message for Cooperative Driving: Analysis in Mixed Traffic".

This fork provides the Vanetza extensions required by the
[Artery maneuver-coordination research implementation](https://github.com/maksimovskid/artery-maneuver-coordination),
including MCM ASN.1 support and related codec fixes. It is complete for that
project's currently validated research scope: coordinated merging and
safety-critical lane change. This does not claim production readiness or full
standard interoperability. Stock upstream Vanetza cannot replace this dependency.

The validated Artery revision `2fd300edc18aacbec08d8a992dcb40dccfea7bae` pins
Vanetza commit `d554d1188d59ca19c5f6bcae6d6bd2092308ca9d`. Reproduce that
pair through the parent's recursive submodule checkout; follow its
[installation guide](https://github.com/maksimovskid/artery-maneuver-coordination/blob/2fd300edc18aacbec08d8a992dcb40dccfea7bae/docs/install.md).
The normal build uses tracked codec sources and does not require ASN.1 regeneration.

### Codec scope and limitations

Commit `d554d118` adds missing OPTIONAL member maps in
`IntentSharingContainer.c` (indices 7–15), `ManeuverNegotiationContainer.c`
(6–11 and 14), `ManeuverExecutionContainer.c` (4), and `McmParameters.c` (2, 3).
These preserve optional lane data, negotiation content, and one-/two-CV execution
container decoding with the checked-in codec. The commit changes only those four
maps/counts: eight insertions and four deletions.

The checked-in codec is the compatibility target for the validated parent, not a
claim of exact correspondence with independently regenerated ASN.1 codecs:

- Both `PriMa-MCM.asn` copies place the extension marker after the optional
  execution container. The adapted `McmParameters.c` descriptor instead sets
  `first_extension=3`, with one optional root member and one optional extension
  member, so execution is encoded as an extension addition. The map fix preserves
  that existing layout; it does not resolve this schema/descriptor mismatch.
- The adapted `CooperationID` descriptor lacks a constraint callback and explicit
  PER bounds for the schema's 0..255 range. Artery bypasses full-object ASN.1
  validation for execution containers and supplies guarded application identities.
- Artery uses global SUMO coordinates and absolute simulation-centisecond point
  times in delta-named trajectory fields. Its existing `generationDeltaTime` is
  the message-age basis; no extra trajectory-reference timestamp fields are used.

Regenerating or replacing the descriptors requires separate wire-compatibility
review. The nonstandard MCM BTP port is 2027. Unused schema categories, larger
participant counts, arbitrary optional combinations, and independent-peer
interoperability are outside the validated scope.

From a built parent checkout, run the actual UPER byte tests and scenario checks:

```sh
cmake --build build --target mcm_timing_tests -j2
ctest --test-dir build -R '^mcm_timing$' --output-on-failure
build/mcm_timing_tests --runtime-roundtrip
python3 -m unittest discover -s tests/mcm -p 'test_*.py' -v
```

The dedicated roundtrip path covers runtime-shaped intention, Request, Offer,
Confirm, Accept and one-/two-CV Execute messages, including trajectories, lane
information, identities and generation time. These are same-codec roundtrips,
not independent ASN.1 conformance tests.

```bibtex
@INPROCEEDINGS{Maksimovski-ITSC2023,
  author={Maksimovski, Daniel and Facchi, Christian},
  booktitle={2023 IEEE 26th International Conference on Intelligent Transportation Systems (ITSC)},
  title={Decentralized {V2X} Maneuver Sharing and Coordination Message for Cooperative Driving: Analysis in Mixed Traffic},
  pages={5182-5189},
  year={2023},
  doi={10.1109/ITSC57777.2023.10421976},
}
```

## How to build

Building Vanetza is accomplished by the CMake build system. Hence, CMake needs to be available on the build host.
See this checkout's [prerequisites and compilation instructions](docs/how-to-build.md).
For the validated Artery environment, use the parent installation guide linked above.

## Documentation

Please visit our project website at [www.vanetza.org](https://www.vanetza.org) where most documentation about Vanetza can be found.


## Continuous Integration

We strive for quality in our code base.
New commits and pull requests are regularly checked by our unit tests in a container environment.
At the moment, the three latest Ubuntu LTS versions are run on GitHub's Actions infrastructure.

[![Build Status](https://github.com/riebl/vanetza/actions/workflows/docker-ci.yml/badge.svg?branch=master)](https://github.com/riebl/vanetza/actions/workflows/docker-ci.yml)


## Authors

Development of Vanetza started as research project at [Technische Hochschule Ingolstadt](https://www.thi.de/forschung/carissma/labore/car2x-labor/).
Since 2024, [nfiniity GmbH](https://www.nfiniity.com) has been sponsoring the active development.
Maintenance is coordinated by Raphael Riebl. Contributions are happily accepted.

## License

Vanetza is licensed under LGPLv3, see [license file](LICENSE.md) for details.
