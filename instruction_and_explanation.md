## Some Results
![](https://lh3.googleusercontent.com/-K1BcXaqtqMc/W3EBvIxSFtI/AAAAAAAAbI8/U_F4BtWBH08ahUi4dcXgDRM3dKMVbLTnACL0BGAs/w1060-d-h328-n-rw/Screen%2BShot%2B2018-08-13%2Bat%2B10.57.17.png)

## Instruction
### Installation
Make sure that you're using `ruby 2.5.1`
```
gem install bundler
```

Installing gems for testing and debugging.
```
bundle install
```

### Run tests
Run all test cases.

```
bundle exec rspec
```
### Run main program
Edit data/input.txt file if you want to change the input. (this file contains sample input provided as default).
```
AB5, BC4, CD8, DC8, DE6, AD5, CE2, EB3, AE7
```

```
bundle exec rake
```

## Explanation
### Solution

Because I was excited at first problem so that I chose it. 

According to the requirement, we have 10 outputs corresponding 10 small cases.
#### Case#1 to Case#5
After get the route as an argument, for example `case#1` 'A-B-C', we split this route to smaller, direct route like 'A-B' and 'B-C'. Then, we look for each route in the input to get the distance. If the result not found, the distance is nil, we show 'NO SUCH ROUTE'.
```
def distance
  exists? ? total_distance : 'NO SUCH ROUTE'
end
def valid?; exists?; end
def exists?
  distances.all? { |distance| distance.nil? == false }
end
```

#### Case#6 and Case#7
As we all know about the algorithms for finding routes, I developed a recursive function to find all of routes. 
```
module RouteFinder
  def find_routes(starting_point, max, routes = [], stops_order = [])
  end
end
```
The results depend on the number of stops we want to have in these routes.

#### Case#8 and Case#9
We can find all of routes from a starting point to a ending point as above. Now we find the route with minimum of distance. It leads to find a min value.

#### Case#10
This is the most complex case. At first glance, we think we will find all of routes from C to C and then calculate the distance to see if it's less than 30. However, there are a lot of round trips. For example

Input: 'AB2', 'BC3', 'CB4', 'CD5', 'DC6'

With above input we can have this kind of trip: ABCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCD. 

This is the route we found from `A` to `D`.

In fact, we will have a lot of small round strips at the middle of route.

It's not only `BC`: A**BCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBCBC**D

But we also have: AB**CDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCDCD**CD.

My solution is: I find the direct route with shortest direct distance. Because we want to find the route with maximum stops (in case of round trips) but its distance is less than a number like 30.

=> The maximum stops = number / mininum distance of direct route.

Regarding to the sample input provided of this problem: AB5, BC4, CD8, DC8, DE6, AD5, CE2, EB3, AE7.

The route with minimum distance is CE2. CE has distance is 2 => `max stops = 30/2 = 15`.

It leads to solve the Case#6

I developed three main classes and two modules. 

Classes: 
- Route: this class is the route between any two towns. For example, 'AC', 'ABC', 'BCED'. As a route, it has starting point (starting town), ending point, distance and stops. It has two public behaviors, `distance` and `valid?`, in order to calculate the distance of this route, from starting point and ending point, and check if route is valid - having that kind of route.
- TwoPointsRoute: this is the class which inherits `Route`. Because this class is a `Route` but it's the direct route between two points (towns). It also provides the functions needed to query data from memory as class methods.
- Trip: according to the requirement, we have to deal with trips. Doing a bunch of things to calculate the number of trips, distance, finding out the shortest trip, based on the number of stops, starting and ending point. `Trip` class inherits `Route` because a trip is also a `Route` but it has extra behaviors.

Modules:
- RouteFinder: providing indepedent methods to find foutes as its name.
- RouteProcessor: providing independent methods to deal with routes as its name.

### Easy to maintain
In order to decouple regular changeable code from less changeable code, we split Trip's independent methods into two modules `RouteFinder` and `RouteProcessor`. Each module has single responsibility as it's name. Trip inherits Route to reuse the attributes and methods. Logically, Trip is a Route. TwoPointsRoute is same. 

Code is clean:

- Each class has enough public methods to use for the requirement. The private methods are the changeable methods but public methods aren't or they rarely change.
- Each method has single responsibility and the method's name is readable and understandable.
- Each class and method is short enough. 
- The Unit Tests and Integration test provided with 100% of coverage to make our code reliable.

### Easy to use
- The responsibility of each class and module is clear enough to know.
- Each Route and Trip we can provide any starting point and ending point (towns) and involving it's behaviors to get the results.

### Easy to evolve
- In future, if we want to add extra behaviors to Trip, just add extra public methods, including modules if necessary.
- We can reuse any behaviors. 
- I made classes, modules and methods independent as much as possible so that make it easy to make changes with less cost of time. 

