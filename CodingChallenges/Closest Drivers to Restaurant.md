# Closest Drivers to Restaurant

Question
Given a restaurant geolocation ( longitude / latitude), find 3 closest Dashers (drivers) near the restaurant who can be assigned for delivery, ordered by their distance from the restaurant. In case 2 Dashers are equidistant from the restraunt, use Dasher rating as tie breaker.

Each Dasher has 3 properties:

Dasher ID
Last known location [x,y]
Rating (0 - 100). Higher the better
Assume you have a method GetDashers() which returns a list of all Dashers.

Input
Restaurant Location

Output
List of 3 nearest Dasher IDs. Example: [11, 14, 17]

Assume GetDashers() returns a List<Dasher> where Dasher is represented by:
```
class Dasher {
    long id;
    Location lastLocation
    int rating;
    
    public Dasher(long id, Location lastLocation, int rating) {
        this.id = id;
        this.lastLocation = lastLocation;
        this.rating = rating;
    }
}
```
and Location is represented by:
```
class Location {
    double longitude;
    double lattitude;
    
    Location(double longitude, double lattitude) {
        this.longitude = longitude;
        this.lattitude = lattitude;
    }
}
```


# Solution:
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

class Location {
    double longitude;
    double latitude;

    Location(double longitude, double latitude) {
        this.longitude = longitude;
        this.latitude = latitude;
    }
}

class Dasher {
    long id;
    Location lastLocation;
    int rating;

    public Dasher(long id, Location lastLocation, int rating) {
        this.id = id;
        this.lastLocation = lastLocation;
        this.rating = rating;
    }
}

public class DasherFinder {

    public static List<Long> findNearestDashers(Location restaurantLocation) {
        List<Dasher> dashers = getDashers(); // Assume this method returns a list of all Dashers

        // Calculate distances and sort Dashers by distance and rating
        Collections.sort(dashers, Comparator.comparingDouble(dasher ->
                calculateDistance(restaurantLocation, dasher.lastLocation) +
                        dasher.rating / 100.0));

        // Select top 3 closest Dashers
        List<Long> result = new ArrayList<>();
        for (int i = 0; i < Math.min(3, dashers.size()); i++) {
            result.add(dashers.get(i).id);
        }

        return result;
    }

    private static double calculateDistance(Location location1, Location location2) {
        // Assume you have a method to calculate the distance between two locations
        // This can be implemented using the Haversine formula or other distance formulas
        // For simplicity, let's assume a simple Euclidean distance here
        return Math.sqrt(Math.pow(location1.longitude - location2.longitude, 2) +
                Math.pow(location1.latitude - location2.latitude, 2));
    }

    // Assume this method retrieves a list of all Dashers
    private static List<Dasher> getDashers() {
        // Implementation of GetDashers() method
        // ...
    }

    public static void main(String[] args) {
        Location restaurantLocation = new Location(12.971598, 77.594562); // Example restaurant location
        List<Long> nearestDashers = findNearestDashers(restaurantLocation);
        System.out.println("Nearest Dashers: " + nearestDashers);
    }
}

```
