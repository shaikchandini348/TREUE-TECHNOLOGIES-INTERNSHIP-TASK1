# TREUE-TECHNOLOGIES-INTERNSHIP-TASK1
ONLINE PARKING SYSTEM :
Develop an online parking system where users can search for available parking spots, book them. The system should provide real-time updates on parking availability and allow users to manage their bookings. Implement features like user registration, parking spot search, booking confirmation

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

class User {
    private String username;
    private String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }
}

class ParkingSpot {
    private int spotId;
    private String location;
    private boolean isAvailable;

    public ParkingSpot(int spotId, String location) {
        this.spotId = spotId;
        this.location = location;
        this.isAvailable = true;
    }

    public int getSpotId() {
        return spotId;
    }

    public String getLocation() {
        return location;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void bookSpot() {
        isAvailable = false;
    }

    public void freeSpot() {
        isAvailable = true;
    }
}

public class ParkingSystem {
    private Map<String, User> users = new HashMap<>();
    private List<ParkingSpot> parkingSpots = new ArrayList<>();
    private Map<String, ParkingSpot> bookings = new HashMap<>();
    private int spotCounter = 1;

    public void registerUser(String username, String password) {
        User user = new User(username, password);
        users.put(username, user);
    }

    public void addParkingSpot(String location) {
        ParkingSpot spot = new ParkingSpot(spotCounter++, location);
        parkingSpots.add(spot);
    }

    public void searchParkingSpots() {
        System.out.println("Available Parking Spots:");
        for (ParkingSpot spot : parkingSpots) {
            if (spot.isAvailable()) {
                System.out.println("Spot ID: " + spot.getSpotId() + ", Location: " + spot.getLocation());
            }
        }
    }

    public void bookParkingSpot(String username, int spotId) {
        ParkingSpot spot = bookings.get(username);
        if (spot != null) {
            System.out.println("You have already booked a spot.");
        } else {
            for (ParkingSpot availableSpot : parkingSpots) {
                if (availableSpot.getSpotId() == spotId && availableSpot.isAvailable()) {
                    availableSpot.bookSpot();
                    bookings.put(username, availableSpot);
                    System.out.println("Booking confirmed. Spot ID: " + spotId + ", Location: " + availableSpot.getLocation());
                    return;
                }
            }
            System.out.println("Spot with ID " + spotId + " is not available.");
        }
    }

    public static void main(String[] args) {
        ParkingSystem parkingSystem = new ParkingSystem();
        Scanner scanner = new Scanner(System.in);

        // User Registration
        System.out.print("Enter username: ");
        String username = scanner.nextLine();
        System.out.print("Enter password: ");
        String password = scanner.nextLine();
        parkingSystem.registerUser(username, password);

        // Adding Parking Spots
        parkingSystem.addParkingSpot("Parking Lot A");
        parkingSystem.addParkingSpot("Parking Lot B");
        parkingSystem.addParkingSpot("Parking Lot C");

        // User Login (In a real system, you'd implement proper authentication)
        System.out.print("Enter username: ");
        String loggedInUser = scanner.nextLine();

        // Search for Available Parking Spots
        parkingSystem.searchParkingSpots();

        // Booking a Spot
        System.out.print("Enter spot ID to book: ");
        int spotId = scanner.nextInt();
        parkingSystem.bookParkingSpot(loggedInUser, spotId);

        // Close the scanner
        scanner.close();
    }
}
