[image1]: ./img/example_output.png "Example Output"
[image2]: ./img/sample_world.png "Sample World"


# Landmark Detection & Robot Tracking

Implementing SLAM (Simultaneous Localization and Mapping) for a 2 dimensional world

![Example Output][image1]


# Overview

1. Robot Moving and Sensing
2. Omega and Xi, Constraints
3. Landmark Detection and Tracking


# Robot Moving and Sensing

Localizing a robot in a 2D grid world. The basis for simultaneous localization and mapping (SLAM) is to gather information from a robot's sensors and motions over time, and then use information about measurements and motion to re-construct a map of the world.

## Robot class
```python
class robot:
    def __init__(self, world_size = 100.0, measurement_range = 30.0,
                 motion_noise = 1.0, measurement_noise = 1.0):
        """creates a robot with the specified parameters and initializes
           the location (self.x, self.y) to the center of the world"""

        self.measurement_noise = 0.0
        self.world_size = world_size
        self.measurement_range = measurement_range
        self.x = world_size / 2.0
        self.y = world_size / 2.0
        self.motion_noise = motion_noise
        self.measurement_noise = measurement_noise
        self.landmarks = []
        self.num_landmarks = 0
    
    
    # returns a positive, random float
    def rand(self):
        return random.random() * 2.0 - 1.0
    

    def move(self, dx, dy):
        """attempts to move robot by dx, dy. If outside world
           boundary, then the move does nothing and instead returns failure"""

        x = self.x + dx + self.rand() * self.motion_noise
        y = self.y + dy + self.rand() * self.motion_noise
        
        if x < 0.0 or x > self.world_size or y < 0.0 or y > self.world_size:
            return False
        else:
            self.x = x
            self.y = y
            return True


    def sense(self):
        """ This function does not take in any parameters, instead it references internal variables
            (such as self.landamrks) to measure the distance between the robot and any landmarks
            that the robot can see (that are within its measurement range).
            This function returns a list of landmark indices, and the measured distances (dx, dy)
            between the robot's position and said landmarks.
            This function should account for measurement_noise and measurement_range.
            One item in the returned list should be in the form: [landmark_index, dx, dy].
            """
           
        measurements = []

        for i in range(len(self.landmarks)):
            dx = (self.landmarks[i][0] - self.x) + self.rand() * self.measurement_noise
            dy = (self.landmarks[i][1] - self.y) + self.rand() * self.measurement_noise
            
            if np.sqrt(dx*dx + dy*dy) <= self.measurement_range:
                measurements.append([i, dx, dy])
        
        return measurements


    def make_landmarks(self, num_landmarks):
        """make random landmarks located in the world"""

        self.landmarks = []
        for i in range(num_landmarks):
            self.landmarks.append([round(random.random() * self.world_size),
                                   round(random.random() * self.world_size)])
        self.num_landmarks = num_landmarks


    # called when print(robot) is called; prints the robot's location
    def __repr__(self):
        return 'Robot: [x=%.5f y=%.5f]'  % (self.x, self.y)

```

## Define a world and a robot

```python
world_size         = 10.0    # size of world (square)
measurement_range  = 5.0     # range at which we can sense landmarks
motion_noise       = 0.2     # noise in robot motion
measurement_noise  = 0.2     # noise in the measurements

# instantiate a robot
r = robot(world_size, measurement_range, motion_noise, measurement_noise)
```

## Visualizing the world

```python
# import helper function
from helpers import display_world

# define figure size
plt.rcParams["figure.figsize"] = (5,5)

# call display_world and display the robot in it's grid world
print(r)
display_world(int(world_size), [r.x, r.y])
```

![Sample World][image2]

# Omega and Xi, Constraints

# Landmark Detection and Tracking

# Reference
