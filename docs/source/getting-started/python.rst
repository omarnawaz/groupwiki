Python Basics and Best Practices
========================================

Python is the primary language for scientific computing in the CASA group. This guide covers the fundamentals of Python coding, managing packages with Conda, and introduces libraries commonly used in atmospheric science.

Python Basics
------------------------------------

**Running Python**

You can run Python in several ways:

.. code-block:: bash

   python                          # Interactive Python shell
   python script.py                # Run a Python file
   python -c "print('hello')"      # Execute Python code directly
   ipython                         # Enhanced interactive shell (if installed)
   jupyter notebook                # Jupyter notebook environment

**Basic Syntax**

Variables and assignments:

.. code-block:: python

   name = "Alice"                  # String
   age = 25                        # Integer
   temperature = 23.5              # Float
   is_valid = True                 # Boolean
   items = [1, 2, 3]              # List
   coords = (10, 20)              # Tuple
   data = {"name": "Alice", "age": 25}  # Dictionary

Print output:

.. code-block:: python

   print("Hello, world!")
   print(f"Name: {name}, Age: {age}")  # f-strings (modern approach)
   print("Name: {}, Age: {}".format(name, age))  # Older approach

**Control Flow**

If statements:

.. code-block:: python

   if temperature > 30:
       print("Hot!")
   elif temperature > 20:
       print("Warm")
   else:
       print("Cold")

Loops:

.. code-block:: python

   # For loop
   for i in range(5):
       print(i)  # Prints 0, 1, 2, 3, 4

   # For loop over list
   for item in items:
       print(item)

   # While loop
   count = 0
   while count < 5:
       print(count)
       count += 1

   # List comprehension (powerful!)
   squares = [x**2 for x in range(5)]  # [0, 1, 4, 9, 16]

**Functions**

Define and use functions:

.. code-block:: python

   def greet(name):
       """Greet someone by name."""
       return f"Hello, {name}!"

   result = greet("Alice")
   print(result)  # Hello, Alice!

   # Functions with default arguments
   def add(a, b=0):
       """Add two numbers."""
       return a + b

   print(add(5))        # 5 (b defaults to 0)
   print(add(5, 3))     # 8

   # Functions with multiple return values
   def get_min_max(numbers):
       return min(numbers), max(numbers)

   min_val, max_val = get_min_max([1, 5, 3, 9, 2])

**Working with Lists and Dictionaries**

Lists:

.. code-block:: python

   fruits = ["apple", "banana", "cherry"]

   # Access by index
   print(fruits[0])            # apple
   print(fruits[-1])           # cherry (last item)

   # Slicing
   print(fruits[0:2])          # ["apple", "banana"]
   print(fruits[1:])           # ["banana", "cherry"]

   # Methods
   fruits.append("date")       # Add item
   fruits.remove("banana")     # Remove specific item
   length = len(fruits)        # Get length
   fruits.sort()               # Sort in place

Dictionaries:

.. code-block:: python

   person = {"name": "Alice", "age": 25, "city": "Cardiff"}

   # Access by key
   print(person["name"])       # Alice

   # Add or modify
   person["age"] = 26
   person["email"] = "alice@example.com"

   # Methods
   keys = person.keys()        # Get all keys
   values = person.values()    # Get all values
   for key, value in person.items():
       print(f"{key}: {value}")

Managing Python Environments with Conda
------------------------------------

**What is Conda?**

Conda is a package and environment manager. It lets you:
- Install Python packages easily
- Create isolated environments for different projects
- Manage package versions to avoid conflicts
- Share environments with colleagues

**Installing Conda**

Conda comes with Anaconda or Miniconda. For Falcon:

.. code-block:: bash

   module load anaconda3           # Load Conda on Falcon
   conda --version                 # Check installation

**Creating Environments**

Create a new environment:

.. code-block:: bash

   conda create -n myproject python=3.9         # Python 3.9
   conda create -n myproject python=3.10        # Python 3.10

Specify packages when creating:

.. code-block:: bash

   conda create -n myproject python=3.9 numpy pandas matplotlib

**Activating/Deactivating Environments**

Activate an environment:

.. code-block:: bash

   conda activate myproject

You'll see ``(myproject)`` in your terminal prompt when it's active.

Deactivate:

.. code-block:: bash

   conda deactivate

