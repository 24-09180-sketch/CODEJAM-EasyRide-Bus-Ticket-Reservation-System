# CODEJAM-EasyRide-Bus-Ticket-Reservation-System
This system is a is a console-based system that is designed to simplify bus ticket booking and management by allowing users or passengers to view available destinations, check seat availability, book tickets, and cancel reservations using a booking ID. The goal of this system is to provide a convenient and organized way to handle bus reservations. ⋆˙⟡

// ──── ୨୧ ──── Group 4 >⩊<   ──── ୨୧ ────
// CodeJAM EasyRide: Bus Ticket Reservation System

import java.util.ArrayList;
import java.util.Scanner;

// ===================================================
//  ☆ ABSTRACTION ☆
//  This abstract class acts as a template for all
//  destination types. We only show the important
//  methods and hide the details. 
// ===================================================
abstract class Destination {
    public abstract void showDestinations();        // method to display destinations
    public abstract double getFare(int choice);     // method to get fare
    public abstract String getName(int choice);     // method to get name of destination
}

// ===================================================
//  𖹭 INHERITANCE + POLYMORPHISM 𖹭
//  BusDestination inherits from Destination and
//  provides its own version of the abstract methods.
// ===================================================
class BusDestination extends Destination {

    // Sample data for places and fares
    private final String[] places = {"Manila", "Baguio", "Cebu", "Davao", "Vigan"};
    private final double[] fares  = {600, 900, 1200, 1800, 1500};

    @Override
    public void showDestinations() {
        System.out.println("Available Destinations and Fares:");
        for (int i = 0; i < places.length; i++) {
            System.out.printf("%d. %s - ₱%.2f%n", i + 1, places[i], fares[i]);
        }
    }

    @Override
    public double getFare(int choice) {
        return fares[choice - 1];  // get fare based on user choice
    }

    @Override
    public String getName(int choice) {
        return places[choice - 1]; // get destination name
    }
}

// ===================================================
//  ⋆.𐙚 ̊ ENCAPSULATION⋆.𐙚 ̊
//  Booking class keeps its data private so they cannot
//  be changed directly. Only getters are allowed.
// ===================================================
class Booking {
    private String bookingID;
    private String passengerName;
    private String destination;
    private int seatsBooked;
    private double totalFare;

    // constructor to create a booking
    public Booking(String bookingID, String passengerName, String destination, int seatsBooked, double totalFare) {
        this.bookingID = bookingID;
        this.passengerName = passengerName;
        this.destination = destination;
        this.seatsBooked = seatsBooked;
        this.totalFare = totalFare;
    }

    // Getter methods only (no direct editing allowed)
    public String getBookingID() { return bookingID; }
    public String getPassengerName() { return passengerName; }
    public String getDestination() { return destination; }
    public int getSeatsBooked() { return seatsBooked; }
    public double getTotalFare() { return totalFare; }
}

// ===================================================
//  ⋆.𐙚 ̊ENCAPSULATION (Bus class)⋆.𐙚 ̊
//  This class handles seat management. Seats cannot be
//  directly edited because they are private.
// ===================================================
class Bus {
    private int totalSeats = 20;
    private int remainingSeats = 20;

    public int getRemainingSeats() { return remainingSeats; }
    public int getTotalSeats() { return totalSeats; }

    // Books seats only if enough seats are available
    public boolean bookSeats(int seats) {
        if (seats <= remainingSeats) {
            remainingSeats -= seats;
            return true;
        }
        return false;
    }

    // Adds back seats if booking is cancelled
    public void cancelSeats(int seats) {
        remainingSeats += seats;
    }

    public void viewSeatAvailability() {
        System.out.println("\n--- SEAT AVAILABILITY ---");
        System.out.println("Total Seats: " + totalSeats);
        System.out.println("Seats Remaining: " + remainingSeats);
        System.out.println("Status: " + (remainingSeats > 0 ? "Seats available" : "Fully booked"));
    }
}

// ===================================================
//  𖹭.ᐟ MAIN PROGRAM𖹭.ᐟ
//  Handles everything: menu, booking, cancel, admin.
// ===================================================
public class Main {
    private static Scanner sc = new Scanner(System.in);

    private static Destination dest = new BusDestination(); // polymorphism example
    private static Bus bus = new Bus();

    // List to store all bookings
    private static ArrayList<Booking> bookings = new ArrayList<>();

    private static int bookingCounter = 1;

    // Saves last user’s name for personalized exit message
    private static String lastUserName = "";

