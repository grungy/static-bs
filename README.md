# static-bs
A simple 3D electro-magnetostatic biot-savart solving simulator written in python.

No dependencies beyond numpy and scipy

Sometimes you don't want a giant framework. Sometimes you just want some quick and easy bs!

**Authors:** Josh Marks and Andrea Waite
****
### What questions can static-bs answer?

Static-bs can answer questions like these:

- What is the B field at a specific point for a coil of wire without having to use a large volume of points like a finite element solver?
- What does that 3D field look like?
- How does the B field change if I have a bunch of coils oriented in a bunch of different ways?
- What is the electric field if I have charged particles moving through the B field?
- What is the voltage field I see from this electric field?

### What is static-bs not good at doing?

- Evaluating a large number of points or a lot of line segments quickly


### How do you use this fangled thing?

##### Start in the main.py file

   Here you will find an example experiment setup.

##### Setting up the observation grid and current

The observation grid is the collection of points where the B field will be evaluated.

 1. Adjust the observation grid to the dimensions and point density that you require for your experiment.

 2. If you don't care about looking at one of the dimensions you can decrease the processing
  time by not including it. Look for the section in the code called "Example for how to ignore the z axis."

 3. Set the current you would like in each loop of the coil and how many loops you would like.

##### Creating your coil

Currently, there is a polygon class that can make regular polygons to approximate your coil shape. If you need a non-regular polygon shape, then create a class that inherits from the loop class and calculates the vertices of the non-regular shape. The new shape class then needs to call create_segments and pass in the vertices. Consider pushing your shape to the repo so others may use it too! Copy and paste the code block below into your shape class to do this.

```python
self.segments = self.create_segments(self.vertices)
```

  1. Look in the section called: *create coil*
  2. Set the coil radius
  3. Pass the number of sides, the center point of the polygon, and the polygon radius to the polygon class

##### Making a current object
   1. Look in the section: *make a list of all of the current segments*
   2. Call the get_segments() function of your coil.
   3. If you have multiple coils simply add the outputs of this function call for each coil. See the code block below:

```python
# make a list of all of the current segments
current_object = bs.iobjs.current_obj_list(coil.get_segments() + coil2.get_segments())
```

##### Don't forget to save your data by calling 'save()' function at the end of the file.
  1. The save function will by default save in the data directory. This can be changed by passing a new value for the data_dir parameter.
  2. Pass filename = data into the save function and it will save the data as the filename provided.

#### Run main.py from the command line

#### Visualize your results by using plot.py
   1. Plot.py takes two command line arguments:
      * the location of the B field data
      * the location of the E field data if applicable



### Output examples

#### B field plot of two regular octagons with a radius of 71.53mm in Helmholtz coil configuration
![B field plot of two regular octagons with a radius of 71.53mm in Helmholtz coil configuration]( https://github.com/grungy/static-bs/blob/master/imgs/regular_octagon_helmholtz_71.53mm_radius.png)

#### E field plot of two regular octagons with a radius of 71.53mm in a Helmholtz coil configuration and a fluid moving uniformly in the x direction at 1cm / s
![E field plot of two regular octagons with a radius of 71.53mm in a Helmholtz coil configuration and a fluid moving uniformly in the x direction at 1cm / s](https://github.com/grungy/static-bs/blob/master/imgs/regular_octagon_helmholtz_71.53mm_radius_e_field.png)

### How does staticbs work?

Static-bs works by solving the biot-savart law for finite line segments. The exact solution Static-bs uses can be found on page 9-4 (page 4 in the pdf) of this [course notes guide from MIT](http://web.mit.edu/viz/EM/visualizations/coursenotes/modules/guide09.pdf). Look for the section titled: "Example 9.1: Magnetic Field due to a Finite Straight Wire."

The vertices returned by the polygon class or a user created class are used to create straight wire segments. The B fields from these segments are then vector added together to obtain the final answer.


### Things that still need to be done

2. Update plot.py to automagically determine which axis you might have ignored and plot appropriately.

5. Implement some kind of SVG gobbling class, so coils can be drawn and easily imported

6. Time varying solutions?

4. Easily reverse the order of the vertices to change the direction of current flow.
