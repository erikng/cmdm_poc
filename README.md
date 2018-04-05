# C-MDM POC
This POC code is to showcase the possibilities of MDM as desired state.

## Warning
The code provided here is _not_ intended for production use. Deploying this to your machines could result in the leaking of data. These API's are extremely powerful and could potentially result in the loss of MDM management on _all_ of your devices.

### A subsequent warning about MicroMDM
Currently, MicroMDM's API should be thought of as an "internal/server API". This means that the api was designed for interacting with the server configurations.

While it can be used/abused for this POC, deploying this to your devices means that devices could potentially change _any_ MDM configuration on your server.

## Configuration
The POC comes with a few global variables to set. These variables are documented in the binary.

### Mandatory variables
- authorization
- baseurl
- airwatch
- micromdm

You _cannot_ specify both `airwatch` and `micromdm`

### Mandatory AirWatch variables
- airwatchtenantcode

### Optional variables
- cachecomputerid = True
- validateprofiles

### Optional MicroMDM variables
- localtest

## Installation
1. ensure the binary is marked as an executable `chmod a+x ./poctool/profiles`
2. copy it to /usr/local/bin `sudo cp -r ./poctool/profiles /usr/local/bin`

## Use with chef
After installation, any subsequent chef-runs will immediately use the binary. If you would like to test a profile, simply delete a profile already installed via chef.

## Manual use
Since this was designed to abuse chef, specific arguments must be passed. There are certainly edge cases here.

### Install a local profile
`/usr/local/bin/profiles -I -F /path/to/your.mobileconfig`

### Remove a profile installed via MDM/this tool.
`/usr/local/bin/profiles -Rp profileidentifier`

### View contents of all installed profiles
`/usr/local/bin/profiles -Po stdout-xml`

## Limitations
- If you attempt to remove a profile that is installed locally, it will fail. Please use `/usr/bin/profiles -Rp identifier` instead. A future version of this poc may handle this natively.
- If you have scripts (or manually) invoke `profiles` using it's relative path, this tool will take over it's use. More than likely you will receive an error: `profiles: error: no such option: %option`

### AirWatch Limitations
- Currently this feature is only available in AirWatch console version 9.1 -> 9.3.
- As of AirWatch 9.3, attempting to install a profile payload greater than 2,000 characters will result in a failure. If your profile is too large for installation, it will be installed locally on the device. This limitation will be removed entirely in a future version of AirWatch.

### MicroMDM Limitations
- As of MicroMDM 1.2, there is a bug in the API for profile removals. This will be fixed in a future version of MicroMDM.
- As of MicroMDM 1.2, attempting to install a profile payload greater than 10,000 characters will result in a failure. If your profile is too large for installation, it will be installed locally on the device. This will be raised to 100,000 characters in a future version of MicroMDM.
