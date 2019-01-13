<h1> 1. Install python and dependencies </h1>

We use the anaconda 2. Download it from [here](https://www.anaconda.com/download/#linux), choose anaconda 2, then you will get a '.sh' file. To install anaconda2, execute:

```
$ bash name_of_the_file.sh
```

Then, add anaconda's bin directory into your path:

```
$ export PATH="/home/path_to_anaconda/anaconda2/bin:$PATH"
```

Next, install dependencies:

```
$ conda install setuptools
$ conda install nose
$ conda install numpy
$ conda install wheel
$ pip install autowrap
```

<h1> 2. Clone and build OpenMS </h1>

<h2> 2.1. Build the contribs before building OpenMS </h2>

To clone and then build contrib, execute:

```
$ mkdir -p ~/BaseOpenMS
$ cd ~/BaseOpenMS

$ git clone  https://github.com/OpenMS/contrib.git
$ mkdir -p ~/BaseOpenMS/contrib-build
$ cd ~/BaseOpenMS/contrib-build

$ cmake -DBUILD_TYPE=ALL -DNUMBER_OF_JOBS=4 ../contrib
```

If you encounter any problems, please refer to the [instructions](http://ftp.mi.fu-berlin.de/pub/OpenMS/release-documentation/html/install_linux.html) page.

<h2> 2.2. Build OpenMS </h2>

Next, we clone the OpenMS repository. Note that you have to provide **the absolute path** to the contrib-build directory for `cmake` command. The absolute path should look like `/home/your_user_name/BaseOpenMS/contrib-build`.

```
$ cd ~/BaseOpenMS

$ git clone https://github.com/OpenMS/OpenMS.git
$ cd ~/BaseOpenMS/OpenMS
$ mkdir ~/BaseOpenMS/OpenMS-build
$ cd OpenMS-build

$ cmake -DOPENMS_CONTRIB_LIBS="/home/your_user_name/BaseOpenMS/contrib-build" -DBOOST_USE_STATIC=OFF ../OpenMS
```

Next, enter the `make` command to start the build process for OpenMS library. `-j4` ensures that the job is done in parallel on 4 CPU's.

```
$ make -j4
```

Again, if you encounter any errors, refer to the [official build instructions of OpenMS](http://ftp.mi.fu-berlin.de/pub/OpenMS/release-documentation/html/install_linux.html). 

<h2> 2.3. Getting your environment ready to use OpenMS </h3>

Edit `~/.bashrc` to add the following two lines. Make sure you edit the _absolute paths_ in both lines according to your system.
The first line adds the path to lib folder of OpenMS-build to the environment variable `LD_LIBRARY_PATH`. 
The second line puts the path to bin folder of OpenMS-build to the environment variable `PATH`. This allows the user to use the commands for TOPP tools from anywhere on the system.

```
export LD_LIBRARY_PATH="/home/your_user_name/BaseOpenMS/OpenMS-build/lib:LD_LIBRARY_PATH"
export PATH=PATH:/home/your_user_name/BaseOpenMS/OpenMS-build/bin
```

Afterwards, restart the terminal for a new session or execute `source ~/.bashrc` so that the changes can take effect for the current session.

<h2> 2.4. Testing your OpenMS/TOPP installation </h2>

Execute the following command to test your installation. 

```
$ make test
```

If everything went well, you should see the following lines at the end of the execution message:

```
100% tests passed, 0 tests failed out of 2017

Total Test time (real) = xxx.xx sec
```

<h1> 3. Install and test pyOpenMS </h2>

<h2> 3.1. Install pyOpenMS </h2>

The recommended way to install pyOpenMS is using `pip` tool, simply execute the following line and it's done:

```
$ pip install pyopenms
```

Optionally: If you want to install the pyopenms into a specific location, use:
```
$ pip install pyopenm --target="/home/path_to_destination/pyopenms"
```
Then add that directory into the `PYTHONPATH`:
```
$ export PYTHONPATH="/home/path_to_destination/pyopenms:$PYTHONPATH"
```

Of course you can also build pyopenms from source, please refer to [instructions for pyOpenMS](http://ftp.mi.fu-berlin.de/pub/OpenMS/release-documentation/html/pyOpenMS.html#build). We recommend to use `pip` tool, it's easier and faster.

<h2> 3.2. Test pyOpenMS </h2>

To check your installation, the following command should not give any errors.
```
$ python -c "from pyopenms import BilinearInterpolation, MzMLFile, FeatureXMLFile, FeatureMap, MSExperiment, SplineSpectrum"
```

<h1> Other python packages </h1>
Python packages required for image creation and HDF5 data format can be installed as:

```
$ pip install pillow==4.2.1
$ pip install h5py==2.7.0
$ pip install scipy==0.19.1
```
