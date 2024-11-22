import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Cab class
class Cab {
    private int cabId;
    private String cabType;
    private boolean available;

    public Cab(int cabId, String cabType) {
        this.cabId = cabId;
        this.cabType = cabType;
        this.available = true;
    }

    public int getCabId() {
        return cabId;
    }

    public String getCabType() {
        return cabType;
    }

    public boolean isAvailable() {
        return available;
    }

    public void setAvailable(boolean available) {
        this.available = available;
    }

    @Override
    public String toString() {
        return "Cab ID: " + cabId + ", Type: " + cabType + ", Available: " + available;
    }
}

// Driver class
class Driver {
    private int driverId;
    private String name;
    private String licenseNumber;

    public Driver(int driverId, String name, String licenseNumber) {
        this.driverId = driverId;
        this.name = name;
        this.licenseNumber = licenseNumber;
    }

    public int getDriverId() {
        return driverId;
    }

    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return "Driver ID: " + driverId + ", Name: " + name + ", License: " + licenseNumber;
    }
}

// Booking class
class Booking {
    private int bookingId;
    private String customerName;
    private Cab cab;
    private Driver driver;

    public Booking(int bookingId, String customerName, Cab cab, Driver driver) {
        this.bookingId = bookingId;
        this.customerName = customerName;
        this.cab = cab;
        this.driver = driver;
    }

    @Override
    public String toString() {
        return "Booking ID: " + bookingId + ", Customer: " + customerName + ", Cab: [" + cab + "], Driver: [" + driver + "]";
    }
}

// CorporateCabsService
class CorporateCabsService {
    private List<Cab> cabs = new ArrayList<>();
    private List<Driver> drivers = new ArrayList<>();
    private List<Booking> bookings = new ArrayList<>();
    private int bookingCounter = 1;

    public void addCab(Cab cab) {
        cabs.add(cab);
    }

    public void addDriver(Driver driver) {
        drivers.add(driver);
    }

    public Booking bookCab(String customerName, String cabType) {
        for (Cab cab : cabs) {
            if (cab.isAvailable() && cab.getCabType().equalsIgnoreCase(cabType)) {
                for (Driver driver : drivers) {
                    cab.setAvailable(false);
                    Booking booking = new Booking(bookingCounter++, customerName, cab, driver);
                    bookings.add(booking);
                    return booking;
                }
            }
        }
        return null;
    }

    public void displayAvailableCabs() {
        for (Cab cab : cabs) {
            if (cab.isAvailable()) {
                System.out.println(cab);
            }
        }
    }

    public void displayBookings() {
        for (Booking booking : bookings) {
            System.out.println(booking);
        }
    }
}

// Main Class
public class CorporateCabsApp {
    public static void main(String[] args) {
        CorporateCabsService service = new CorporateCabsService();
        Scanner scanner = new Scanner(System.in);

        // Adding sample data
        service.addCab(new Cab(1, "Sedan"));
        service.addCab(new Cab(2, "SUV"));
        service.addDriver(new Driver(1, "John Doe", "XYZ123"));
        service.addDriver(new Driver(2, "Jane Smith", "ABC456"));

        while (true) {
            System.out.println("\n--- Corporate Cabs ---");
            System.out.println("1. Add Cab");
            System.out.println("2. Add Driver");
            System.out.println("3. Book Cab");
            System.out.println("4. Display Available Cabs");
            System.out.println("5. Display Bookings");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter Cab ID: ");
                    int cabId = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter Cab Type (Sedan/SUV): ");
                    String cabType = scanner.nextLine();
                    service.addCab(new Cab(cabId, cabType));
                    System.out.println("Cab added successfully!");
                    break;

                case 2:
                    System.out.print("Enter Driver ID: ");
                    int driverId = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter Driver Name: ");
                    String driverName = scanner.nextLine();
                    System.out.print("Enter License Number: ");
                    String license = scanner.nextLine();
                    service.addDriver(new Driver(driverId, driverName, license));
                    System.out.println("Driver added successfully!");
                    break;

                case 3:
                    System.out.print("Enter Customer Name: ");
                    String customerName = scanner.nextLine();
                    System.out.print("Enter Cab Type (Sedan/SUV): ");
                    String bookingType = scanner.nextLine();
                    Booking booking = service.bookCab(customerName, bookingType);
                    if (booking != null) {
                        System.out.println("Booking successful: " + booking);
                    } else {
                        System.out.println("No available cabs of the specified type.");
                    }
                    break;

                case 4:
                    System.out.println("Available Cabs:");
                    service.displayAvailableCabs();
                    break;

                case 5:
                    System.out.println("All Bookings:");
                    service.displayBookings();
                    break;

                case 6:
                    System.out.println("Exiting Corporate Cabs...");
                    scanner.close();
                    System.exit(0);

                default:
                    System.out.println("Invalid choice! Please try again.");
            }
        }
    }
}
