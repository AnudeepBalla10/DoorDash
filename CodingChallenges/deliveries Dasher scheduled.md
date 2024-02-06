# DoorDash Deliveries ,

At DoorDash, many deliveries are scheduled well in advance. To improve our assignment rate, we want to enable dashers to claim these scheduled deliveries early. However, we noticed that certain dashers perform better, and want to reward them with a better selection. As a simple solution, we will introduce open windows for when deliveries will appear for a particular dasher. Below are the following requirements.
1. Deliveries scheduled two days or further into the future should never be available
2. High tier dashers can see all of next day deliveries if the current time is 18:00 or later
3. All dashers can see all of next day deliveries if the current time is 19:00 or later
4. All dashers can see same day deliveries anytime

```java
import java.time.Duration;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;

class Delivery {
    String id;
    LocalDateTime pickupTime;
    String storeId;

    public Delivery(String id, LocalDateTime pickupTime, String storeId) {
        this.id = id;
        this.pickupTime = pickupTime;
        this.storeId = storeId;
    }
}

class Dasher {
    String id;
    String tier;

    public Dasher(String id, String tier) {
        this.id = id;
        this.tier = tier;
    }
}

class DoorDashScheduler {

    public static List<Delivery> getAvailableDeliveries(Dasher dasher, List<Delivery> deliveries, LocalDateTime currentTime) {
        List<Delivery> availableDeliveries = new ArrayList<>();

        for (Delivery delivery : deliveries) {
            if (isAvailableDeliveryForDasher(currentTime, delivery.pickupTime, dasher.tier)) {
                availableDeliveries.add(delivery);
            }
        }

        return availableDeliveries;
    }

    private static boolean isAvailableDeliveryForDasher(LocalDateTime currentTime, LocalDateTime deliveryTime, String dasherTier) {
        long daysDifference = Duration.between(currentTime, deliveryTime).toDays();
        long hoursDifference = Duration.between(currentTime, deliveryTime).toHours();
        //edge case of not showing deliveries that are in past
        if (hoursDifference < 0) {
            return false;}
        if (daysDifference >= 2) {
            return false;  // Deliveries scheduled two days or further into the future should not be available
        } else if (daysDifference == 0) {
            return true;   // Same day deliveries are always available
        } else if (daysDifference == 1) {
            if ("high".equals(dasherTier) && currentTime.getHour() >= 18) {
                return true;  // High-tier dashers can see next day deliveries at 18:00 or later
            } else {
                return currentTime.getHour() >= 19;  // All dashers can see next day deliveries at 19:00 or later
            }
        }

        return false;
    }

    public static void main(String[] args) {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");

        Dasher dasher = new Dasher("dasher", "low");
        List<Delivery> deliveries = new ArrayList<>();

        deliveries.add(new Delivery("1", LocalDateTime.now().plusHours(10), "store_1"));
        deliveries.add(new Delivery("2", LocalDateTime.now().plusDays(1), "store_1"));
        deliveries.add(new Delivery("3", LocalDateTime.now().plusDays(2), "store_1"));

        LocalDateTime current_time = LocalDateTime.now().plusHours(15); // Set current_time to the current system time

        List<Delivery> available_deliveries = getAvailableDeliveries(dasher, deliveries, current_time);
        for (Delivery d : available_deliveries) {
            System.out.println(d.id);
        }
    }
}
```

## follow Up

```java
 boolean isPreferredDasher = storePreferences.stream()
                    .anyMatch(preference -> preference.storeId.equals(delivery.storeId) && preference.dasherId.equals(dasher.id));
            if (isPreferredDasher && currentTime.getHour() >= 17) {
                return true;  // Preferred dashers can see next day deliveries at 17:00 or later

```
