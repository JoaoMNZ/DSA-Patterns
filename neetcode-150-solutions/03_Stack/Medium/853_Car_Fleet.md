# <a href="https://leetcode.com/problems/car-fleet/" target="_blank">853. Car Fleet</a>

### Problem Statement
There are `n` cars traveling to a destination at `target` miles. You are given two integer arrays, `position` and `speed`, where `position[i]` and `speed[i]` are the position and speed of the `i`th car.

A car cannot pass another car, but it can catch up and travel behind it at the same speed. A group of cars traveling together is called a car fleet. Return the number of car fleets that will arrive at the destination.

### Approach
The core of this problem is to determine if a car starting further behind can catch up to the car(s) in front of it. An efficient way to do this is to process the cars in order of their starting position.

1.  **Sort:** First, we sort the cars based on their starting position in **descending order** (from closest to the target to furthest away). This allows us to process the car that is closest to the finish line first.

2.  **Iterate and Check:** We loop through this sorted list of cars. We'll use a variable, `currentFleetTime`, to keep track of the arrival time of the current "lead" fleet (the one closest to the target).

3.  **Logic:** For each car, we calculate its own time to reach the target.
    -   If the car's arrival time is **greater than** `currentFleetTime`, it means this car is slower and will not catch up to the fleet ahead of it. Therefore, it forms a **new fleet**. We then update `currentFleetTime` to this car's arrival time and increment our fleet count.
    -   If the car's arrival time is **less than or equal to** `currentFleetTime`, it means the car is fast enough to catch up and merge with the fleet ahead. It does not form a new fleet, so we do nothing.

After checking all the cars, our counter will hold the total number of distinct fleets.

### Implementation
```java
import java.util.Arrays;

class Solution {
    private static class Car implements Comparable<Car> {
        private final double position;
        private final double speed;

        public Car(double position, double speed) {
            this.position = position;
            this.speed = speed;
        }

        public double calculateTimeToTarget(int target){
            return (target - this.position) / this.speed;
        }

        @Override
        public int compareTo(Car other) {
            return Double.compare(other.position, this.position);
        }
    }

    public int carFleet(int target, int[] position, int[] speed) {
        int n = position.length;
        
        Car[] cars = new Car[n];
        for (int i = 0; i < n; i++) {
            cars[i] = new Car((double) position[i], (double) speed[i]);
        }
        Arrays.sort(cars);

        double currentFleetTime = 0.0;
        int fleets = 0;
        for (int i = 0; i < n; i++) {
            Car car = cars[i];
            double timeToTarget = car.calculateTimeToTarget(target);
            if (timeToTarget > currentFleetTime) {
                fleets++;
                currentFleetTime = timeToTarget;
            }
        }

        return fleets;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of cars.

-   **Time Complexity:** O(n log n)
    -   The main performance cost is sorting the array of `n` cars, which takes `O(n log n)` time. The subsequent loop to iterate through the cars takes O(n) time. Therefore, the sorting step dominates the complexity.

-   **Space Complexity:** O(n)
    -   We need to create an array of `n` `Car` objects to store the combined position and speed data before sorting. The space required grows linearly with the number of cars.
