# Codesimport org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;

import java.io.FileInputStream;
import java.io.IOException;

public class ExcelUtil {

    public static String getCellValue(String filePath, int sheetIndex, int rowIndex, int cellIndex) {
        try (FileInputStream fis = new FileInputStream(filePath);
             Workbook workbook = new HSSFWorkbook(fis)) {

            Sheet sheet = workbook.getSheetAt(sheetIndex);
            Row row = sheet.getRow(rowIndex);
            if (row == null) {
                return "";
            }
            Cell cell = row.getCell(cellIndex);
            if (cell == null) {
                return "";
            }

            return getCellValueAsString(cell);
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }

    private static String getCellValueAsString(Cell cell) {
        switch (cell.getCellType()) {
            case STRING:
                return cell.getStringCellValue();
            case NUMERIC:
                return String.valueOf(cell.getNumericCellValue());
            case BOOLEAN:
                return String.valueOf(cell.getBooleanCellValue());
            case FORMULA:
                return cell.getCellFormula();
            default:
                return "";
        }
    }

    public static void main(String[] args) {
        String filePath = "path/to/your/file.xls";
        int sheetIndex = 0; // Sheet index (0-based)
        int rowIndex = 1;   // Row index (0-based)
        int cellIndex = 2;  // Cell index (0-based)

        String cellValue = getCellValue(filePath, sheetIndex, rowIndex, cellIndex);
        System.out.println("Cell Value: " + cellValue);
    }
}
