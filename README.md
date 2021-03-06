# Continuous integration

* Linux build: [![Circle CI](https://circleci.com/gh/QIICR/dcmqi.svg?style=svg)](https://circleci.com/gh/QIICR/dcmqi)
* Windows build: [![Build status](https://ci.appveyor.com/api/projects/status/04l87y2j6prboap7?svg=true)](https://ci.appveyor.com/project/fedorov/dcmqi)
* Mac OS X build: [![TravisCI](https://travis-ci.org/QIICR/dcmqi.svg?branch=master)](https://travis-ci.org/QIICR/dcmqi)

# About

This is work in progress to develop libraries and command line tools with minimum dependencies
to support conversion of DICOM data specific to quantitative image analysis research.

The logic behind the name is DICOM (dcm) + Quantitative Imaging (qi). This follows the patterns used by DCMTK modules (dcmsr, dcmrt, ...). The idea is to put various components needed for QI applications that for one or another reason do not fit DCMTK.

This work is part of the QIICR project, http://qiicr.org.

# Prerequisites

You will need the development environment suitable for your platform. 
CMake (http://cmake.org) is required to configure the build. The code is
expected to compile on Windows, Mac and Linux (you can take a look at the icons
on top of this document to see if the build succeeds on these platforms).

The project is configured to download and build the following prerequisites:
* Insight toolkit (ITK) http://itk.org
* DICOM Toolkit (DCMTK) http://dcmtk.org
* Slicer Execution Model https://github.com/Slicer/SlicerExecutionModel

### Did you build Slicer on your own? You can speed up the build process!

If you built Slicer previously on your own, you can disable the SuperBuild option. Since your Slicer build already
downloaded and built all essential prerequisites, this CMake project will be able to reuse them.

1. Set DCMQI_SUPERBUILD in your CMAKE settings to OFF
2. Configure
3. Set SlicerExecutionModel_DIR manually, which is usually located under
{Slicer_Build_Directory}/SlicerExecutionModel-build
4. Configure and generate

This will speed up the build process enormously.

# Usage

This library is in the early stages of development. We are currently not
providing the binary releases, and you will need to build the source code in
order to use the tools.

Currently, the following tools are being built:

* itkimage2segimage - converts a (list of) segmentation label(s) saved in any
  format supported by ITK into DICOM Segmentation Image. Input parameters
  should include metafile specifying semantics of the segmented regions.
  Example usage (subject to changes, see CMakeLists.txt in apps/seg for the
  up-to-date invocation example):

```
BINARY_DIR/itkimage2segimage
  --seriesNumber 1 --seriesDescription "Liver Segmentation"
  --labelAttributesFiles doc/sample.info
  --segImageFiles data/ct-3slice/liver_seg.nrrd
  --dicomImageFiles data/ct-3slice/01.dcm,data/ct-3slice/02.dcm,data/ct-3slice/03.dcm
  --segDICOMFile liver.dcm
```

* segimage2itkimage - converts DICOM Segmentation Image instance into a format
  readable by ITK. The output specified to this tool is a directory, since a
  DICOM Segmentation Image can contain multiple segments, which may overlap.
  The output directory will also contain the files with the per-segment
  information, such as segment semantics, segmentation algorithm type, etc.
  Example usage:

```
BINARY_DIR/segimage2itkimage
  data/ct-3slice/liver.dcm .
```

For the examples how to use
the tools, run the following command after building the source code in the
dcmqi-build directory:

```
ctest -VV .
```

# What is planned

* separate functionality used by the converter tools into a reusable library
* improve mechanisms used for communicating metadata to the converter tools;
  one idea being explored is to use JSON instead of or as an alternative to the
  comma-separated .info files used currently. It is also conceivable to replace
  command line arguments with a JSON to communicate series level attributes
  separately from segment-level attributes.
* integrate this library within a 3D Slicer extension
* provide converter tools for generating DICOM Structured Reports following TID
  1500 (Measurement Report).

# Acknowledgments

This work is supported in part the National Institutes of Health, National
Cancer Institute, Informatics Technology for Cancer Research (ITCR) program,
grant Quantitative Image Informatics for Cancer Research (QIICR) (U24
CA180918, PIs Kikinis and Fedorov).

# Contact

Please send your feedback, comments or problem reports to [Andrey Fedorov](http://fedorov.github.io)

More information about QIICR: http://qiicr.org
