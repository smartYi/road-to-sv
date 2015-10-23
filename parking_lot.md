# Parking Lot

Design a parking lot using object-oriented principles

## Solution

Brief Design

+ enum VehicleSize {Moto, Compact, Large}
+ abstract class Vehicle
+ class Bus extends Vehicle
+ class Car extends Vehicle
+ class Moto extends Vehicle
+ class ParkingLot
    + private Level[] levels
    + private final int NUM_LEVELS = 5;
    + public boolean parkVehicle(Vehicle vehicle) {...}
+ class Level
    + private int floor;
    + private ParkingSpot[] spots;
    + private int available Spots = 0;
    + public boolean parkVehicle(Vehicle vehicle) {...}
    + public void spotFreed() { availableSpots++; }
+ class ParkingSpot
    + private Vehicle vehicle;
    + private VehicleSize spotSize;
    + private int number;
    + private Level level;
    + public boolean isAvailable() { return vehicel == null; }
    + public boolean park(Vehicle v) {...}
    + public remove Vehicle() {...}