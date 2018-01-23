# PeakFit
A set of peak fitting tools based on MATLAB for spectroscopic data analysis.

## Tested On
- MATLAB R2015 - R2017

## Requirements
- MATLAB Curve Fitting Toolbox
- [PhysConst](https://github.com/heriantolim/PhysConst) - required when invoking the `convertunit` method

## Licensing
This software is licensed under the GNU General Public License (version 3). 

## Usage
Construct the PeakFit object in the following ways and the fit results will be populated in the [public properties](https://github.com/heriantolim/PeakFit#public-properties) of the object.

```MATLAB
obj=PeakFit(Data, ... Name-Value ...)
```

OR

```MATLAB
obj=PeakFit(XData, YData, ... Name-Value ...)
```

OR

```MATLAB
% Create an empty PeakFit object.
obj=PeakFit();

% Specify the data points and peak-fit settings via property assignments. 
obj.XData= ... ;
obj.YData= ... ;
obj.Property1Name=Value1;
obj.Property2Name=Value2;
...

% Perform the peak fitting by reinstantiating the PeakFit object.
obj=PeakFit(obj);
```

`Data` must be specified as a two-column (or two-row) matrix where the first column (or first row) is the X data points and the second column (or second row) is the Y data points. In the alternative syntax, `XData` and `YData` are respectively the X and the Y data points, specified as vectors, of the curve to be fitted.

The peak-fit settings can be specified after the mandatory arguments in Name-Value syntax, e.g. `PeakFit(Data, Window, [100,900], NumPeaks, 3)`. If no settings are specified, the algorithm will attempt to fit all peaks in the data using the default settings. The default behavior may not be optimal for noisy data. See [Best Practices](https://github.com/heriantolim/PeakFit#best-practices) for recommendations in making an optimal fit.

## Name-Value Pair Arguments
Any of the [public properties](https://github.com/heriantolim/PeakFit#public-properties) can be specified as arguments in Name-Value syntax during the object construction. Specifying other things as Name-Value arguments will return an error.

## Public Properties
- `Data`, `XData`, `YData`: The data points of the curve to be fitted. Please ensure that the Y data points are all positive, otherwise the peak fitting may not work properly.
- `Window`: A vector of length two [a,b] that limits the fitting to only the data points whose X coordinates lies within [a,b].
- `NumPeaks`: The number of peaks wished to be fitted. When the fitting peak shapes, start points, lower, or upper bounds are set with vectors of length greater than `NumPeaks`, then `NumPeaks` will be incremented to adjust to the maximum length of these vectors. When the maximum length of these vectors is less, then these vectors will be expanded and filled with the default values. When `NumPeaks`=0 and all the start point, lower, and upper are not set, then the program attempts to fit all peaks found in the curve. Defaults to 0.
- `PeakShape`: A string vector that specifies the peak shape of each peak. The choices of `PeakShape` currently are: 'Lorentzian' (1) and 'Gaussian' (2). `PeakShape` may be set with an integer, the initial of these names, e.g. 'L' or 'G', or the full name, e.g. 'Lorentzian'. When the length of `PeakShape` is less than `NumPeaks`, the remaining peaks will be set with the default `PeakShape`, which is 'Lorentzian'. If PeakShape contains only one element, then the default value is the same as that element.

### Fitting Start Points
- `AreaStart`, `CenterStart`, `WidthStart`, `HeightStart`, `BaselineStart`: A vector of initial values for the area, center, width, height, and baseline coefficients, respectively. The default values are determined heuristically. To make certain properties default, set their values to NaN. They will be then replaced with the default values upon fitting.

### Fitting Lower Bounds
- `AreaLow`, `CenterLow`, `WidthLow`, `HeightLow`, `BaselineLow`: A vector of lower bounds for the area, center, width, height, and baseline coefficients, respectively. The default values are determined heuristically. To make certain properties default, set their values to -Inf. They will be then replaced with the default values upon fitting.

### Fitting Upper Bounds
- `AreaUp`, `CenterUp`, `WidthUp`, `HeightUp`, `BaselineUp`: A vector of upper bounds for the area, center, width, height, and baseline coefficients, respectively. The default values are determined heuristically. To make certain properties default, set their values to Inf. They will be then replaced with the default values upon fitting.

### Fit Results
- (Read-only) `Peak`: A struct containing the fit results for each peak.
- (Read-only) `Base`: A struct containing the fit results for the baseline.
- (Read-only) `Area`, `Center`, `Height`, `Width`, `Baseline`: A 3-by-`NumPeaks` matrix that stores the fit results for the area, center, height, width, and baseline, respectively. The first row is the values at convergence; the second row is the 95% CI lower bounds; and the third row is the 95% CI upper bounds.
- (Read-only) `RelStDev`: The relative standard deviation of the fit results.
- (Read-only) `CoeffDeterm`: The coefficient of determination of the fit results.
- (Read-only) `AdjCoeffDeterm`: The degree-of-freedom adjusted coefficient of determination of the fit results.
- (Read-only) `NumFunEvals`: The number of function evaluations.
- (Read-only) `NumIters`: The number of iterations.
- (Read-only) `ExitFlag`: Describes the exit condition of the algorithm. Positive flags indicate convergence, within tolerances. Zero flags indicate that the maximum number of function evaluations or iterations was exceeded. Negative flags indicate that the algorithm did not converge to a solution.

### Algorithm Parameters
- (Read-only) `Method` = 'NonLinearLeastSquares'; The method used for the fitting.
- `Robust`: The type of the least-squares method to be used in the fitting. Avaliable values are 'off', 'LAR' (least absolute residual method), and 'Bisquare' (bisquare weight method). Defaults to 'off'.
- `Algorithm`: The algorithm to be used in the fitting. Available values are 'Lavenberg-Marquardt', 'Gauss-Newton', or 'Trust-Region'. Defaults to 'Trust-Region'.
- `MovMeanWidth`: The window width of the moving average used for smoothing the curve in order to filter out the noise before finding the maximas. This parameter is used only when CenterStart is not given. The value of this can be set as a positive integer which specifies the width in terms of the number of data points, OR a real scalar between [0,1] which specifies the width as a fraction of the total number of data points.
- `DiffMaxChange`: The maximum change in coefficients for finite difference gradients. Defaults to 0.1.
- `DiffMinChange`: The minimum change in coefficients for finite difference gradients. Defaults to 10^-8.
- `MaxFunEvals`: The allowed maximum number of evaluations of the model. Defaults to 50000.
- `MaxIters`: The maximum number of iterations allowed for the fit. Defaults to 10000.
- `TolFun`: The termination tolerance on the model value. Defaults to 10^-6.
- `TolX`: The termination tolerance on the coefficient values. Defaults to 10^-6.

## Public Methods

## Examples

## Best Practices

## See Also
- [BiErfFit](https://github.com/heriantolim/BiErfFit)
