/**
 * Algorithm class containing implementations of searching algorithms
 * for DCIT 204 Lab 1
 */
public class Algorithm {

    /**
     * Linear Search - Searches through array element by element
     * Time Complexity: O(n)
     * Space Complexity: O(1)
     * 
     * @param array The array to search in
     * @param target The value to search for
     * @return The index of the target if found, -1 otherwise
     */
    public static int linearSearch(int[] array, int target) {
        // Check if array is null or empty
        if (array == null || array.length == 0) {
            return -1;
        }
        
        // Iterate through each element in the array
        for (int i = 0; i < array.length; i++) {
            if (array[i] == target) {
                return i; // Target found at index i
            }
        }
        return -1; // Target not found
    }

    /**
     * Binary Search - Searches in a sorted array by repeatedly dividing search space
     * Time Complexity: O(log n)
     * Space Complexity: O(1)
     * 
     * @param array The sorted array to search in
     * @param target The value to search for
     * @return The index of the target if found, -1 otherwise
     */
    public static int binarySearch(int[] array, int target) {
        // Check if array is null or empty
        if (array == null || array.length == 0) {
            return -1;
        }
        
        // Check if array is sorted (Binary Search requires sorted array)
        if (!isSorted(array)) {
            System.out.println("Warning: Array is not sorted. Binary Search requires a sorted array.");
            return -1;
        }
        
        int left = 0;
        int right = array.length - 1;
        
        while (left <= right) {
            // Calculate middle index to avoid overflow
            int mid = left + (right - left) / 2;
            
            if (array[mid] == target) {
                return mid; // Target found at index mid
            }
            
            if (array[mid] < target) {
                left = mid + 1; // Search in the right half
            } else {
                right = mid - 1; // Search in the left half
            }
        }
        return -1; // Target not found
    }

    /**
     * Helper method to check if an array is sorted in ascending order
     * 
     * @param array The array to check
     * @return true if sorted, false otherwise
     */
    private static boolean isSorted(int[] array) {
        for (int i = 0; i < array.length - 1; i++) {
            if (array[i] > array[i + 1]) {
                return false;
            }
        }
        return true;
    }

