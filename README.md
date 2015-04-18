# ogre_binding
 * simple installation and binding for ogre, install that lib to use ogre in [LMS](https://github.com/Phibedy/LMS)

# Installation guide
````sh
# Start from the LMS directory and go to the libraries directory
cd lms/external/libraries

# clone the current version
git clone https://github.com/Phibedy/ogre_binding

# Change into the new directory and start the install script
cd ogre_binding
./install_fw.sh

# Add ogre_binding to CMakeData.txt
# Should look like: set(LIBRARIES ogre_binding)
cd ..
nano CMakeData.txt

# Change to your build directory and build.
# Then you are ready to go!
````

###TODO
 * add script for win and macosx
 * some docs
