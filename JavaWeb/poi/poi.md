# poi

Java专门用于解析excel，Word等一些办公文本，获取里面的数据



## 解析excel

#### 读取Excel 

​	第一步：获取excel路径，创建**工作薄对象**  [需要传入解析工作薄的位置]

​		一个工作薄中有多个工作表，一个工作薄对象可以获取多个工作表对象

​	第二步：通过工作薄获取工作表对象

​		一个工作表中有n行n列，通过工作表可以获取到有效行数量

​	第三步：通过工作表对象获取行对象

​		一个行对象中有n列，通过行对象可以获取列的有效列数量

​	第四步：通过行对象获取列对象

​		一个列中存有一个excel值，可以清楚知道该值类型，获取该值

​	第五步：判断列中的值类型，不同的类型不同的方式获取

​		获取值的时候注意值类型，获取没有Object方式，获取方式是String就按String获取若表

​		中不是String则报错

```java
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFCell;
import java.io.FileInputStream;
public class ReadXL {
    /** Excel文件的存放位置。注意是反斜线*/
    public static String fileToBeRead = "D:\\test1.xls";
    public static void main(String argv[]) {
        try {
            // 创建对Excel工作簿对象
            HSSFWorkbook workbook = new HSSFWorkbook(new FileInputStream(fileToBeRead));
            // 创建对工作表的引用。
            // 本例是按名引用（让我们假定那张表有着缺省名"Sheet1"）
            HSSFSheet sheet = workbook.getSheet("Sheet1");
            // 也可用getSheetAt(int index)按索引引用，
            // 在Excel文档中，第一张工作表的缺省索引是0，
            // 其语句为：HSSFSheet sheet = workbook.getSheetAt(0);
            //获取第一行
            HSSFRow row = sheet.getRow(0);
            //获取第一列
            HSSFCell cell = row.getCell((short)0);
            // 输出第一行，第一列的值
            System.out.println("左上端单元是： " + cell.getStringCellValue());
        } catch (Exception e) {
            System.out.println("已运行xlRead() : " + e);
        }
    }
}
```









#### 写入Excel

​	第一步：

​		创建工作薄对象[需要传入生成工作薄的位置]

​	第二步：

​		通过工作薄创建工作表对象

​	第三步：

​		获取行对象，向行对象中的列对象写入值

​	第四步：

​		将工作薄生成

```java
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFCell;
import java.io.FileOutputStream;
public class CreateXL {
    /** Excel 文件要存放的位置，假定在D盘下*/
    public static String outputFile = "D:\\test.xls";
    public static void main(String argv[]) {
        try {
            // 创建新的Excel 工作簿
            HSSFWorkbook workbook = new HSSFWorkbook();
            // 在Excel工作簿中建一工作表，其名为缺省值
            // 如要新建一名为"效益指标"的工作表，其语句为：
            // HSSFSheet sheet = workbook.createSheet("效益指标");
            HSSFSheet sheet = workbook.createSheet();
            // 在索引0的位置创建行（最顶端的行）
            HSSFRow row = sheet.createRow((short)0);
            //在索引0的位置创建单元格（左上端）
            HSSFCell cell = row.createCell((short)0);
            // 定义单元格为字符串类型
            cell.setCellType(HSSFCell.CELL_TYPE_STRING);//已过时
            // 在单元格中输入一些内容
            cell.setCellValue("增加值");
            // 新建一输出文件流
            FileOutputStream fOut = new FileOutputStream(outputFile);
            // 把相应的Excel 工作簿存盘
            workbook.write(fOut);
            fOut.flush();
            // 操作结束，关闭文件
            fOut.close();
            System.out.println("文件生成...");
        } catch (Exception e) {
            System.out.println("已运行 xlCreate() : " + e);
        }
    }
}
```