    /**
     * Helper method to sort an array using Bubble Sort
     * This is for demonstration purposes - Binary Search requires sorted input
     * 
     * @param array The array to sort
     */
    public static void sortArray(int[] array) {
        if (array == null || array.length <= 1) {
            return;
        }
        
        int n = array.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (array[j] > array[j + 1]) {
                    // Swap
                    int temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                }
            }
        }
    }

    /**
     * Binary Search with optional sorting
     * 
     * @param array The array to search in
     * @param target The value to search for
     * @param sortFirst If true, sort the array before searching
     * @return The index of the target if found, -1 otherwise
     */
    public static int binarySearch(int[] array, int target, boolean sortFirst) {
        if (array == null || array.length == 0) {
            return -1;
        }
        
        // Create a copy to avoid modifying original array if sorting
        int[] searchArray = array.clone();
        
        if (sortFirst) {
            sortArray(searchArray);
            System.out.println("Array sorted for Binary Search: " + java.util.Arrays.toString(searchArray));
        }
  
   import java.util.Arrays;
import java.util.Scanner;

/**
 * Main class to demonstrate searching algorithms
 * for DCIT 204 Lab 1
 */
public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        try {
            // Step 1: Get array size
            System.out.print("Enter array size: ");
            int size = scanner.nextInt();
            
            // Validate size
            while (size <= 0) {
                System.out.println("Array size must be positive. Please enter again: ");
                size = scanner.nextInt();
            }
            
            // Step 2: Create array and get elements
            int[] array = new int[size];
            System.out.println("Enter array elements:");
            for (int i = 0; i < size; i++) {
                System.out.print("Enter element " + (i + 1) + ": ");
                array[i] = scanner.nextInt();
            }
            
            // Display the original array
            System.out.println("\nOriginal Array: " + Arrays.toString(array));
            
            // Step 3: Get target value
            System.out.print("\nEnter target value: ");
            int target = scanner.nextInt();
            
            System.out.println("\n" + "=".repeat(50));
            System.out.println("SEARCH RESULTS");
            System.out.println("=".repeat(50));
            
            // Step 4: Perform Linear Search
            System.out.println("\nLINEAR SEARCH:");
            System.out.println("-".repeat(30));
            long linearStartTime = System.nanoTime();
            int linearResult = Algorithm.linearSearch(array, target);
            long linearEndTime = System.nanoTime();
            long linearTime = linearEndTime - linearStartTime;
            
            displaySearchResult("Linear Search", linearResult, array, target, linearTime);
            
            // Step 5: Perform Binary Search
            System.out.println("\nBINARY SEARCH:");
            System.out.println("-".repeat(30));
            
            // Option 1: Check if array is sorted and warn user
            System.out.println("Performing Binary Search on original array...");
            long binaryStartTime = System.nanoTime();
            int binaryResult = Algorithm.binarySearch(array, target);
            long binaryEndTime = System.nanoTime();
            long binaryTime = binaryEndTime - binaryStartTime;
            
            if (binaryResult == -1 && !Algorithm.isSorted(array)) {
                System.out.println("Note: Binary Search requires a sorted array.");
                System.out.println("Current array is not sorted.");
                System.out.println("\nWould you like to:");
                System.out.println("1. Sort the array and try Binary Search again");
                System.out.println("2. Continue without Binary Search");
                System.out.print("Enter your choice (1 or 2): ");
                int choice = scanner.nextInt();
                
                if (choice == 1) {
                    // Option 2: Sort and search again
                    System.out.println("\nSorting array and performing Binary Search...");
                    int[] sortedArray = array.clone();
                    Algorithm.sortArray(sortedArray);
                    System.out.println("Sorted Array: " + Arrays.toString(sortedArray));
                    
                    binaryStartTime = System.nanoTime();
                    binaryResult = Algorithm.binarySearch(sortedArray, target);
                    binaryEndTime = System.nanoTime();
                    binaryTime = binaryEndTime - binaryStartTime;
                    
                    displaySearchResult("Binary Search (Sorted)", binaryResult, sortedArray, target, binaryTime);
                } else {
                    System.out.println("Binary Search skipped.");
                }
            } else {
                displaySearchResult("Binary Search", binaryResult, array, target, binaryTime);
            }
            
            // Step 6: Performance Comparison
            System.out.println("\n" + "=".repeat(50));
            System.out.println("PERFORMANCE COMPARISON");
            System.out.println("=".repeat(50));
            System.out.printf("Linear Search Time:  %d ns%n", linearTime);
            System.out.printf("Binary Search Time:  %d ns%n", binaryTime);
            
            // Additional analysis if both searches were successful
            if (linearResult != -1 && binaryResult != -1) {
                System.out.println("\nBoth algorithms found the target.");
                if (linearTime > binaryTime) {
                    System.out.println("Binary Search was faster in this case.");
                } else if (binaryTime > linearTime) {
                    System.out.println("Linear Search was faster in this case.");
                } else {
                    System.out.println("Both algorithms had similar performance.");
                }
            }
            
        } catch (Exception e) {
            System.out.println("Error: Invalid input. Please enter valid integers.");
            e.printStackTrace();
        } finally {
            scanner.close();
        }
    }
    
    /**
     * Helper method to display search results in a formatted way
     */
    private static void displaySearchResult(String algorithmName, int result, int[] array, int target, long time) {
        if (result != -1) {
            System.out.printf("✓ Target %d found at index %d%n", target, result);
            System.out.printf("  Time taken: %d ns%n", time);
            
            // Show surrounding elements for context
            int start = Math.max(0, result - 2);
            int end = Math.min(array.length - 1, result + 2);
            System.out.print("  Context: [");
            for (int i = start; i <= end; i++) {
                if (i == result) {
                    System.out.print("[" + array[i] + "] ");
                } else {
                    System.out.print(array[i] + " ");
                }
            }
            System.out.println("]");
        } else {
            System.out.printf("✗ Target %d not found in the array%n", target);
            System.out.printf("  Time taken: %d ns%n", time);
        }
    }
}



        return binarySearch(searchArray, target);
    }
}
