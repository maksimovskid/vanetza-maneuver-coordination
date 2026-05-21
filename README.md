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

This fork supports the related Artery maneuver coordination implementation. It is currently under active development and should be considered work in progress.

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
You can find more details on [prerequisites](https://www.vanetza.org/how-to-build/#prerequisites) and [steps for compilation](https://www.vanetza.org/how-to-build/#compilation) on our website.

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
