         ✩°｡🧸𓏲⋆.🧺𖦹 ₊˚     𐔌 .⋮ CODEJAM EasyRide  .ᐟ ֹ ₊ ꒱ ✩°｡🧸𓏲⋆.🧺
         
                       Bus Ticket Reservation System 𖹭
                                     IT-2109
                            Cabral, Jhannea Maica M. 𖹭  J
                             De Castro, Marc James D. 𖹭  M 
                           Ronquillo, Ashley Annabel E. 𖹭  A
                                = CODEJAM ˚.𖥻 ׁ ׅ ! ׁ ׅ 🪷

🚌 CodeJAM EasyRide Bus Ticket Reservation System 🚌

₊˚🖇️✩ OVERVIEW‧ ₊˚📖 

⋮ ⌗ ┆ The CodeJAM EasyRide: Bus Reservation System is a console-based system that is designed to simplify bus ticket booking and management by allowing users or passengers to view available destinations, check seat availability, book tickets, and cancel reservations using a booking ID. The system also includes an admin feature that enables administrators to view all bookings and manage seat allocations. The goal of this system is to provide a convenient and organized way to handle bus reservations which promotes smoother and sustainable transportation services.

CodeJAM EasyRide is a console-based Java application designed to simplify bus ticket booking and management.
It demonstrates Object-Oriented Programming (OOP) concepts such as:

🔒 *Encapsulation <𝟑 .ᐟ

In the EasyRide system, encapsulation is implemented by keeping sensitive data private inside classes such as Booking and Bus. For example, passenger names, booking IDs, booked seats, and total fares are stored as private fields in the Booking class, while the bus seat statuses are private in the Bus class. These values cannot be modified directly from outside the class. Instead, the program uses getters and controlled methods like bookSeats() and cancelSeats() to access or update the data safely. This ensures that bookings and seat availability remain accurate and prevents accidental or unauthorized changes.

🧬 Inheritance✧₊⁺.˚୨ৎ

Inheritance is demonstrated through the relationship between Destination and BusDestination. The BusDestination class extends the abstract Destination class, inheriting its structure and behavior while providing specific implementations for methods like showDestinations(), getFare(), and getName(). This allows code reuse and ensures that all destination types follow a consistent structure without rewriting the same methods for each subclass.

🎭 Polymorphism ☆⋆｡𖦹°‧★

Polymorphism appears in the system when the program interacts with the Destination reference while calling methods implemented in BusDestination. For example, dest.showDestinations() can work with any subclass of Destination, allowing the system to handle different destination types without changing the main code. Similarly, booking or cancellation operations can work uniformly on Booking objects regardless of the specific details of the booking.

🧱 Abstraction₊˚⊹ᰔ

Abstraction is used by hiding complex logic inside methods, making the main program clean and easy to follow. Tasks like checking seat availability, validating seat numbers, and generating booking IDs are encapsulated in dedicated methods such as bookSeats(), cancelSeats(), and getFare(). Users interact only with simple, high-level options in the menu, while the program handles the detailed operations behind the scenes.

🧩 Program Structure (CodeJAM EasyRide) 𐙚⋆°.⋆♡

The system is organized into classes with clear responsibilities:

၄၃ Destination (abstract) → defines a blueprint for destinations

၄၃ BusDestination → provides specific destination data

၄၃ Booking → manages booking details safely

၄၃ Bus → handles seat management and availability

၄၃ Main → controls the user interface and menu flow

This structure keeps the code modular, readable, and easy to maintain, while demonstrating the core OOP principles effectively.

👥 Users Can:
🗺️ View available destinations and fares
🪑 Check seat availability
🎟️ Book tickets with seat selection
❌ Cancel bookings using Booking ID
🔒 Admin functionalities:

♡ View all bookings

♡ Remove any booking

💾 Ticket & Seat Management
📌 All bookings are stored in ArrayList for real-time management
🪑 Booked seats are tracked to prevent double bookings
🎫 Each booking has a unique Booking ID automatically generated

🏗️ Project Structure
1️⃣ Destination (Abstract Class)

📝 Defines a template for all destinations

Methods:

showDestinations() – Display destinations and fares
getFare(choice) – Get fare for selected destination
getName(choice) – Get name of selected destination

