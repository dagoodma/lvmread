# How to Export Labview Data into MATLAB with lvmread.m

Data in Labview can be exported to an lvm (similar to a .csv) file, which
contains a header and comma or tab seperated columns of data.

## Getting Started

1. To get started, set up the DAQ and choose your sample time appropriately.
Keep in mind that faster sampling rates will produce a lot more data and
introduce additional noise.

2. Once you have you set up your VI how you lik, add the *Write To
Measurement File* block that records signals to an .lvm file. You can find
it in the function palette: *Programming >> File I/O >> Write To
Measurement File*. Then, connect the time-domain signal coming out of the
DAQ block to the *Write To Measurement File* block. Don't worry about
filtering before this point unless you know what you are doing, because
you can do so later in MATLAB.

3. Right-click on the *Write To Measurement* File block, and click *Properties*.
This will bring up a window to configure the block.  Set `File Format = Text
(LVM)`, `Segment Headers = One header only`, `X Value (Time) Columns = One
column per channel`, and `Delimiter = Comma`.  Lastly, choose an .lvm
filename to save your data to and then click OK to close the configuration
menu.

4. Start recording by clicking *Run* or *Run Continuously*. To stop, press
*Stop*. This will have recorded data to the specified file for as long as you
let the VI run. Beware about overwriting previously recorded data. You can
select a new filename or rename the old one to prevent loss of data.

5. Move lvmread.m to the directory where you saved your .lvm file (or
visa versa). Now, open MATLAB and change to that same directory. Run
`data = lvmread('my_measurement_file.lvm')` to open the file and read the
columns of data that you recorded. For an .lvm file with two columns of
data, ie. only one signal (x and y axes) was connected to the block for
exporting , you can get each with: `x = data(:,1); y = data(:,2)`, which
returns all rows for columns 1 and 2 respectively.

## Notes on Plotting Data 

Using MATLAB, you can customize your plots many different ways. This text will
not go into depth on this (that's what google is for), but I would like to list
a few commands that will help you in customizing your MATLAB plots. These
commands affect the most recently opened figure. To switch to a different
figure use `figure(i)` first, where i is the number of the figure you want to
choose (click *Window* to see list of opened figures).

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

