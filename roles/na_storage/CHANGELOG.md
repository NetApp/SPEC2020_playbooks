
# Change Log
 
## [1.1.0] - 2017-03-15

### Added

  - MAJOR The ability to set up aggregates through auto provisioning
  - MINOR The ability to get FC port information from the cluster instead of pulling from variables passed in.
  - MINOR Added the ability to rename luns and volumes to allow for more specific naming within a vserver. This is done by setting vol_name or lun_name with the other vars passed to the role.

### Changed

  - MAJOR Moved from lun_count_per_vserver to lun_count_per_volume which allows for more complex storage setups

### Fixed

  - MINOR Added qos policy to default to none, which had previously gone unspecified.

## [1.0.0] - 2021

### Added

  - MAJOR The ability to setup storage based on variables passed in.

### Changed

### Fixed


# Format

The format is based on [Keep a Changelog](http://keepachangelog.com/) and this project adheres to [Semantic Versioning](http://semver.org/).