    public static void main(String[] args) throws Exception {

        // For peso sign support
        System.setOut(new java.io.PrintStream(System.out, true, "UTF-8"));

        int choice;

        while (true) {
            System.out.println("\n─────── == ༘ 🚍⋆｡˚ BUS TICKET RESERVATION SYSTEM˚ ༘ 🚍⋆｡˚ == ───────");
            System.out.println("Hop on and enjoy your trip! Sit back and relax for a fun and safe journey!\n ");
            System.out.println("[1] View Destinations");
            System.out.println("[2] View Seat Availability");
            System.out.println("[3] Book Ticket");
            System.out.println("[4] Cancel Booking (use Booking ID)");
            System.out.println("[5] Admin Login (view all bookings)");
            System.out.println("[6] Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1: viewDestinations(); break;
                case 2: bus.viewSeatAvailability(); break;
                case 3: bookTicket(); break;
                case 4: cancelBooking(); break;
                case 5: adminLogin(); break;
                case 6:
                    // Shows name if user booked something
                    if (lastUserName.isEmpty()) {
                        System.out.println("Thank you for using the system! God bless you! ♡");
                    } else {
                        System.out.println("Thank you for using the system, " + lastUserName +"! God bless you! ♡");
                    }
                    return;
                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }

    private static void viewDestinations() {
        dest.showDestinations(); 
    }

    // ====================================
    //  𐔌՞. .՞ Booking a Ticket 𐔌՞. .՞𐦯
    // ====================================
    private static void bookTicket() {
        sc.nextLine(); // clears input buffer
        System.out.print("\nEnter passenger name: ");
        String name = sc.nextLine();
        lastUserName = name; // save last user name

        dest.showDestinations();
        System.out.print("Choose destination (1-5): ");
        int choice = sc.nextInt();

        System.out.print("How many seats to book? ");
        int seats = sc.nextInt();

        // checks if enough seats are available
        if (!bus.bookSeats(seats)) {
            System.out.println("Not enough seats available.");
            return;
        }

        // auto-generate booking ID
        String bookingID = String.format("B%03d", bookingCounter++);
        double fare = dest.getFare(choice);
        double total = fare * seats;

        Booking b = new Booking(bookingID, name, dest.getName(choice), seats, total);
        bookings.add(b);

        // booking confirmation
        System.out.println("\n--- BOOKING CONFIRMATION ---");
        System.out.println("Booking ID: " + bookingID);
        System.out.println("Destination: " + dest.getName(choice));
        System.out.println("Seats Booked: " + seats);
        System.out.printf("Total Fare: ₱%.2f%n", total);
        System.out.println("Seats Remaining: " + bus.getRemainingSeats());
    }

    // ===============================
    //  ⋆.˚ CANCEL BOOKING USING ID ⋆.˚
    // ===============================
    private static void cancelBooking() {
        sc.nextLine();
        System.out.print("Enter your Booking ID to cancel: ");
        String id = sc.nextLine();

        for (Booking b : bookings) {
            if (b.getBookingID().equals(id)) {
                bus.cancelSeats(b.getSeatsBooked());
                bookings.remove(b);
                System.out.println("Booking cancelled successfully.");
                return;
            }
        }
        System.out.println("Booking ID not found.");
    }

    // ===============================
    //        ʚɞ  ADMIN LOGIN ʚɞ
    // ===============================
    private static void adminLogin() {
        int attempts = 3;
        sc.nextLine(); // clear buffer

        while (attempts > 0) {
            System.out.print("Enter admin password: ");
            String pass = sc.nextLine();

            if (pass.equals("CODEJAM")) {
                System.out.println("Access granted! >ᴗ<");
                break;
            } else {
                attempts--;
                System.out.println("Incorrect password. Attempts left: " + attempts);

                if (attempts == 0) {
                    System.out.println("Too many failed attempts. Returning to main menu...");
                    return;
                }
            }
        }

        int choice;
        while (true) {
            System.out.println("\n--- ADMIN MENU ---");
            System.out.println("1. View All Bookings");
            System.out.println("2. View Seat Availability");
            System.out.println("3. Remove a Booking");
            System.out.println("4. Back to Main Menu");
            System.out.print("Enter choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1: viewAllBookings(); break;
                case 2: bus.viewSeatAvailability(); break;
                case 3: removeBooking(); break;
                case 4: return;
                default: System.out.println("Invalid choice.");
            }
        }
    }

    private static void viewAllBookings() {
        System.out.println("\n--- ALL BOOKINGS ---");
        for (Booking b : bookings) {
            System.out.println(
                b.getBookingID() + " | " +
                b.getPassengerName() + " | " +
                b.getDestination() + " | Seats: " +
                b.getSeatsBooked() + " | Total: ₱" +
                String.format("%.2f", b.getTotalFare())
            );
        }
    }

    private static void removeBooking() {
        sc.nextLine();
        System.out.print("Enter Booking ID to remove: ");
        String id = sc.nextLine();

        for (Booking b : bookings) {
            if (b.getBookingID().equals(id)) {
                bus.cancelSeats(b.getSeatsBooked());
                bookings.remove(b);
                System.out.println("Booking removed successfully.");
                return;
            }
        }
        System.out.println("Booking ID not found.");
    }
}
