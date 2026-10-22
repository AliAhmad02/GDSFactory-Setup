# GDSFactory Setup Guide

The following guides the user through setting up GDSFactory for development on their machine. For an introductory guide to using the package, see the [Layout Guide](https://gdsfactory.github.io/gdsfactory/notebooks/00_geometry/) in the official documentation. For an overview of the available functions in GDSFactory, see the [Official Reference](https://gdsfactory.github.io/gdsfactory/components/).

## Installation

Firstly, the user should set up a virtual Python environment with the appropriate Python version. Python versions 3.12, 3.13 and 3.14 are officially supported, but the documentation [recommends Python 3.12](https://gdsfactory.github.io/gdsfactory/developer/). For users of Anaconda, the environment can be set up from the command line in the following way:

```
conda create -n gdsfactory python=3.12 
```

This creates a virtual environment with the name "gdsfactory". The environment can subsequently be activated with:

```
conda activate gdsfactory
```

The next step is to install the gdsfactory package and its dependencies. The documentation provides [two installation methods](https://gdsfactory.github.io/gdsfactory/) in the Quick Start guide. We recommend the simplest of the two, which is the following:

```
pip install gdsfactory
```

Users that prefer to manage the dependencies with ``uv`` instead of ``pip`` may refer to the documentation for the alternative installation method. The ``gdsfactory`` package comes with many dependencies, including ``numpy``, ``scipy``, ``pandas``, ``matplotlib``, ``pydantic`` and many more. So it is likely that you already have all that you need!

## Basic Setup

To confirm that everything is working, try to run the following example code and check that it gives no errors:

```
import gdsfactory as gf

# Activate the built-in generic process design kit
gf.gpdk.PDK.activate()

# Create a new component
c = gf.Component()

# Add a rectangle
r = gf.components.rectangle(size=(10, 10), layer=(1, 0))
rect = c.add_ref(r)

# Add text elements
t1 = gf.components.text("Hello", size=10, layer=(2, 0))
t2 = gf.components.text("world", size=10, layer=(2, 0))

text1 = c.add_ref(t1)
text2 = c.add_ref(t2)

# Position elements
text1.xmin = rect.xmax + 5
text2.xmin = text1.xmax + 2
text2.rotate(30)

# Show the result
c.show()
```

At this point, the code above may not result in any output. This is because GDSFactory has two ways of visualizing the geometry. The first is the ``c.show()`` method which is what is used above. This uses GDSFactory's integration with the widely used [KLayout](https://www.klayout.de/) interactive mask viewer. To set up the integration with KLayout, first install KLayout.

KLayout can be opened in either viewer mode or editor mode. We recommend opening the program in viewer mode, since it is generally faster and we will not need any of the editing functionality, since the editing will be done in Python.

To get KLayout to integrate with GDSFactory, you need to install the GDSFactory extension for KLayout. In KLayout, this can be done from the package manager, which is accessed from ``Tools>Manage Packages`` in the top bar. Then you can search for GDSFactory on the "Install New Packages" tab and install the extension.

With the package successfully installed, you should now see options relating to the GDSFactory integration in KLayout, for example, "Restart Klive". At this point, with KLayout open in the background, you should simply re-run the code from earlier and the geometry should be streamed to KLayout automatically. For a demonstration of this, see the video below:

![](gdsfactory_klayout.mp4)

Finally, the second way of displaying the geometries is the ``c.plot()`` method. This method just gives a picture without the interactive functionality of the KLayout viewer. It is equivalent to ``plt.plot()`` with matplotlib. Note that ``c.plot()`` only shows the plot in an interactive environment like a Jupyter Notebook. If you are running the code in a standalone script, you need to import matplotlib and run ``plt.show()`` after ``c.plot()``.