2️⃣ BusDestination (Inheritance & Polymorphism)

🚍 Implements Destination

Stores available places and fares

Overrides abstract methods to provide specific destination data

3️⃣ Booking (Encapsulation)

🔒 Stores passenger name, destination, seats, and total fare

Only getter methods provided for secure access

4️⃣ Bus (Encapsulation)

🪑 Manages total seats, remaining seats, and booking logic

Tracks booked seats to prevent double booking

5️⃣ Main Class

💻 Handles user interaction and menus
Allows booking, cancelling, and admin operations
Provides personalized messages to users

₊˚ ┊ Contributions 

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# CODEJAM-EasyRide-Bus-Ticket-Reservation-System
This system is a is a console-based system that is designed to simplify bus ticket booking and management by allowing users or passengers to view available destinations, check seat availability, book tickets, and cancel reservations using a booking ID. The goal of this system is to provide a convenient and organized way to handle bus reservations. ⋆˙⟡

// ──── ୨୧ ──── Group 4 >⩊<   ──── ୨୧ ────
// CodeJAM EasyRide: Bus Ticket Reservation System

// ──── ୨୧ ──── Group 4 >⩊<   ──── ୨୧ ────
// CodeJAM EasyRide: Bus Ticket Reservation System

import java.util.ArrayList;
import java.util.Scanner;

// ===================================================
//             ☆ ABSTRACTION ☆  
//  This abstract class acts as a template for all
//  destination types. We only show the important
//  methods and hide the details. 
// ===================================================
abstract class Destination {
    public abstract void showDestinations();
    public abstract double getFare(int choice);
    public abstract String getName(int choice);
}

// ===================================================
//       𖹭 INHERITANCE + POLYMORPHISM 𖹭
//  BusDestination inherits from Destination and
//  provides its own version of the abstract methods.
// ===================================================
class BusDestination extends Destination {

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
        return fares[choice - 1];
    }

    @Override
    public String getName(int choice) {
        return places[choice - 1];
    }
}

// ===================================================
//            ⋆.𐙚 ̊ ENCAPSULATION⋆.𐙚 ̊
// Booking class keeps its data private so they cannot
// be changed directly. Only getters are allowed.
// ===================================================
class Booking {
    private String bookingID;
    private String passengerName;
    private String destination;
    private int[] seatsBooked;
    private double totalFare;

    public Booking(String bookingID, String passengerName, String destination, int[] seatsBooked, double totalFare) {
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
    public int[] getSeatsBooked() { return seatsBooked; }
    public double getTotalFare() { return totalFare; }

    // Setters
    public void setPassengerName(String passengerName) { this.passengerName = passengerName; }
    public void setSeatsBooked(int[] seatsBooked) { this.seatsBooked = seatsBooked; }
    public void setTotalFare(double totalFare) { this.totalFare = totalFare; }
}

// ===================================================
//     ⋆.𐙚 ̊ ENCAPSULATION (Bus class) ⋆.𐙚
// This class handles seat management. Seats cannot be
//  directly edited because they are private.
// ===================================================
class Bus {
    private final int totalSeats = 40;
    private boolean[] seats = new boolean[totalSeats]; // false = available, true = booked

    public int getTotalSeats() { return totalSeats; }

    public void viewSeatAvailability() {
        System.out.println("\n--- SEAT AVAILABILITY ---");
        for (int i = 0; i < seats.length; i++) {
            System.out.printf("%2d [%s]  ", i + 1, seats[i] ? "X" : "O");
            if ((i + 1) % 8 == 0) System.out.println(); // 8 seats per row
        }
    }

    public int getRemainingSeats() {
        int count = 0;
        for (boolean b : seats) if (!b) count++;
        return count;
    }

    public boolean bookSeats(int[] chosenSeats) {
        for (int s : chosenSeats) {
            if (s < 1 || s > totalSeats || seats[s - 1]) return false;
        }
        for (int s : chosenSeats) seats[s - 1] = true;
        return true;
    }

    public void cancelSeats(int[] cancelledSeats) {
        for (int s : cancelledSeats) {
            if (s >= 1 && s <= totalSeats) seats[s - 1] = false;
        }
    }
}

// ===================================================
//             𖹭.ᐟ MAIN PROGRAM 𖹭.ᐟ
// Handles everything: menu, booking, cancel, admin.
// ===================================================
public class Main {
    private static Scanner sc = new Scanner(System.in);

