
This is instructions for creating a hello_world executable that will use the 
model trained in colab.

It is encouraged that you create your own notebook and run it, but you can use the completed one at:
https://colab.research.google.com/github/tensorflow/tflite-micro/blob/18aec279a0f35af82e4543feae00e1c87a75c8bf/tensorflow/lite/micro/examples/hello_world/train/train_hello_world_model.ipynb#scrollTo=j1FB4ieeg0lw





Assumes base directory for me of:
/home/erke9581/embeddedai-forclass

Will use <basedir> when I note


## For Hello World

STEP 0) 

clone tflite repo

git clone https://github.com/tensorflow/tflite-micro.git


STEP 1) Need to build the library: libtensorflow-microlite.a

For info: open tensorflow/lite/micro/tools/make/Makefile
-- the default make target is  this library
# The default Makefile target(all) must appear before any target,
# which is compiled if there's no command-line arguments.
all: $(MICROLITE_LIB_PATH)

Note: You'll need some things installed, e.g., g++, make, git, unzip. 
Below are what I recall, but as errors come up, use: sudo apt install <something>

sudo apt update
sudo apt install build-essential
sudo apt install unzip 


Command to run to build the library:

<basedir>/tflite-micro$ make -f tensorflow/lite/micro/tools/make/Makefile

(this will take a while)

At the end, this file should exist:
<basedir>/tflite-micro/gen/linux_x86_64_default_gcc/lib/libtensorflow-microlite.a




(optional) Step 2) Look in tflite-micro/tensorflow/lite/micro/examples/hello_world

This (and others in the examples directory) are provided in the repo.  They use the bazel build tool, which I didn't use.  But, for how they're recommending on building, see this directory.  This include BUILD and Makefile.inc and a bunch of python files doing a variety of stuff.  

Also use this for guidance on writing the code.

We're compiling outside of the tensorflow repo and build system, as we'll want to be writing code for an embedded system and use their build system.

We'll create a Makefile and hello_world.cc from scratch (copying code snipits):

Good doc for understanding API:
  https://medium.com/@jahiz.ahmed/deep-dive-into-the-tensorflow-workflow-2dfa211475d1

Makefile will be provided -- there's one place you need to edit.

 

STEP 3) Modify the model data structure.

For a tflite model named <NAME>.tflite, e.g., hello_world_float.tflite, the output array will be named <NAME>_tflite.

After you run this command (in colab):
xxd -i hello_world_float.tflite > hello_world_float_model_data.cc

There will be an array as follows:
unsigned char hello_world_float_tflite[] = {
...
}

For readability, we'll rename the array to g_hello_world_float_model_data
And we'll create a hello_world_float_model_data.h file to define the array.

#ifndef  HELLO_WORLD_FLOAT_MODEL_DATA_H
#define  HELLO_WORLD_FLOAT_MODEL_DATA_H

extern unsigned char g_hello_world_float_model_data[];

#endif  

In the .cc file, be sure to include the header:
#include "hello_world_float_model_data.h"

STEP 4) 

edit hello_world.cc
   Modify MYNAMESTR to be your first and last names.
   Cut and Paste each of the snippets of code from the bottom into the appropriate place.



STEP 5) 

Build the executable (the default target is the hello_world executable, so you can just run make), and run it.

make 

./hello_world


STEP 6) HANDING IN

screen capture the output of hello_world (at least 5 rows), and store as file in this directory

Add the jupyter notebook to this repo. (or a README_NOTEBOOK that mentions you used the provided one directly)

Add the tflite file (output of the jupyter notebook) to the git repo.

commit and push code to github

In canvas enter in url of repo in textbox of submission
