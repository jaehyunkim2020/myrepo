# myrepo
This is a line from RStudio

I want to go skydiving, cliff diving, or anything else exhilarating

## **Notes from Task 5 reading**

ggplot2 is one of the most elegant and most versatile out of all the graphs in R.

The first argument of ggplot() is the dataset to use in the graph. So ggplot(data = mpg) creates an empty graph. The graph is completed by adding one or more layers to ggplot(). The function geom_point() adds a layer of points to the plot, which creates a scatterplot. ggplot2 comes with many geom functions that each add a different type of layer to a plot.

# *Aesthetic mappings*

Each geom function in ggplot2 takes a mapping argument which defines how variables in the dataset are mapped to visual properties. The mapping argument is always paired with aes(), and the x and y arguments of aes() specify which variables to map to the x and y axes. ggplot2 looks for the mapped variables in the data argument.

ggplot(data = <DATA>) +
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))
  
You can add a third variable to a two dimensional scatterplot by mapping it to an *aesthetic*. An aesthetic is a visual property of the objects in your plot. Aesthetics include things like the size, the shape, or the color of your points.

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, color = class))
  
To map an aesthetic to a variable, associate the name of the aesthetic to the name of the variable inside aes(). ggplot2 will automatically assign a unique level of the aesthetic to each unique value of the variable, a process known as *scaling*. ggplot2 will also add a legend that explains which levels correspond to which values.

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, alpha = class))
# alpha controls the transparency of the points

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, shape = class))
# shape controls the shape of the points

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
# changes all the points in the plot to the color of blue. Needs to be outside the aes
  
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, stroke = cyl))
# stroke defines the stroke thickness
  
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, colour = displ < 5))
  
# *Facets*

One way to add additional variables is with aesthetics. Another way, particularly useful for categorical variables, is to split your plot into facets, subplots that each display one subset of the data.

To facet your plot by a single variable, use facet_wrap(). The first argument of facet_wrap() should be a formula, which you create with ~ followed by a variable name. The variable that you pass to facet_wrap() should be discrete.

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_wrap(~ class, nrow = 2)

To facet your plot on the combination of two variables, add facet_grid() to your plot call. The first argument of facet_grid() is also a formula. This time the formula should contain two variable names separated by a ~. 

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_grid(drv ~ cyl)

If you prefer to not facet in the rows or columns dimension, use a . instead of a variable name, e.g. + facet_grid(. ~ cyl).

# *Geometric objects*

A *geom* is the geometrical object that a plot uses to represent data. To change the geom in your plot, change the geom function that you add to ggplot().

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy))
  
 ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy))

Every geom function in ggplot2 takes a mapping argument. However, not every aesthetic works with every geom. You could set the shape of a point, but you couldn't set the "shape" of a line. On the other hand, you could set the linetype of a line. geom_smooth() will draw a different line, with a different linetype, for each unique value of the variable that you map to linetype.

ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv))
# Here geom_smooth() separates the cars into three lines based on their drv value, which describes a car's drivetrain. One line describes all of the points with a 4 value, one line describes all of the points with an f value, and one line describes all of the points with an r value.

ggplot2 provides over 40 geoms, and extension packages provide even more (see https://exts.ggplot2.tidyverse.org/gallery/ for a sampling). The best way to get a comprehensive overview is the ggplot2 cheatsheet, which you can find at http://rstudio.com/resources/cheatsheets. To learn more about any single geom, use help: ?geom_smooth.


To display multiple geoms in the same plot, add multiple geom functions to ggplot():

ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy))

Avoid repetition by passing a set of mappings to ggplot(). ggplot2 will treat these mappings as global mappings that apply to each geom in the graph. In other words, this code will produce the same plot as the previous code:

ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point() +
  geom_smooth()

If you place mappings in a geom function, ggplo2 will treat them as local mappings for the layer. It will use these mappings to extend or overwrite the global mappings for that layer only. This makes it possible to display different aesthetics in different layers.

ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point(mapping = aes(color = class)) +
  geom_smooth()

You can use the same idea to specify different data for each layer. Here, our smooth line displays just a subset of the mpg dataset, the subcompact cars. The local data argument in geom_smooth() overrides the global data argument in ggplot() for that layer only.

ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point(mapping = aes(color = class)) +
  geom_smooth(data = filter(mpg, class == "subcompact"), se = FALSE)