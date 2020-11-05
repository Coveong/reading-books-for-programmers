# 10장 클래스

# TOC

- [Contents](#contents)
  * [클래스 체계](#클래스-체계)
    + [캡슐화](#캡슐화)
  * [클래스는 작아야 한다!](#클래스는-작아야-한다!)
    + [단일 책임 원칙 (Single Responsibility Principle)](#단일-책임-원칙-(single-responsibility-principle))
    + [응집도 (Cohesion)](#응집도-(cohesion))
    + [응집도를 유지하면 작은 클래스 여럿이 나온다.](#응집도를-유지하면-작은-클래스-여럿이-나온다)
    + [변경하기 쉬운 클래스](#변경하기-쉬운-클래스)
    + [변경으로부터 격리](#변경으로부터-격리)

- [Reference](#reference)


<br>

# Contents

## 클래스 체계

위에서 아래로 위 순서대로 선언한다.

1. 정적 공개 상수
2. 정적 비공개 변수
3. 비공개 인스턴스 변수
4. 공개 함수
5. 비공개 함수

<br>

### 캡슐화

테스트를 위해 **protected**로 바꾸는 것을 허용하는 경우도 있다.

단, 캡슐화를 푸는 것은 항상 **최후의 수단**으로 여겨야한다.

<br>

## 클래스는 작아야 한다!

```java
// bad
public class SuperDashboard extends JFrame implements MetaDataUser {
    public String getCustomizerLanguagePath()
    public void setSystemConfigPath(String systemConfigPath) 
    public String getSystemConfigDocument()
    public void setSystemConfigDocument(String systemConfigDocument) 
    public boolean getGuruState()
    public boolean getNoviceState()
    public boolean getOpenSourceState()
    public void showObject(MetaObject object) 
    public void showProgress(String s)
    public boolean isMetadataDirty()
    public void setIsMetadataDirty(boolean isMetadataDirty)
    public Component getLastFocusedComponent()
    public void setLastFocused(Component lastFocused)
    public void setMouseSelectState(boolean isMouseSelected) 
    public boolean isMouseSelected()
    public LanguageManager getLanguageManager()
    public Project getProject()
    public Project getFirstProject()
    public Project getLastProject()
    public String getNewProjectName()
    public void setComponentSizes(Dimension dim)
    public String getCurrentDir()
    public void setCurrentDir(String newDir)
    public void updateStatus(int dotPos, int markPos)
    public Class[] getDataBaseClasses()
    public MetadataFeeder getMetadataFeeder()
    public void addProject(Project project)
    public boolean setCurrentProject(Project project)
    public boolean removeProject(Project project)
    public MetaProjectHeader getProgramMetadata()
    public void resetDashboard()
    public Project loadProject(String fileName, String projectName)
    public void setCanSaveMetadata(boolean canSave)
    public MetaObject getSelectedObject()
    public void deselectObjects()
    public void setProject(Project project)
    public void editorAction(String actionName, ActionEvent event) 
    public void setMode(int mode)
    public FileManager getFileManager()
    public void setFileManager(FileManager fileManager)
    public ConfigManager getConfigManager()
    public void setConfigManager(ConfigManager configManager) 
    public ClassLoader getClassLoader()
		...
}
```

- 클래스 이름은 해당 클래스의 책임을 기술해야한다.
    - Processor, Manager, Super 와 같은 애매한 단어가 클래스 이름이 들어간다는건, 클래스가 책임을 많이 가지고 있고, 크기가 크다는 뜻이다.
- 클래스 설명은 **if, and, or, but을 사용하지 않고** **25단어 이내**로 설명이 가능해야 한다.

<br>

### 단일 책임 원칙 (Single Responsibility Principle)

클래스나 모듈을 변경할 이유가 하나여야한다.

큰 클래스 몇개로 이루어진 시스템 < **작은 클래스 여럿으로 이루어진 시스템**

<br>

### 응집도 (Cohesion)

인스턴스 변수 수가 작아야 한다.

함수를 작게, 매개변수 목록을 짧게

<br>

### 응집도를 유지하면 작은 클래스 여럿이 나온다.

```java
package literatePrimes;

public class PrintPrimes {
    public static void main(String[] args) {
        final int M = 1000; 
        final int RR = 50;
        final int CC = 4;
        final int WW = 10;
        final int ORDMAX = 30; 
        int P[] = new int[M + 1]; 
        int PAGENUMBER;
        int PAGEOFFSET; 
        int ROWOFFSET; 
        int C;
        int J;
        int K;
        boolean JPRIME;
        int ORD;
        int SQUARE;
        int N;
        int MULT[] = new int[ORDMAX + 1];

        J = 1;
        K = 1; 
        P[1] = 2; 
        ORD = 2; 
        SQUARE = 9;

        while (K < M) { 
            do {
                J = J + 2;
                if (J == SQUARE) {
                    ORD = ORD + 1;
                    SQUARE = P[ORD] * P[ORD]; 
                    MULT[ORD - 1] = J;
                }
                N = 2;
                JPRIME = true;
                while (N < ORD && JPRIME) {
                    while (MULT[N] < J)
                        MULT[N] = MULT[N] + P[N] + P[N];
                    if (MULT[N] == J) 
                        JPRIME = false;
                    N = N + 1; 
                }
            } while (!JPRIME); 
            K = K + 1;
            P[K] = J;
        } 
        {
            PAGENUMBER = 1; 
            PAGEOFFSET = 1;
            while (PAGEOFFSET <= M) {
                System.out.println("The First " + M + " Prime Numbers --- Page " + PAGENUMBER);
                System.out.println("");
                for (ROWOFFSET = PAGEOFFSET; ROWOFFSET < PAGEOFFSET + RR; ROWOFFSET++) {
                    for (C = 0; C < CC;C++)
                        if (ROWOFFSET + C * RR <= M)
                            System.out.format("%10d", P[ROWOFFSET + C * RR]); 
                    System.out.println("");
                }
                System.out.println("\f"); PAGENUMBER = PAGENUMBER + 1; PAGEOFFSET = PAGEOFFSET + RR * CC;
            }
        }
    }
}
```

위 코드의 문제점은

1. 들여쓰기가 심하다.
2. 이상한 변수가 많다.
3. 구조가 빡빡하게 결합되어있다.

위 코드를 바꿔보면?

```java
package literatePrimes;

public class PrimePrinter {
    public static void main(String[] args) {
        final int NUMBER_OF_PRIMES = 1000;
        int[] primes = PrimeGenerator.generate(NUMBER_OF_PRIMES);

        final int ROWS_PER_PAGE = 50; 
        final int COLUMNS_PER_PAGE = 4; 
        RowColumnPagePrinter tablePrinter = 
            new RowColumnPagePrinter(ROWS_PER_PAGE, 
                        COLUMNS_PER_PAGE, 
                        "The First " + NUMBER_OF_PRIMES + " Prime Numbers");
        tablePrinter.print(primes); 
    }
}
```

```java
package literatePrimes;

import java.io.PrintStream;

public class RowColumnPagePrinter { 
    private int rowsPerPage;
    private int columnsPerPage; 
    private int numbersPerPage; 
    private String pageHeader; 
    private PrintStream printStream;

    public RowColumnPagePrinter(int rowsPerPage, int columnsPerPage, String pageHeader) { 
        this.rowsPerPage = rowsPerPage;
        this.columnsPerPage = columnsPerPage; 
        this.pageHeader = pageHeader;
        numbersPerPage = rowsPerPage * columnsPerPage; 
        printStream = System.out;
    }

    public void print(int data[]) { 
        int pageNumber = 1;
        for (int firstIndexOnPage = 0 ; 
            firstIndexOnPage < data.length ; 
            firstIndexOnPage += numbersPerPage) { 
            int lastIndexOnPage =  Math.min(firstIndexOnPage + numbersPerPage - 1, data.length - 1);
            printPageHeader(pageHeader, pageNumber); 
            printPage(firstIndexOnPage, lastIndexOnPage, data); 
            printStream.println("\f");
            pageNumber++;
        } 
    }

    private void printPage(int firstIndexOnPage, int lastIndexOnPage, int[] data) { 
        int firstIndexOfLastRowOnPage =
        firstIndexOnPage + rowsPerPage - 1;
        for (int firstIndexInRow = firstIndexOnPage ; 
            firstIndexInRow <= firstIndexOfLastRowOnPage ;
            firstIndexInRow++) { 
            printRow(firstIndexInRow, lastIndexOnPage, data); 
            printStream.println("");
        } 
    }

    private void printRow(int firstIndexInRow, int lastIndexOnPage, int[] data) {
        for (int column = 0; column < columnsPerPage; column++) {
            int index = firstIndexInRow + column * rowsPerPage; 
            if (index <= lastIndexOnPage)
                printStream.format("%10d", data[index]); 
        }
    }

    private void printPageHeader(String pageHeader, int pageNumber) {
        printStream.println(pageHeader + " --- Page " + pageNumber);
        printStream.println(""); 
    }

    public void setOutput(PrintStream printStream) { 
        this.printStream = printStream;
    } 
}
```

```java
package literatePrimes;

import java.util.ArrayList;

public class PrimeGenerator {
    private static int[] primes;
    private static ArrayList<Integer> multiplesOfPrimeFactors;

    protected static int[] generate(int n) {
        primes = new int[n];
        multiplesOfPrimeFactors = new ArrayList<Integer>(); 
        set2AsFirstPrime(); 
        checkOddNumbersForSubsequentPrimes();
        return primes; 
    }

    private static void set2AsFirstPrime() { 
        primes[0] = 2; 
        multiplesOfPrimeFactors.add(2);
    }

    private static void checkOddNumbersForSubsequentPrimes() { 
        int primeIndex = 1;
        for (int candidate = 3 ; primeIndex < primes.length ; candidate += 2) { 
            if (isPrime(candidate))
                primes[primeIndex++] = candidate; 
        }
    }

    private static boolean isPrime(int candidate) {
        if (isLeastRelevantMultipleOfNextLargerPrimeFactor(candidate)) {
            multiplesOfPrimeFactors.add(candidate);
            return false; 
        }
        return isNotMultipleOfAnyPreviousPrimeFactor(candidate); 
    }

    private static boolean isLeastRelevantMultipleOfNextLargerPrimeFactor(int candidate) {
        int nextLargerPrimeFactor = primes[multiplesOfPrimeFactors.size()];
        int leastRelevantMultiple = nextLargerPrimeFactor * nextLargerPrimeFactor; 
        return candidate == leastRelevantMultiple;
    }

    private static boolean isNotMultipleOfAnyPreviousPrimeFactor(int candidate) {
        for (int n = 1; n < multiplesOfPrimeFactors.size(); n++) {
            if (isMultipleOfNthPrimeFactor(candidate, n)) 
                return false;
        }
        return true; 
    }

    private static boolean isMultipleOfNthPrimeFactor(int candidate, int n) {
        return candidate == smallestOddNthMultipleNotLessThanCandidate(candidate, n);
    }

    private static int smallestOddNthMultipleNotLessThanCandidate(int candidate, int n) {
        int multiple = multiplesOfPrimeFactors.get(n); 
        while (multiple < candidate)
            multiple += 2 * primes[n]; 
        multiplesOfPrimeFactors.set(n, multiple); 
        return multiple;
    } 
}
```

1. 좀 더 길고 서술적인 변수 이름을 사용했다.
2. 함수 선언과 클래스 선언으로 코드를 설명했다.
3. 가독성을 위해 공백을 추가하고 형식을 맞췄다.



<br>

### 변경하기 쉬운 클래스

```java
public class Sql {
    public Sql(String table, Column[] columns)
    public String create()
    public String insert(Object[] fields)
    public String selectAll()
    public String findByKey(String keyColumn, String keyValue)
    public String select(Column column, String pattern)
    public String select(Criteria criteria)
    public String preparedInsert()
    private String columnList(Column[] columns)
    private String valuesList(Object[] fields, final Column[] columns) 
	private String selectWithCriteria(String criteria)
    private String placeholderList(Column[] columns)
}
```

위 클래스는 변경할 이유가 두 가지이다.

1. update문 지원
2. select 문에 내장된 select문 지원

```java
abstract public class Sql {
	public Sql(String table, Column[] columns) 
	abstract public String generate();
}

public class CreateSql extends Sql {
	public CreateSql(String table, Column[] columns) 
	@Override public String generate()
}

public class SelectSql extends Sql {
	public SelectSql(String table, Column[] columns) 
	@Override public String generate()
}

public class InsertSql extends Sql {
	public InsertSql(String table, Column[] columns, Object[] fields) 
	@Override public String generate()
	private String valuesList(Object[] fields, final Column[] columns)
}

public class SelectWithCriteriaSql extends Sql { 
	public SelectWithCriteriaSql(
	String table, Column[] columns, Criteria criteria) 
	@Override public String generate()
}

public class SelectWithMatchSql extends Sql { 
	public SelectWithMatchSql(String table, Column[] columns, Column column, String pattern) 
	@Override public String generate()
}

public class FindByKeySql extends Sql public FindByKeySql(
	String table, Column[] columns, String keyColumn, String keyValue) 
	@Override public String generate()
}

public class PreparedInsertSql extends Sql {
	public PreparedInsertSql(String table, Column[] columns) 
	@Override public String generate() {
	private String placeholderList(Column[] columns)
}

public class Where {
	public Where(String criteria) public String generate()
	public String generate() {
}

public class ColumnList {
	public ColumnList(Column[] columns) public String generate()
	public String generate() {
	}
```

클래스가 서로 분리되었기 때문에 클래스가 단순해졌고, 코드는 쉽게 이해할 수 있게 되었다.

- 함수 하나를 수정했다고 다른 함수가 망가질 위험도 사라졌다.
- 테스트하기도 쉬워졌다.
- SRP와 OCP를 지원하게 되었다.

Update 문을 추가할 때는 기존 클래스를 변경하지 않고 새로운 클래스 UpdateSql을 구현하면 된다.



<br>

### 변경으로부터 격리

5분마다 값이 변경되는 API를 호출하는 클래스(Porfolio)가 있다면? 테스트를 어떻게 해야할까

Portfolio 클래스에서 API를 직접 호출하지 않고 StockExchange라는 인터페이스를 생성하고 이 인터페이스를 구현하는 TokyoStockExchange 클래스를 구현한다.

```java
public insterface StockExchange {
	Money currentPrice(String symbol);
}
```

```java
public Portfolio {
	private StockExchange exchange;
	public Portfolio(StockExchange exchange) {
		this.exchange = exchange;
	}
	// ...
}
```

테스트용 클래스를 만들 수 있다.

```java
public class PortfolioTest {
	private FixedStockExchangeStub exchange;
	private Portfolio portfolio;

	@Before
	protected void setUp() throws Exception {
		exchange = new FixedStockExchangeStub();
		exchange.fix("MSFT", 100);
		portfolio = new Portfolio(exchange);
	}

	@Test
	public void GivenFiveMSFTTotalShouldBe500() throws Exception {
		portfolio.add(5, "MSFT");
		Assert.assertEquals(500, portfolio.value());
	}
}
```

테스트가 가능할 정도로 시스템의 결합도를 낮추면 **유연성**과 **재사용성**이 높아진다.

- 시스템 요소가 다른 요소와 변경으로부터 잘 격리되어 있다는 의미
- 격리되어있으면 각 요소를 이해하기 더 쉬움

<br>

# Reference

[Clean Code](https://book.naver.com/bookdb/book_detail.nhn?bid=7390287)

[클린코드 10장 - 클래스](http://amazingguni.github.io/blog/2016/06/Clean-Code-10-%ED%81%B4%EB%9E%98%EC%8A%A4)