# Hyperelastic Tensile Test Post-Processing for Skin-Mimicking Materials

This repository contains Jupyter notebooks for post-processing uniaxial tensile tests of skin-mimicking elastomeric materials. The workflow combines tensile testing data with optical image-based dimensional measurements to obtain stress–strain curves suitable for the mechanical characterization of highly deformable materials.

The current notebooks process two material groups:

- `Resultados_SiliconeC.ipynb` — tensile post-processing for silicone-based samples.
- `Resultados_CompositoE.ipynb` — tensile post-processing for composite/elastomeric samples.

The analysis is intended to support the extraction of experimental mechanical data for nonlinear constitutive modelling, particularly when conventional linear elastic descriptors such as Young's modulus and Poisson's ratio are insufficient.

## Overview

Soft elastomeric and skin-mimicking materials typically undergo large deformations during tensile testing. For this reason, the notebooks compute and compare:

- engineering stress–strain curves, also referred to in the notebooks as first Piola–Kirchhoff stress;
- true stress–strain curves, also referred to as Cauchy stress;
- specimen width and length evolution from camera images;
- combined replicate curves for each material condition;
- exported `.csv` files for subsequent fitting of hyperelastic constitutive models.

The generated outputs are particularly useful for regression of nonlinear material models in software such as COMSOL Multiphysics, Abaqus, ANSYS, MATLAB, or Python-based optimization routines.

## Repository structure

A recommended structure for the repository is:

```text
.
├── README.md
├── Results_Functions.ipynb
├── Resultados_SiliconeC.ipynb
├── Resultados_CompositoE.ipynb
├── data/
│   ├── tensile/
│   │   ├── SiliconeC/
│   │   └── CompositoE/
│   └── camera/
│       ├── SiliconeC/
│       └── CompositoE/
└── results/
    ├── 1Piola-kirchhoffStress.csv
    └── CauchyStress.csv
```

`Results_Functions.ipynb` is required because the analysis notebooks call helper functions from it using:

```python
%run Results_Functions.ipynb
```

The raw experimental files are not included by default and should be organized locally according to the paths defined in each notebook.

## Workflow

The notebooks follow the same general procedure for each sample:

1. Load tensile testing data from the selected material folder.
2. Downsample or resample the mechanical data to match the available optical frames.
3. Load the corresponding camera images.
4. Process the images to estimate specimen dimensional evolution during stretching.
5. Define the initial specimen thickness.
6. Calculate engineering and true stress–strain curves.
7. Plot individual and combined results.
8. Export processed stress–strain datasets to `.csv`.

The main functions called in the notebooks are:

```python
read_and_downsample(...)
compare_plots(...)
resample_frames(...)
process_camera(...)
calculate_ss(...)
```

These functions should be defined in `Results_Functions.ipynb`.

## Experimental assumptions

The workflow assumes that the user has access to:

- tensile testing data acquired from a universal testing machine;
- synchronized or approximately synchronized camera frames;
- known pixel-to-millimetre calibration;
- known initial sample thickness;
- approximately uniform uniaxial deformation within the analysed region;
- manually defined image-processing thresholds for each sample when required.

In the analysed notebooks, the nominal testing velocity is defined as:

```python
velocity = 10  # mm/min
```

The pixel-to-millimetre calibration used in the uploaded notebooks is:

```python
ppm = 3.4106412
```

Different frame rates, image thresholds, and initial thicknesses are defined per sample.

## Outputs

The notebooks export processed stress–strain curves as `.csv` files:

```text
1Piola-kirchhoffStress.csv
CauchyStress.csv
```

These files contain strain and stress values in MPa and can be used for further material model fitting.

The notebooks also generate plots comparing:

- original and downsampled force–displacement data;
- engineering stress and true stress;
- stress–strain curves from multiple sample replicates.

## Installation

A Python environment with Jupyter support is required. The notebooks were prepared in a Conda-based Python environment.

Recommended installation:

```bash
conda create -n tensile-postprocessing python=3.11
conda activate tensile-postprocessing
pip install numpy pandas matplotlib seaborn scipy opencv-python pillow jupyter
```

Depending on the functions implemented in `Results_Functions.ipynb`, additional packages may be required.

## Usage

Clone the repository:

```bash
git clone https://github.com/your-username/your-repository-name.git
cd your-repository-name
```

Start Jupyter:

```bash
jupyter notebook
```

Open the desired notebook:

```text
Resultados_SiliconeC.ipynb
Resultados_CompositoE.ipynb
```

Before running the notebooks, update the local paths to match your data directory. For example:

```python
tensile_path = "path/to/data/tensile/SiliconeC/"
camera_path = "path/to/data/camera/SiliconeC/sample_22/Process/"
```

Then run the cells sequentially.

## Notes on reproducibility

The uploaded notebooks currently contain absolute Windows paths, such as:

```python
C://Users//nnuno//Desktop//PhD//Fantomas//Skin//Ensaios//SiliconeC//
```

For public release, it is recommended to replace these with relative paths using `pathlib`, for example:

```python
from pathlib import Path

base_dir = Path("data")
tensile_path = base_dir / "tensile" / "SiliconeC"
camera_path = base_dir / "camera" / "SiliconeC" / "22" / "Process"
```

It is also recommended to keep exported results in a dedicated `results/` folder rather than writing them to the repository root.

## Important repository-cleaning recommendations

Before making the repository public, consider the following improvements:

- remove absolute local paths;
- remove printed local file paths from notebook outputs;
- clear unnecessary notebook outputs to reduce file size;
- include `Results_Functions.ipynb`, since the current notebooks depend on it;
- add a small example dataset or synthetic sample data;
- standardize notebook names and sample labels;
- verify that `Resultados_CompositoE.ipynb` points to the correct composite data paths, since the uploaded version appears to still reference `SiliconeC` folders in several cells;
- add a license file;
- add a citation section if the repository supports a thesis, article, or conference paper.

## Suggested citation

If this code supports a publication, thesis, dataset, or Zenodo archive, cite the archived version of the repository together with the associated paper.

Example:

```bibtex
@software{fernandes_tensile_postprocessing,
  author  = {Fernandes, Nuno A. T. C.},
  title   = {Hyperelastic Tensile Test Post-Processing for Skin-Mimicking Materials},
  year    = {2026},
  url     = {https://github.com/your-username/your-repository-name}
}
```

If a DOI is created through Zenodo, replace the URL with the DOI.

## License

No license has been specified yet.

For academic open-source release, consider using one of the following:

- MIT License, for permissive reuse;
- BSD 3-Clause License, for permissive reuse with clearer attribution conditions;
- GPL-3.0 License, if derivative code should remain open-source.

## Author

Nuno A. T. C. Fernandes  
University of Minho

## Acknowledgements

This repository was developed as part of research activities related to the mechanical characterization of skin-mimicking and elastomeric materials for biomedical and ultrasound-related applications.
