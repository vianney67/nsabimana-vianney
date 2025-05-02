# nsabimana-vianney
##Practice
```java
package land;
import java.util.Date;

public class AgriculturalLand extends Land{
    public AgriculturalLand(String landId, String ownerName, String location, double sizeInAcres, Date registrationDate, String landUseStatus) {
        super(landId, ownerName, location, sizeInAcres, registrationDate, landUseStatus);
    }

    @Override
    public boolean validateOwnership() {
        return ownerName != null && !ownerName.trim().isEmpty();
    }

    @Override
    public boolean checkZoningCompliance() {
        // Simulating a check (you could add a zoning attribute)
        return location.toLowerCase().contains("farm") && sizeInAcres >= 1;
    }

    @Override
    public double calculateTax() {
        return sizeInAcres * 5000 * 0.01;
    }

    @Override
    public void generateLandReport() {
        System.out.println("=== Agricultural Land Report ===");
        System.out.printf("Land ID: %s%nOwner: %s%nLocation: %s%nSize: %.2f acres%nUse: %s%n", landId, ownerName, location, sizeInAcres, landUseStatus);
        System.out.printf("Ownership Valid: %s%nZoning Compliant: %s%nTax: $%.2f%n", validateOwnership(), checkZoningCompliance(), calculateTax());
    }
}

package land;
import java.util.Date;

public class CommercialLand extends Land{
    public CommercialLand(String landId, String ownerName, String location, double sizeInAcres, Date registrationDate, String landUseStatus) {
        super(landId, ownerName, location, sizeInAcres, registrationDate, landUseStatus);
    }

    @Override
    public boolean validateOwnership() {
        return ownerName != null && !ownerName.trim().isEmpty();
    }

    @Override
    public boolean checkZoningCompliance() {
        return location.toLowerCase().contains("commercial");
    }

    @Override
    public double calculateTax() {
        return sizeInAcres * 10000 * 0.025;
    }

    @Override
    public void generateLandReport() {
        System.out.println("=== Commercial Land Report ===");
        System.out.printf("Land ID: %s%nOwner: %s%nLocation: %s%nSize: %.2f acres%nUse: %s%n", landId, ownerName, location, sizeInAcres, landUseStatus);
        System.out.printf("Ownership Valid: %s%nZoning Compliant: %s%nTax: $%.2f%n", validateOwnership(), checkZoningCompliance(), calculateTax());
    }
}
package land;
import java.util.Date;

public class IndustrialLand extends Land {
    private boolean hasEnvironmentalClearance;

    public IndustrialLand(String landId, String ownerName, String location, double sizeInAcres, Date registrationDate, String landUseStatus, boolean hasEnvironmentalClearance) {
        super(landId, ownerName, location, sizeInAcres, registrationDate, landUseStatus);
        this.hasEnvironmentalClearance = hasEnvironmentalClearance;
    }

    @Override
    public boolean validateOwnership() {
        return ownerName != null && !ownerName.trim().isEmpty();
    }

    @Override
    public boolean checkZoningCompliance() {
        return location.toLowerCase().contains("industrial") && hasEnvironmentalClearance;
    }

    @Override
    public double calculateTax() {
        return sizeInAcres * 12000 * 0.03;
    }

    @Override
    public void generateLandReport() {
        System.out.println("=== Industrial Land Report ===");
        System.out.printf("Land ID: %s%nOwner: %s%nLocation: %s%nSize: %.2f acres%nUse: %s%n", landId, ownerName, location, sizeInAcres, landUseStatus);
        System.out.printf("Ownership Valid: %s%nZoning Compliant: %s%nTax: $%.2f%n", validateOwnership(), checkZoningCompliance(), calculateTax());
    }
}

package land;
import java.util.Date;
public abstract class Land {
        protected String landId;
        protected String ownerName;
        protected String location;
        protected double sizeInAcres;
        protected Date registrationDate;
        protected String landUseStatus;

        public Land(String landId, String ownerName, String location, double sizeInAcres, Date registrationDate, String landUseStatus) {
            this.landId = landId;
            this.ownerName = ownerName;
            this.location = location;
            this.sizeInAcres = sizeInAcres;
            this.registrationDate = registrationDate;
            this.landUseStatus = landUseStatus;
        }

        public abstract boolean validateOwnership();

        public abstract boolean checkZoningCompliance();

        public abstract double calculateTax();

        public abstract void generateLandReport();
    }


package land;
import java.util.*;
public class LandRegistry {
        private List<Land> lands = new ArrayList<>();

        public void registerLand(Land land) {
            if (land.validateOwnership()) {
                lands.add(land);
                System.out.println("Land registered successfully: " + land.landId);
            } else {
                System.out.println("Ownership validation failed. Cannot register land: " + land.landId);
            }
        }

        public void generateAllReports() {
            for (Land land : lands) {
                land.generateLandReport();
                System.out.println("----------------------------------");
            }
        }

        public void searchByOwner(String owner) {
            for (Land land : lands) {
                if (land.ownerName.equalsIgnoreCase(owner)) {
                    land.generateLandReport();
                    System.out.println("----------------------------------");
                }
            }
        }

        public void searchByType(Class<?> type) {
            for (Land land : lands) {
                if (type.isInstance(land)) {
                    land.generateLandReport();
                    System.out.println("----------------------------------");
                }
            }
        }
    }

package land;
import java.util.Date;
import java.util.Scanner;

public class Main {

        public static void main(String[] args) {
            LandRegistry registry = new LandRegistry();
            Scanner scanner = new Scanner(System.in);

            System.out.println("=== Land Management System ===");
            while (true) {
                System.out.println("\nSelect an option:");
                System.out.println("1. Register new land");
                System.out.println("2. Generate all reports");
                System.out.println("3. Search by owner");
                System.out.println("4. Search by type");
                System.out.println("5. Exit");
                System.out.print("Your choice: ");
                int choice = scanner.nextInt();
                scanner.nextLine(); // consume newline

                switch (choice) {
                    case 1 -> {
                        System.out.println("Enter land type (agricultural, residential, commercial, industrial): ");
                        String type = scanner.nextLine().trim().toLowerCase();

                        System.out.print("Enter Land ID: ");
                        String landId = scanner.nextLine();

                        System.out.print("Enter Owner Name: ");
                        String ownerName = scanner.nextLine();

                        System.out.print("Enter Location: ");
                        String location = scanner.nextLine();

                        System.out.print("Enter Size in Acres: ");
                        double size = scanner.nextDouble();
                        scanner.nextLine(); // consume newline

                        System.out.print("Enter Land Use Status: ");
                        String useStatus = scanner.nextLine();

                        Land land = null;
                        switch (type) {
                            case "agricultural" -> land = new AgriculturalLand(landId, ownerName, location, size, new Date(), useStatus);
                            case "residential" -> {
                                System.out.print("Enter number of residential units: ");
                                int units = scanner.nextInt();
                                scanner.nextLine(); // consume newline
                                land = new ResidentialLand(landId, ownerName, location, size, new Date(), useStatus, units);
                            }
                            case "commercial" -> land = new CommercialLand(landId, ownerName, location, size, new Date(), useStatus);
                            case "industrial" -> {
                                System.out.print("Environmental clearance obtained (true/false): ");
                                boolean clearance = scanner.nextBoolean();
                                scanner.nextLine(); // consume newline
                                land = new IndustrialLand(landId, ownerName, location, size, new Date(), useStatus, clearance);
                            }
                            default -> System.out.println("Invalid land type.");
                        }

                        if (land != null) {
                            registry.registerLand(land);
                        }
                    }
                    case 2 -> registry.generateAllReports();

                    case 3 -> {
                        System.out.print("Enter owner name to search: ");
                        String ownerSearch = scanner.nextLine();
                        registry.searchByOwner(ownerSearch);
                    }

                    case 4 -> {
                        System.out.print("Enter type to search (agricultural, residential, commercial, industrial): ");
                        String typeSearch = scanner.nextLine().trim().toLowerCase();
                        switch (typeSearch) {
                            case "agricultural" -> registry.searchByType(AgriculturalLand.class);
                            case "residential" -> registry.searchByType(ResidentialLand.class);
                            case "commercial" -> registry.searchByType(CommercialLand.class);
                            case "industrial" -> registry.searchByType(IndustrialLand.class);
                            default -> System.out.println("Unknown type.");
                        }
                    }

                    case 5 -> {
                        System.out.println("Exiting system. Goodbye!");
                        scanner.close();
                        return;
                    }

                    default -> System.out.println("Invalid choice. Try again.");
                }
            }
        }
    }

package land;
import java.util.Date;

public class ResidentialLand extends Land{
    private int residentialUnits;

    public ResidentialLand(String landId, String ownerName, String location, double sizeInAcres, Date registrationDate, String landUseStatus, int residentialUnits) {
        super(landId, ownerName, location, sizeInAcres, registrationDate, landUseStatus);
        this.residentialUnits = residentialUnits;
    }

    @Override
    public boolean validateOwnership() {
        return ownerName != null && !ownerName.trim().isEmpty();
    }

    @Override
    public boolean checkZoningCompliance() {
        return residentialUnits <= (sizeInAcres * 2);
    }

    @Override
    public double calculateTax() {
        return sizeInAcres * 8000 * 0.015;
    }

    @Override
    public void generateLandReport() {
        System.out.println("=== Residential Land Report ===");
        System.out.printf("Land ID: %s%nOwner: %s%nLocation: %s%nSize: %.2f acres%nUse: %s%n", landId, ownerName, location, sizeInAcres, landUseStatus);
        System.out.printf("Ownership Valid: %s%nZoning Compliant: %s%nTax: $%.2f%n", validateOwnership(), checkZoningCompliance(), calculateTax());
    }
}


```