**Installing and Updating Packages**

Install a package:

.. code-block:: bash

   conda install numpy              # Latest version
   conda install numpy=1.21.0       # Specific version
   conda install numpy pandas scipy # Multiple packages

Update a package:

.. code-block:: bash

   conda update numpy

Remove a package:

.. code-block:: bash

   conda remove numpy

**Listing Packages**

See installed packages:

.. code-block:: bash

   conda list                       # All packages
   conda list | grep numpy          # Search for a specific package

**Sharing Environments**

Export your environment to a file:

.. code-block:: bash

   conda env export > environment.yml

Someone else can recreate it:

.. code-block:: bash

   conda env create -f environment.yml

Or create from a minimal YAML file:

.. code-block:: yaml

   # Create a file called environment.yml
   name: myproject
   channels:
     - conda-forge
   dependencies:
     - python=3.9
     - numpy
     - pandas
     - matplotlib
     - xarray
     - netcdf4

Then create it:

.. code-block:: bash

   conda env create -f environment.yml

**Useful Conda Commands**

.. code-block:: bash

   conda info                      # Show conda configuration
   conda env list                  # List all environments
   conda env remove -n myproject   # Delete an environment
   conda search numpy              # Search for package versions
   conda clean --all              # Clean cache (frees disk space)

Common Libraries
------------------------------------

**NumPy — Numerical Computing**

NumPy provides arrays and mathematical functions. Most scientific Python builds on NumPy.

.. code-block:: python

   import numpy as np

   # Create arrays
   arr = np.array([1, 2, 3, 4, 5])
   zeros = np.zeros((3, 3))           # 3x3 array of zeros
   ones = np.ones((2, 4))             # 2x4 array of ones
   range_arr = np.arange(0, 10, 2)   # [0 2 4 6 8]

   # Array operations
   arr2d = np.array([[1, 2], [3, 4], [5, 6]])
   print(arr2d.shape)                  # (3, 2)
   print(arr2d[0, 1])                  # 2 (element at row 0, col 1)

   # Math operations (element-wise)
   result = arr * 2                    # [2 4 6 8 10]
   result = np.sin(arr)
   result = np.exp(arr)

   # Useful functions
   mean = np.mean(arr)
   std = np.std(arr)
   max_val = np.max(arr)
   min_val = np.min(arr)

   # Reshaping
   reshaped = arr.reshape(5, 1)
   flattened = arr2d.flatten()

**Pandas — Tabular Data**

Pandas handles table-like data (like spreadsheets or databases). Essential for climate data analysis.

.. code-block:: python

   import pandas as pd

   # Create a DataFrame (table)
   data = {
       'date': ['2024-01-01', '2024-01-02', '2024-01-03'],
       'temperature': [15.2, 16.1, 14.8],
       'humidity': [65, 70, 68]
   }
   df = pd.DataFrame(data)

   # Access columns
   temps = df['temperature']
   print(temps[0])                     # 15.2

   # Access rows
   row = df.iloc[0]                    # First row as Series
   row = df.loc[0]                     # Access by label

   # Filtering
   hot_days = df[df['temperature'] > 15]

   # Statistics
   print(df['temperature'].mean())      # 15.366...
   print(df['temperature'].std())
   print(df.describe())                # Summary statistics

   # Read from file
   df = pd.read_csv('data.csv')
   df = pd.read_excel('data.xlsx')

   # Write to file
   df.to_csv('output.csv', index=False)
   df.to_excel('output.xlsx')

   # Basic operations
   df['new_column'] = df['temperature'] * 1.8 + 32  # Fahrenheit
   df.drop('humidity', axis=1)         # Drop a column
   df.dropna()                         # Remove rows with missing values

**Matplotlib — Plotting**

Create visualizations:

.. code-block:: python

   import matplotlib.pyplot as plt
   import numpy as np

   # Simple plot
   x = np.linspace(0, 10, 100)
   y = np.sin(x)

   plt.plot(x, y, label='sin(x)')
   plt.xlabel('X')
   plt.ylabel('Y')
   plt.title('Sine Wave')
   plt.legend()
   plt.show()

   # Multiple plots
   fig, axes = plt.subplots(2, 2, figsize=(10, 8))

   axes[0, 0].plot(x, np.sin(x))
   axes[0, 0].set_title('sin(x)')

   axes[0, 1].plot(x, np.cos(x))
   axes[0, 1].set_title('cos(x)')

   axes[1, 0].scatter(x[:20], y[:20])
   axes[1, 0].set_title('scatter')

   axes[1, 1].hist(np.random.normal(0, 1, 1000))
   axes[1, 1].set_title('histogram')

   plt.tight_layout()
   plt.savefig('plots.png', dpi=300)
   plt.show()