    private static Destination dest = new BusDestination();
    private static Bus bus = new Bus();

    private static ArrayList<Booking> bookings = new ArrayList<>();

    private static int bookingCounter = 1;

    private static String lastUserName = "";

    public static void main(String[] args) throws Exception {

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

            try {
                System.out.print("Enter your choice: ");
                choice = sc.nextInt();
            } catch (Exception e) {
                System.out.println("Invalid input! Numbers only.");
                sc.nextLine();
                continue;
            }

            switch (choice) {
                case 1: viewDestinations(); break;
                case 2: bus.viewSeatAvailability(); break;
                case 3: bookTicket(); break;
                case 4: cancelBooking(); break;
                case 5: adminLogin(); break;
                case 6:
                    if (lastUserName.isEmpty()) {
                        System.out.println("Thank you for riding with us, God bless you! ♡");
                    } else {
                        System.out.println("Thank you for riding with us, " + lastUserName +"! God bless you! ♡");
                    }
                    return;
                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }

    private static void viewDestinations() { dest.showDestinations(); }

    // ====================================
    //  Booking a Ticket with Seat Selection
    // ====================================
    private static void bookTicket() {
        sc.nextLine();
        System.out.print("Enter passenger name: ");
        String name = sc.nextLine();
        lastUserName = name;

        dest.showDestinations();
        bus.viewSeatAvailability();

        int choice;
        try {
            System.out.print("Choose destination (1-5): ");
            choice = sc.nextInt();

            if (choice < 1 || choice > 5) {
                System.out.println("Invalid destination number.");
                return;
            }

            System.out.print("How many seats to book? ");
            int seatCount = sc.nextInt();

            if (seatCount <= 0) {
                System.out.println("Invalid seat number.");
                return;
            }

            int[] chosenSeats = new int[seatCount];
            for (int i = 0; i < seatCount; i++) {
                System.out.print("Choose seat #" + (i + 1) + ": ");
                chosenSeats[i] = sc.nextInt();
            }

            if (!bus.bookSeats(chosenSeats)) {
                System.out.println("Some seats are already booked or invalid.");
                return;
            }

            String bookingID = String.format("B%03d", bookingCounter++);
            double fare = dest.getFare(choice);
            double total = fare * seatCount;

            Booking b = new Booking(bookingID, name, dest.getName(choice), chosenSeats, total);
            bookings.add(b);

            System.out.println("\n--- BOOKING CONFIRMATION ---");
            System.out.println("Booking ID: " + bookingID);
            System.out.println("Destination: " + dest.getName(choice));
            System.out.print("Seats Booked: ");
            for (int s : chosenSeats) System.out.print(s + " ");
            System.out.println("\nTotal Fare: ₱" + total);
            System.out.println("Seats Remaining: " + bus.getRemainingSeats());

        } catch (Exception e) {
            System.out.println("Invalid input! Please enter numbers only.");
            sc.nextLine();
        }
    }

    // ===============================
    // CANCEL BOOKING USING ID
    // ===============================
    private static void cancelBooking() {
        sc.nextLine();

        try {
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
        } catch (Exception e) {
            System.out.println("Error cancelling booking.");
        }
    }

    // ===============================
    // ADMIN LOGIN
    // ===============================
    private static void adminLogin() {
        int attempts = 3;
        sc.nextLine();

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

            try {
                System.out.print("Enter choice: ");
                choice = sc.nextInt();
            } catch (Exception e) {
                System.out.println("Invalid input! Numbers only.");
                sc.nextLine();
                continue;
            }

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
            System.out.print(
                b.getBookingID() + " | " +
                b.getPassengerName() + " | " +
                b.getDestination() + " | Seats: "
            );
            for (int s : b.getSeatsBooked()) System.out.print(s + " ");
            System.out.printf("| Total: ₱%.2f%n", b.getTotalFare());
        }
    }

    private static void removeBooking() {
        sc.nextLine();

        try {
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

        } catch (Exception e) {
            System.out.println("Error removing booking.");
        }
    }
}
