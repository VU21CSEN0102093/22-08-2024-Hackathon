# 22-08-2024-Hackathon

import java.util.*;

class ChargingStation {
    private String name;
    private String location;
    private int totalSlots;
    private int availableSlots;

    public ChargingStation(String name, String location, int totalSlots) {
        this.name = name;
        this.location = location;
        this.totalSlots = totalSlots;
        this.availableSlots = totalSlots; // Initially, all slots are available
    }

    public String getName() {
        return name;
    }

    public String getLocation() {
        return location;
    }

    public int getAvailableSlots() {
        return availableSlots;
    }

    public boolean bookSlot() {
        if (availableSlots > 0) {
            availableSlots--;
            return true;
        } else {
            return false;
        }
    }

    public void releaseSlot() {
        if (availableSlots < totalSlots) {
            availableSlots++;
        }
    }

    @Override
    public String toString() {
        return name + " (" + location + ") - Available Slots: " + availableSlots + "/" + totalSlots;
    }
}

class EVChargingSystem {
    private List<ChargingStation> stations;

    public EVChargingSystem() {
        stations = new ArrayList<>();
    }

    public void addStation(ChargingStation station) {
        stations.add(station);
    }

    public void findNearbyStations(String location) {
        System.out.println("Nearby Charging Stations at " + location + ":");
        for (ChargingStation station : stations) {
            if (station.getLocation().equalsIgnoreCase(location)) {
                System.out.println(station);
            }
        }
    }

    public void bookSlotAtStation(String stationName) {
        for (ChargingStation station : stations) {
            if (station.getName().equalsIgnoreCase(stationName)) {
                if (station.bookSlot()) {
                    System.out.println("Slot booked successfully at " + stationName + ".");
                } else {
                    System.out.println("No available slots at " + stationName + ".");
                }
                return;
            }
        }
        System.out.println("Charging station not found.");
    }

    public void releaseSlotAtStation(String stationName) {
        for (ChargingStation station : stations) {
            if (station.getName().equalsIgnoreCase(stationName)) {
                station.releaseSlot();
                System.out.println("Slot released at " + stationName + ".");
                return;
            }
        }
        System.out.println("Charging station not found.");
    }
}

public class EVChargingSystemApp {
    public static void main(String[] args) {
        EVChargingSystem system = new EVChargingSystem();

        // Add some charging stations
        system.addStation(new ChargingStation("GreenCharge", "Downtown", 10));
        system.addStation(new ChargingStation("QuickCharge", "Downtown", 8));
        system.addStation(new ChargingStation("PowerUp", "Uptown", 5));

        // Find nearby stations at Downtown
        system.findNearbyStations("Downtown");

        // Book a slot at GreenCharge
        system.bookSlotAtStation("GreenCharge");

        // Try booking another slot at a non-existent station
        system.bookSlotAtStation("FastCharge");

        // Release a slot at GreenCharge
        system.releaseSlotAtStation("GreenCharge");

        // Check availability again
        system.findNearbyStations("Downtown");
    }
}