**xarray — Labeled Multidimensional Data**

Xarray is built on top of NumPy and designed for climate/weather data with named dimensions.

.. code-block:: python

   import xarray as xr
   import numpy as np

   # Create a DataArray (labeled array)
   data = np.random.randn(10, 20)
   da = xr.DataArray(
       data,
       dims=['lat', 'lon'],
       coords={'lat': np.arange(10), 'lon': np.arange(20)},
       name='temperature'
   )

   # Access by label (not just index!)
   print(da.sel(lat=5, lon=10).values)  # Get value at lat=5, lon=10

   # Slice by dimension
   subset = da.sel(lat=slice(2, 7), lon=slice(5, 15))

   # Read from NetCDF file (common format for climate data)
   ds = xr.open_dataset('temperature.nc')
   print(ds)                           # Show dataset info
   print(ds.data_vars)                 # Show all variables
   temps = ds['temperature']           # Get a variable

   # Group and aggregate
   mean_by_lat = da.mean(dim='lon')   # Average over longitude

   # Save to NetCDF
   da.to_netcdf('output.nc')

**SciPy — Scientific Functions**

Advanced mathematical functions:

.. code-block:: python

   import scipy.stats as stats
   from scipy.optimize import minimize

   # Statistics
   p_value = stats.ttest_ind(group1, group2)  # t-test

   # Optimization
   def objective(x):
       return (x - 3)**2

   result = minimize(objective, x0=0)
   print(result.x)  # Minimum value

   # Interpolation
   from scipy.interpolate import interp1d
   f = interp1d(x_data, y_data, kind='cubic')
   y_interp = f(x_new)

Code Organization
------------------------------------

**Scripts vs. Functions**

Bad: Everything in one file

.. code-block:: python

   # analysis.py
   data = [1, 2, 3, 4, 5]
   mean = sum(data) / len(data)
   print(mean)

Good: Organize into functions

.. code-block:: python

   # analysis.py
   def calculate_mean(data):
       """Calculate the mean of data."""
       return sum(data) / len(data)

   def main():
       data = [1, 2, 3, 4, 5]
       mean = calculate_mean(data)
       print(mean)

   if __name__ == "__main__":
       main()

**Module Organization**

For larger projects, organize code:

.. code-block:: text

   myproject/
   ├── data/
   │   ├── raw/
   │   └── processed/
   ├── src/
   │   ├── __init__.py
   │   ├── preprocessing.py
   │   ├── analysis.py
   │   └── plotting.py
   ├── scripts/
   │   ├── process_data.py
   │   └── run_analysis.py
   └── environment.yml

**Importing from Modules**

In ``src/preprocessing.py``:

.. code-block:: python

   def load_data(filename):
       """Load data from file."""
       import pandas as pd
       return pd.read_csv(filename)

   def clean_data(df):
       """Remove missing values."""
       return df.dropna()

In ``scripts/process_data.py``:

.. code-block:: python

   import sys
   sys.path.insert(0, '../src')

   from preprocessing import load_data, clean_data

   df = load_data('../data/raw/input.csv')
   df_clean = clean_data(df)
   df_clean.to_csv('../data/processed/output.csv')

Best Practices
------------------------------------

**1. Use Virtual Environments**

Never code in the base Python environment. Always use conda environments:

.. code-block:: bash

   conda create -n myproject python=3.9
   conda activate myproject
   conda install numpy pandas matplotlib

**2. Use Meaningful Variable Names**

Bad:

.. code-block:: python

   x = [1, 2, 3]
   y = sum(x) / len(x)

Good:

.. code-block:: python

   temperatures = [15.2, 16.1, 14.8]
   mean_temp = sum(temperatures) / len(temperatures)

**3. Write Docstrings**

.. code-block:: python

   def calculate_anomaly(data, climatology):
       """
       Calculate anomalies from climatology.

       Parameters
       ----------
       data : array-like
           Data values
       climatology : float
           Climatological mean

       Returns
       -------
       anomaly : array-like
           Anomalies relative to climatology
       """
       return data - climatology

