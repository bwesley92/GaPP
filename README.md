# GaPP

This Gaussian Processes GaPP code auxiliary repository, replacing the original link [http://www.acgc.uct.ac.za/~seikel/GAPP/index.html] which seems to be broken.  

GaPP was written by Seikel, Clarkson, Smith 2012 (https://arxiv.org/abs/1204.2832, JCAP06(2012)036). The algorithm uses Gaussian processes to reconstruct a function from a sample of data without assuming a parameterization of the function. The GaPP code can be used on any dataset to reconstruct a function. It handles individual error bars on the data and can be used to determine the derivatives of the reconstructed function. The data sample can consist of observations of the function and of its first derivative.

## Installation

1. Download and unzip the GaPP repository
2. Navigate to the GaPP directory
3. Install the package using pip:

```bash
pip install -e .
```

This will install GaPP along with its required dependencies (numpy, scipy, matplotlib).

## How to Run the Algorithm

### Option 1: Using Python Scripts

The repository includes several example scripts that demonstrate how to use GaPP:

#### H0 Measurement Example
To run the H0 measurement example using cosmic chronometers and BAO data:

```bash
python H0_measurement.py
```

This will:
- Load data from `hz_cc_sdss.dat`
- Reconstruct H(z) using Gaussian Process with Squared Exponential covariance function
- Print the reconstructed H0 value at z=0 with uncertainties
- Generate and save a plot (`hz_cc_sdss_reconst.png`) showing the reconstruction with 1σ, 2σ, and 3σ confidence regions
- Save the reconstructed data to `hz_rec.dat`

#### Simple Gaussian Process Example
To run a basic GP reconstruction example:

```bash
cd examples/gp
python gp_example.py
```

This demonstrates:
- Loading input data from a 2D dataset
- Initializing a GaussianProcess object
- Training hyperparameters
- Reconstructing the function on a grid
- Plotting the results (if matplotlib is available)

### Option 2: Using Jupyter Notebook

An interactive Jupyter notebook is provided for experimentation:

```bash
jupyter notebook GP.ipynb
```

The notebook includes:
- Step-by-step implementation of H(z) reconstruction
- Data loading and preprocessing
- Gaussian Process configuration with various covariance functions
- Visualization of results
- Hyperparameter optimization

### Option 3: Using GaPP in Your Own Code

Here's a minimal example to use GaPP in your Python code:

```python
from gapp import gp
from gapp import covariance
from numpy import loadtxt, savetxt

# Load your data (x, y, errors)
X, Y, Sigma = loadtxt("your_data.txt", usecols=(0,1,2), unpack=True)

# Define reconstruction range
xmin = 0.0
xmax = 10.0
npoints = 200

# Initialize Gaussian Process with a covariance function
g = gp.GaussianProcess(X, Y, Sigma, 
                       covfunction=covariance.SquaredExponential,
                       cXstar=(xmin, xmax, npoints))

# Perform reconstruction with hyperparameter training
(reconstruction, theta) = g.gp(thetatrain='True')

# Save results
savetxt("output.txt", reconstruction)
```

## Available Covariance Functions

GaPP supports several covariance functions:
- `covariance.SquaredExponential`
- `covariance.DoubleSquaredExponential`
- `covariance.Matern32`
- `covariance.Matern52`
- `covariance.Matern72`
- `covariance.Matern92`

## Input Data Format

Your input data should be a text file with three columns:
1. X values (independent variable)
2. Y values (function values)
3. Sigma values (error bars/uncertainties)

Example:
```
0.07  69.0  19.6
0.12  68.6  26.2
0.20  72.9  29.6
```

## Output

The algorithm produces:
- Reconstructed function values at specified points
- Uncertainty estimates (standard deviation) at each point
- Optional derivatives (dH/dz, d²H/dz²) if required

Output format: `[x_values, reconstructed_y, sigma_y]`

## Additional Examples

More examples can be found in the `examples/` directory:
- `examples/gp/` - Basic Gaussian Process examples
- `examples/dgp/` - Derivative Gaussian Process examples
- `examples/mcmcdgp/` - MCMC-based Derivative Gaussian Process examples

## Documentation

For detailed documentation, please refer to `Documentation.pdf` in the repository.

## Contact

Questions about GaPP can be addressed to carlosap87@gmail.com.
