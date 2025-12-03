# CODEJAM-EasyRide-Bus-Ticket-Reservation-System
This system is a is a console-based system that is designed to simplify bus ticket booking and management by allowing users or passengers to view available destinations, check seat availability, book tickets, and cancel reservations using a booking ID. The goal of this system is to provide a convenient and organized way to handle bus reservations. 

import java.util.ArrayList;
import java.util.Scanner;

// ===== ABSTRACTION =====
// Abstract class representing generic destinations.
// It hides details and only exposes essential methods.
abstract class Destination {
    public abstract void showDestinations();  // Abstract method
    public abstract double getFare(int choice);  // Abstract method
    public abstract String getName(int choice);  // Abstract method
}

// ===== INHERITANCE + POLYMORPHISM =====
// BusDestination inherits from Destination
// and provides its own implementation.
class BusDestination extends Destination {

    // Concrete data representing places and fares
    private final String[] places = {"Manila", "Baguio", "Cebu", "Davao", "Vigan"};
    private final double[] fares = {600, 900, 1200, 1800, 1500};

    @Override
    public void showDestinations() {
        System.out.println("Available Destinations and Fares:");
        for (int i = 0; i < places.length; i++) {
            // Peso sign included
            System.out.printf("%d. %s - ₱%.2f%n", i + 1, places[i], fares[i]);
        }
    }

    @Override
    public double getFare(int choice) {
        return fares[choice - 1];
    }

    @Override
    public String getName(int choice) {
        return places[choice - 1];
    }
}

// ===== ENCAPSULATION =====
// Private attributes with getters.
// Protects data and prevents direct access.
class Booking {
    private String bookingID;
    private String passengerName;
    private String destination;
    private int seatsBooked;
    private double totalFare;

    public Booking(String bookingID, String passengerName, String destination, int seatsBooked, double totalFare) {
        this.bookingID = bookingID;
        this.passengerName = passengerName;
        this.destination = destination;
        this.seatsBooked = seatsBooked;
        this.totalFare = totalFare;
    }

    // Getters
    public String getBookingID() { return bookingID; }
    public String getPassengerName() { return passengerName; }
    public String getDestination() { return destination; }
    public int getSeatsBooked() { return seatsBooked; }
    public double getTotalFare() { return totalFare; }
}

// ===== ENCAPSULATION =====
// Bus class protects seat data.
class Bus {
    private int totalSeats = 20;
    private int remainingSeats = 20;

    public int getRemainingSeats() { return remainingSeats; }
    public int getTotalSeats() { return totalSeats; }

    // Only way to modify seats safely
    public boolean bookSeats(int seats) {
        if (seats <= remainingSeats) {
            remainingSeats -= seats;
            return true;
        }
        return false;
    }

    // Add seats back when cancelling
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

// ===== MAIN PROGRAM =====
public class Main {
    private static Scanner sc = new Scanner(System.in);

    private static Destination dest = new BusDestination();
    private static Bus bus = new Bus();

    // ===== COMPOSITION =====
    private static ArrayList<Booking> bookings = new ArrayList<>();

    private static int bookingCounter = 1;

    // ===== NEW: store last passenger name =====
    private static String lastUserName = "";

    public static void main(String[] args) throws Exception {

        // PESO SIGN (UTF-8 OUTPUT)
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

    private static void bookTicket() {
        sc.nextLine(); // clear buffer
        System.out.print("Enter passenger name: ");
        String name = sc.nextLine();
        lastUserName = name; // store the last passenger name

        dest.showDestinations();
        System.out.print("Choose destination (1-5): ");
        int choice = sc.nextInt();

        System.out.print("How many seats to book? ");
        int seats = sc.nextInt();

        if (!bus.bookSeats(seats)) {
            System.out.println("Not enough seats available.");
            return;
        }

        String bookingID = String.format("B%03d", bookingCounter++);
        double fare = dest.getFare(choice);
        double total = fare * seats;

        Booking b = new Booking(bookingID, name, dest.getName(choice), seats, total);
        bookings.add(b);

        System.out.println("\n--- BOOKING CONFIRMATION ---");
        System.out.println("Booking ID: " + bookingID);
        System.out.println("Destination: " + dest.getName(choice));
        System.out.println("Seats Booked: " + seats);
        System.out.printf("Total Fare: ₱%.2f%n", total);
        System.out.println("Seats Remaining: " + bus.getRemainingSeats());
    }

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
                System.out.println("Incorrect password. ˙◠˙ Attempts left: " + attempts);

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
