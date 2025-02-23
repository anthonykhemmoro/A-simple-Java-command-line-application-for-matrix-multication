# A-simple-Java-command-line-application-for-matrix-multication
matrix multication assignment
import java.io.*;
import java.util.*;

public class MatrixMultiplication {

    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);

        
        System.out.println("Enter two file names (for matrix files) or an integer (for matrix size):");
        String input = scanner.nextLine();

        try {
        
            int n = Integer.parseInt(input);

            
            System.out.println("Generating two random matrices of size " + n + "x" + n + "...");
            int[][] matrix1 = generateRandomMatrix(n);
            int[][] matrix2 = generateRandomMatrix(n);

            
            writeMatrixToFile(matrix1, "matrix1.txt");
            writeMatrixToFile(matrix2, "matrix2.txt");

            
            int[][] result = multiplyMatrices(matrix1, matrix2);
            if (result != null) {
                writeMatrixToFile(result, "matrix3.txt");
                System.out.println("Matrices multiplied and result saved to matrix3.txt");
            }

        } catch (NumberFormatException e) {
            
            String[] files = input.split(" ");

            if (files.length == 2) {
                
                int[][] matrix1 = readMatrixFromFile(files[0]);
                int[][] matrix2 = readMatrixFromFile(files[1]);

                
                int[][] result = multiplyMatrices(matrix1, matrix2);
                if (result != null) {
                    writeMatrixToFile(result, "matrix3.txt");
                    System.out.println("Matrices read from files, multiplied, and result saved to matrix3.txt");
                }
            } else {
                System.out.println("Invalid input. Please provide two valid file names or an integer for matrix size.");
            }
        }
    }

   
    public static int[][] generateRandomMatrix(int n) {
        Random rand = new Random();
        int[][] matrix = new int[n][n];

        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                matrix[i][j] = rand.nextInt(10);
            }
        }

        return matrix;
    }

   
    public static int[][] multiplyMatrices(int[][] matrix1, int[][] matrix2) {
        int rows1 = matrix1.length;
        int cols1 = matrix1[0].length;
        int rows2 = matrix2.length;
        int cols2 = matrix2[0].length;

        
        if (cols1 != rows2) {
            System.out.println("Error: Matrices cannot be multiplied. Number of columns in the first matrix must match the number of rows in the second.");
            return null;
        }

        int[][] result = new int[rows1][cols2];

        
        for (int i = 0; i < rows1; i++) {
            for (int j = 0; j < cols2; j++) {
                for (int k = 0; k < cols1; k++) {
                    result[i][j] += matrix1[i][k] * matrix2[k][j];
                }
            }
        }

        return result;
    }

    
    public static int[][] readMatrixFromFile(String filename) throws IOException {
        List<int[]> matrixList = new ArrayList<>();
        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;

            
            while ((line = reader.readLine()) != null) {
                String[] tokens = line.split("\\s+"); 
                int[] row = new int[tokens.length];

                
                for (int i = 0; i < tokens.length; i++) {
                    row[i] = Integer.parseInt(tokens[i]);
                }
                matrixList.add(row);
            }
        }

       
        int[][] matrix = new int[matrixList.size()][];
        for (int i = 0; i < matrixList.size(); i++) {
            matrix[i] = matrixList.get(i);
        }

        return matrix;
    }

    
    public static void writeMatrixToFile(int[][] matrix, String filename) throws IOException {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filename))) {

            
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[i].length; j++) {
                    writer.write(matrix[i][j] + " "); 
                }
                writer.newLine(); 
        }
    }
}
