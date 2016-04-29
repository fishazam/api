# FISHazam API

This code takes spectrogram data from professional spectrometers and sends it to the FISHazam API.  The spectrogram data can subsequently be processed to build a fish fingerprint.

Currently, only the spectrometers from OceanOptics work.  This is because the code is built on the python device drivers provided by OceanOptics via the [SeaBreeze python library](http://oceanoptics.com/product/seabreeze/).

In the future, we hope to integrate spectrometer hardware from various manufacturers.

The code was developed as part of [Fishackathon](http://www.fishackathon.co/) 2016 in London.  Our devpost describing the experience can be [found here](http://devpost.com/software/fishazam-ensrpw/).  Unfortunately, we did not have access to professional spectrometers over the weekend.  So some changes might be necessary when implementing.  But the fundamental code will not change.  For a video screencast discussing how this code works, check out our [YouTube channel](https://www.youtube.com/channel/UC4iefN94hdmy2HIM8PxcBPA).  

## Requirements

- [NumPy](http://www.numpy.org)
- [SeaBreeze - Embedded Open-Source Device Driver](http://oceanoptics.com/products/seabreeze/)
- [pyUSB==1.0.0b2](https://walac.github.io/pyusb/)
- [python requests](http://docs.python-requests.org/en/master/)
- [python json](https://docs.python.org/2/library/json.html)


## Installation

* [Linux](docs/LINUX_INSTALL.md)
* [MacOSX](docs/MACOSX_INSTALL.md)
* [Windows](docs/WINDOWS_INSTALL.md)

## Usage

The following example shows how simple it is to acquire a spectrum with
python-seabreeze through the model independent _Spectrometer_ class. For a more
detailed description read the [documentation](docs/DOCUMENTATION.md).:

```{python}
>>> import seabreeze.spectrometers as sb
>>> import requests
>>> import json

>>> devices = sb.list_devices()
>>> print devices
[<SeaBreezeDevice USB2000PLUS:USB2+H02749>, <SeaBreezeDevice USB2000PLUS:USB2+H02751>]
>>> spec = sb.Spectrometer(devices[0])
>>> spec.integration_time_micros(12000)
>>> spec.wavelengths()
array([  340.32581   ,   340.70321186,   341.08058305, ...,  1024.84940994,
        1025.1300678 ,  1025.4106617 ])
>>> spec.intensities()
array([  1.58187931e+01,   2.66704852e+04,   6.80208103e+02, ...,
         6.53090172e+02,   6.35011552e+02,   6.71168793e+02])
>>> specdata = {'fish':'red snapper','wavelengths':spec.wavelengths(),'intensities':spec.intensities()}
>>> specjson = json.dumps(specdata)
>>> url = 'http://api.fishazam.com'
>>> r =requests.post(url,data=specjson)

```


## Supported Devices

| Spectrometer | pyseabreeze |
|:-------------|:-----------:|
| HR2000       |      x      |
| HR2000PLUS   |      x      |
| HR4000       |      x      |
| JAZ          |      x      |
| MAYA2000     |      x      |
| MAYA2000PRO  |      x      |
| MAYALSL      |      x      |
| NIRQUEST256  |      x      |
| NIRQUEST512  |      x      |
| QE65000      |      x      |
| QE-PRO       |      x      |
| STS          |      x      |
| TORUS        |      x      |
| USB2000      |      x      |
| USB2000PLUS  |      x      |
| USB4000      |      x      |
| USB650       |      x      |


## License

Files in this repository are released under the [MIT license](LICENSE.md).
