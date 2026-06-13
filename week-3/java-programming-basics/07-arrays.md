# Arrays in Java

---

## What is an Array?

An array is a **collection of elements** of the **same data type** stored in **contiguous memory locations**. Arrays have a **fixed size** — once created, the size cannot change.

---

## Syntax

### Declaration
```java
dataType[] arrayName;
```

### Initialization
```java
dataType[] arrayName = new dataType[size];
```

### Declaration + Initialization
```java
dataType[] arrayName = {value1, value2, value3};
```

---

## Creating Arrays

### Method 1: Using new keyword
```java
int[] numbers = new int[5];   // array of 5 integers (default values = 0)
```

### Method 2: With values
```java
int[] numbers = {10, 20, 30, 40, 50};
```

### Method 3: Separately
```java
int[] numbers;
numbers = new int[]{10, 20, 30, 40, 50};
```

---

## Accessing Array Elements

Use the **index** (starts from 0) to access elements.

```java
int[] nums = {10, 20, 30, 40, 50};

System.out.println(nums[0]);    // 10 (first element)
System.out.println(nums[2]);    // 30 (third element)
System.out.println(nums[4]);    // 50 (last element)
System.out.println(nums.length); // 5 (size of array)
```

---

## Modifying Array Elements

```java
int[] nums = {10, 20, 30};
nums[1] = 99;   // change second element

System.out.println(nums[1]);   // 99
```

---

## Iterating Through Arrays

### Using for loop
```java
int[] nums = {10, 20, 30, 40, 50};

for (int i = 0; i < nums.length; i++) {
    System.out.println("Index " + i + ": " + nums[i]);
}
```

### Using for-each loop
```java
int[] nums = {10, 20, 30, 40, 50};

for (int num : nums) {
    System.out.println(num);
}
```

---

## Multi-Dimensional Arrays

A 2D array is like a table with rows and columns.

### Syntax
```java
dataType[][] arrayName = new int[rows][cols];
```

### Example
```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Access element
System.out.println(matrix[1][2]);   // 6 (row 2, column 3)

// Iterate through 2D array
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

### Output:
```
1 2 3
4 5 6
7 8 9
```

---

## Common Array Operations

### Find Maximum
```java
int[] nums = {3, 7, 1, 9, 4};
int max = nums[0];

for (int i = 1; i < nums.length; i++) {
    if (nums[i] > max) {
        max = nums[i];
    }
}

System.out.println("Max: " + max);   // 9
```

### Find Minimum
```java
int[] nums = {3, 7, 1, 9, 4};
int min = nums[0];

for (int i = 1; i < nums.length; i++) {
    if (nums[i] < min) {
        min = nums[i];
    }
}

System.out.println("Min: " + min);   // 1
```

### Sum of Elements
```java
int[] nums = {10, 20, 30, 40, 50};
int sum = 0;

for (int num : nums) {
    sum += num;
}

System.out.println("Sum: " + sum);   // 150
```

### Reverse an Array
```java
int[] nums = {1, 2, 3, 4, 5};

for (int i = 0; i < nums.length / 2; i++) {
    int temp = nums[i];
    nums[i] = nums[nums.length - 1 - i];
    nums[nums.length - 1 - i] = temp;
}

for (int num : nums) {
    System.out.print(num + " ");   // 5 4 3 2 1
}
```

---

## Arrays vs ArrayList

| Feature | Array | ArrayList |
|---|---|---|
| Size | Fixed | Dynamic |
| Type | Can hold primitives | Only objects |
| Speed | Faster | Slower |
| Methods | No methods | Has add(), remove(), etc. |

---

## Example Program

```java
public class ArrayDemo {
    public static void main(String[] args) {
        // Create and print array
        String[] fruits = {"Apple", "Banana", "Cherry", "Date"};

        System.out.println("--- All Fruits ---");
        for (int i = 0; i < fruits.length; i++) {
            System.out.println(i + ": " + fruits[i]);
        }

        // Find length
        System.out.println("\nTotal fruits: " + fruits.length);

        // Access specific element
        System.out.println("First fruit: " + fruits[0]);
        System.out.println("Last fruit: " + fruits[fruits.length - 1]);
    }
}
```

### Output:
```
--- All Fruits ---
0: Apple
1: Banana
2: Cherry
3: Date

Total fruits: 4
First fruit: Apple
Last fruit: Date
```

---

## Key Points to Remember

- Array index starts from **0**.
- Array size is **fixed** after creation.
- Accessing an index beyond the size throws **ArrayIndexOutOfBoundsException**.
- Use `arrayName.length` to get the size.
- Use for-each loop for simple iteration.
- For dynamic sizing, use `ArrayList` instead of arrays.