**4. Handle Errors**

.. code-block:: python

   try:
       df = pd.read_csv('data.csv')
   except FileNotFoundError:
       print("Error: File not found!")
   except Exception as e:
       print(f"Unexpected error: {e}")

**5. Use Type Hints (Python 3.5+)**

.. code-block:: python

   def add(a: float, b: float) -> float:
       """Add two numbers."""
       return a + b

   def process_data(data: list[int]) -> dict[str, float]:
       """Process a list of integers."""
       return {
           'mean': sum(data) / len(data),
           'max': max(data),
           'min': min(data)
       }

**6. Test Your Code**

.. code-block:: python

   # test_analysis.py
   from analysis import calculate_mean

   def test_calculate_mean():
       assert calculate_mean([1, 2, 3, 4, 5]) == 3.0
       assert calculate_mean([10]) == 10.0
       print("All tests passed!")

   if __name__ == "__main__":
       test_calculate_mean()

Run with:

.. code-block:: bash

   python test_analysis.py

Common Workflows
------------------------------------

**Workflow 1: Data Processing**

.. code-block:: python

   import pandas as pd
   import numpy as np

   # Load data
   df = pd.read_csv('temperature_data.csv')

   # Inspect
   print(df.head())
   print(df.info())
   print(df.describe())

   # Clean
   df = df.dropna()
   df['date'] = pd.to_datetime(df['date'])

   # Process
   df['anomaly'] = df['temperature'] - df['temperature'].mean()

   # Filter
   winter = df[df['month'].isin([12, 1, 2])]

   # Save
   winter.to_csv('winter_data.csv', index=False)

**Workflow 2: Data Analysis and Plotting**

.. code-block:: python

   import xarray as xr
   import matplotlib.pyplot as plt

   # Load NetCDF file
   ds = xr.open_dataset('temperature.nc')

   # Extract variable
   temp = ds['temperature']

   # Compute statistics
   mean_temp = temp.mean(dim=['lat', 'lon'])
   std_temp = temp.std(dim=['lat', 'lon'])

   # Plot
   fig, axes = plt.subplots(1, 2, figsize=(12, 4))

   mean_temp.plot(ax=axes[0])
   axes[0].set_title('Mean Temperature')

   std_temp.plot(ax=axes[1])
   axes[1].set_title('Std Dev Temperature')

   plt.tight_layout()
   plt.savefig('temperature_stats.png', dpi=300)

**Workflow 3: Batch Processing**

.. code-block:: python

   import os
   import glob
   import pandas as pd

   # Process multiple files
   for filepath in glob.glob('data/raw/*.csv'):
       print(f"Processing {filepath}")

       df = pd.read_csv(filepath)
       df = df.dropna()
       df['processed'] = df['value'] * 2

       outfile = filepath.replace('raw', 'processed')
       df.to_csv(outfile, index=False)

   print("Done!")

Debugging Tips
------------------------------------

**Using Print Statements**

.. code-block:: python

   x = 10
   print(f"DEBUG: x = {x}, type = {type(x)}")

**Using Python Debugger (pdb)**

.. code-block:: python

   import pdb

   def problematic_function(x):
       pdb.set_trace()  # Execution pauses here
       result = x * 2
       return result

   # In the debugger:
   # n  - next line
   # s  - step into function
   # c  - continue
   # p var - print variable
   # l  - list code
   # q  - quit

Or use interactive debugging:

.. code-block:: bash

   python -m pdb script.py

**Using VS Code Debugger**

With Remote-SSH and the Python extension:
1. Set breakpoints by clicking line numbers
2. Press F5 to start debugging
3. Step through code with F10/F11
4. Inspect variables in the sidebar

Need Help?
------------------------------------

- **Python docs**: https://docs.python.org/3/
- **NumPy docs**: https://numpy.org/doc/
- **Pandas docs**: https://pandas.pydata.org/docs/
- **Matplotlib docs**: https://matplotlib.org/stable/contents.html
- **xarray docs**: https://docs.xarray.dev/
- **Stack Overflow**: Search your error message
- **Ask Omar or colleagues**: They've likely encountered similar issues
- **Read existing code**: Look at scripts from groupmates for examples
