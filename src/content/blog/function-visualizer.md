---
title: 'Function Visualizer'
description: 'Here is a sample of some basic Markdown syntax that can be used when writing Markdown content in Astro.'
pubDate: 'February 17 2024'
heroImage: '/post-17-2.png'
---

This project is one of my favourite: A function visualiazation application. The project is completely written in Java and the JavaFX library. Let's dive in!

## The Idea

At first: Why would somebody needs that? That's a good question, because such programms aren't new and there are plenty of them, but it makes a loot of sense, talking about the idea behind the application in example:
How can a linear function be implemented in a program? <br>
After many math lessons, I got the idea: Why should I do allways the same steps, when I can automate it? So I build the first version of the FunctionVisulizer project.

## The Code Base
I stuck with Java and JavaFX and the Maven-Build, so I could implement easly dependencies. I used a libaray called <mark>"jfxutils"</mark> to make life easier when implementing a scale to the coordinate system.<br> Here you can compy the dependency:
```
<dependency>
    <groupId>org.gillius</groupId>
    <artifactId>jfxutils</artifactId>
    <version>1.0</version>
</dependency>
```
Here are is the base structure:
<br>

#### FunctionVisualizer.java:<br>
The foundation of the project. Here is the whole UI implemented and combinates the logic of the functions.
#### CalculationThread.java:<br>
In the CalculationThread class the allocated function will be parsed to the fitting class. In addition the ``` scrollEvent() ``` function has been implemented to find automatically the intersection point where the graph intersect with the y-axis.
#### The function package:
I don't explain the different function types, so I summarize them. In order to display the function, I used allways the same methode:
<br>

```
ObservableList<Coordinate> dataPoints = FXCollections.observableArrayList();
series.getData().clear();
for (double x = -range; x <= range; x++) {
    double y = m * x + b;
    dataPoints.add(new Coordinate(x, y));
    if (x == -range || x == range) {
        series.getData().add(new XYChart.Data<>(x, y));
    }
}
pointTable.setItems(dataPoints);
```

In the for-loop the x coordinates have the same value as the negative range variable which can be changed by the user's input. Then it will count to the positive range value. At the same time for each x-value the fitting y value will be calculated with the intersection point (here 'b') which is also writeable for the user. After that process the ObservableList save the coordinates of the certain point and if the x value is the smallest or the biggest it will be added to the chart.<br><br>
Obviously for each function type the y-formula is different, but in the in the end all of them have the same foundation.

#### Coordinate.java:
The coordinate class represent a point in the coordinate system. It's have been implemented i.e. in the previous function class. It's nothing special about the class, but it's one of the most important classes in the project, because without it no function is able to be shown in the coordinate system.

#### Line.java:
This class has the attributes of a function graph. Here is the method to calculate the intersection point of two functions and a method to check whether a point lies on a function. Here is the algorithm to calculate the intersection point:
```
    double b1 = func1.getB();
    double b2 = func2.getB();
    double m1 = func1.getM();
    double m2 = func2.getM();
    double cacheB;
    double cacheM;
    double x;
   
    if (b1 < b2) cacheB = b2 - b1;
    else cacheB = b1 - b2;
    if (m1 < m2) cacheM = m2 - m1;
    else cacheM = m1 - m2;

    double temp = cacheM;
    cacheM /= cacheM;
    cacheB /= temp;

    x = cacheB;
    double y = m1 * x + b1;
    return new Coordinate(x, y);
```
And here is an example how I got to this.
Let's take two simple linear functions:<br> ```f(x) = 2x + 3``` and ``` g(x) = -3x + 9 ```.
Then we solve the equation with an equivalence transformation:
```
2x + 3 = -3x + 9 |+3x
2x + 3x + 3 = 9  |-3
2x + 3x = 9 - 3 => m1 - m2 = b1 - b2
```
This process is the same like in the code above.

### The general structure
```
├───src
│   ├───main
│   │   ├───java
│   │   │   └───com
│   │   │       └───functionvisualizer
│   │   │           ├───attributs
│   │   │           └───functions
│   │   └───resources
│   │       └───com.functionvisualizer
│   │           └───img
│___└───test
       └───java
           └───com
               └───functionvisualizer
                   └───attributs
```
Check the project on <a href="https://github.com/j-schall/FunctionVisualizer">Github</a>