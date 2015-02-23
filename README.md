# How to Export Labview Data into MATLAB with lvmread.m

Data in Labview can be exported to an lvm (similar to a .csv) file, which
contains a header and comma or tab seperated columns of data.

## Getting Started

1. To get started, set up the DAQ and choose your sample time appropriately.
Keep in mind that faster sampling rates will produce a lot more data and
introduce additional noise.

2. Once your VI is setup, add the *Write To Measurement File* block that
records signals to an .lvm file. You can find it in the function palette:
*Programming >> File I/O >> Write To Measurement File*. Then, connect the
time-domain signal coming out of the DAQ block to the *Write To
Measurement File* block. Don't worry about filtering or processing right
now, because you can always do this later in MATLAB.

3. Right-click on the *Write To Measurement File* block, and click
*Properties*. This will bring up a window to configure the block.
Choose the following settings:
    * `File Format = Text (LVM)`
    * `Segment Headers = One header only`
    * `X Value (Time) Columns = One column per channel`
    * `Delimiter = Comma`
And choose a filename ending in .lvm to save your data into, and then
click *OK* to close the configuration menu.

4. Start recording by clicking *Run* or *Run Continuously*. To stop, press
*Stop*. Data will be recorded to the specified file for as long as your
VI is running. Be careful about overwriting previously recorded data by
pressing *Run* twice. You should set a new filename or rename the first
one to prevent data loss. 

5. Move lvmread.m to the directory where you saved your .lvm file (or
visa versa). Now, open MATLAB and change to that same directory by typing:
`cd directory/with/mydata.lvm`, replacing the path with the directory
where you saved your data to. 

6. Read the data into the variable `data` by typing:
`data = lvmread('my_measurement_file.lvm')`. Each signal is recorded
into columns with a paired time column preceding it. For example, an
.lvm file with only one signal recorded will have two columns of
data, where the first column x holds the time steps of 1/f_s, and the
second column is y, which represents the discreted voltage measured at
the different timesteps by the ADC.

You can access split the data into the x and y columns with:
`x = data(:,1); y = data(:,2)`, which saves column 1 of data into `x`,
and column 2 of data into `y`. See notes below for plotting the data.

## Notes on Plotting Data 

Using MATLAB, you can customize your plots in many different ways. This
text will not delve very deeply into how to make MATLAB plots. Use the
help docs (http://www.mathworks.com/help/matlab/ref/plot.html) and google
for more info.

Here are a few useful commands for manipulating plots. Note that these
commands affect the most recently opened figure. You can switch between
figures with the `figure(i)` command, where `i` is the number of figure
you want to siwtch to. Click *Window* in the menubar to see a list.

* `title('My Title')` - Sets the title of the plot
* `xlabel('Time (sec)')`  - Sets the label of the x-axis
* `ylabel('Voltage (V)')` - Sets the label for the y-axis
* `xlim([0 5])` - Sets the window range for the x-axis (from 0 to 5)
* `ylim([-10 10])` - Sets the window range for the y-axis

## Notes

 * In the .lvm file after the header, the first value in each pair of columns
   is the sample time, and second is the measured value at that time. Each
   additional signal will add another pair of columns in this way.

 * When saving MATLAB plots to files choose .png format for the best results.
   Note that the size of the window when you save it will be the same size
   as the image, so drag it larger to improve the quality.

