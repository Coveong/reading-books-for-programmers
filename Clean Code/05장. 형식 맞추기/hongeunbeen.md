# 5장 형식 맞추기

프로그래머라면 형식을 깔끔하게 맞춰 코드를 작성해야 함

코드 형식을 맞추기 위해 간단한 규칙 정하고 착실히 따라야 함

필요하마녀 규칙을 자동으로 적용하는 도구 활용!

## 형식을 맞추는 목적

- 코드의 형식은 중요
→ 오늘 구현한 기능이 다음 버전에서 바뀔 확률 높아 코드의 품질에 영향 있기 때문

## 적절한 행 길이를 유지하라

자바에서 파일 크기는 클래스 크기와 밀접
→ 일반적으로 큰 파일보다 작은 파일이 이해하기 쉬움

- **신문 기사처럼 작성하라**
    - 이름만 보고도 올바른 모듀을 살펴보고 있는지 아닌지 판단할 정도로  간단하면서도 설명이 가능
    - 소스 파일 첫 부분은 고차원 개념과 알고리즘 설명
    - 소스 파일 내려갈수록 의도를 세세하게 묘사 → 마지막에는 가장 저차원 함수와 세부 내역
- **개념은 빈 행으로 분리하라**
    - 각 행은 수식이나 절을 나타내고, 일련의 행 묶음은 완결된 생각 하나를 표현
    - 빈행은 새로운 개념을 시작한다는 시각적 단서
- **세로 밀집도**
    - 줄바꿈 → 개념 분리 / 세로밀집도 → 연관성 의미
    - 서로 밀접한 코드 행은 세로로 가까이 놓여야 함
- **수직 거리**
    - 서로 밀접한 개념은 세로로 가까이 둬야 함
    - 타당한 근거가 없다면 서로 밀접한 개념은 한 파일에 속해야 함
    → protected 변수를 피해야 하는 이유!!!
    - 같은 파일에 속할 정도록 밀접한 두 개념 → 세로 거리로 연관성(한 개념 이해하는데 다른 개념 중요 정도) 표현
        - 변수 선언 
        → 변수는 사용하는 위치에 최대한 가까이 선언
        → 루프 제어하는 변수는 흔히 루프 문 내부에 선언
        - 인스턴스 변수
        → 인스턴스 변수는 클래스 맨 처음에 선언
        → 클래스는 많은 클래스 메서드가 인스턴스 변수 사용!
        → 언어마다 다르지만 자바에선 보통 클래스 맨 처음에 인스턴스 변수 선언
        - 종속 함수
        → 한 함수가 다른 함수 호출 시 두 함수는 세로로 가까이 배치
        → 가능하다면 호출하는 함수를 호출되는 함수보다 먼저 배치
        → 모듈 전체의 가독성 높아짐
        - 개념적 유사성
        → 개념적인 친화도가 높을수록 코드를 가까이 배치
        → 종속적인 관계가 없더라도 가까이 배치할 수 있으면 배치!(서로가 서로 호출 관계 부차적 요인)
- **세로 순서**
    - 함수 호출 종속성은 아래 방향으로 유지
    - 호출되는 함수를 호출하는 함수보다 나중에 배치

    → 가장 중요한 개념을 가장 먼저 표현!!!(소스 파일에서 첫 함수 몇 개만 읽어도 개념 파악 쉬워짐)

## 가로 형식 맞추기

프로그래머는 명백하게 짧은 행 선호 ⇒ 저자의 생각으로는 120자 정도로 행 길이 제한!

- **가로 공백과 밀집도**
    - 가로로는 공백을 사용해 밀접한 개념과 느슨한 개념을 표현
    - 할당 연산자 강조 위해 앞뒤 공백
    - 함수와 인수 서로 밀접한 관계로 공백 X → 한 개념으로 보이기 위해

    → 한 개념으로 보이기 위해 공백 사용 X
    → 강조 혹은 별개의 개념으로 보이기 위해서 공백 사용

    - 승수 사이는 공백 없음!!( 곱샘은 우선순위가 가장 높기 때문)
        - `return b*b - 4*a*b + a`
- **가로 정렬**
    - 특정 구조를 강조하고자 가로 정렬 X
    - 정렬이 필요할 정도로 목록이 길다면 목록 길이를 줄이거나 쪼개기!!
- **들여쓰기**
    - 파일 전체에 적용되는 정보
    - 파일 내 개별 클래스에 적용되는 정보
    - 클래스 내 각 메서드에 적용되는 정보
    - 블록 내 블록에 재귀적으로 적용되는 정보

    → 계층에서 각 수준은 이름을 선언하는 범위이자 선언문과 실행문 해석 범위!

    → 범위로 이뤄진 계층 표현 위새 코드 들여쓴다!(파일 수준인 문장은 들여쓰기X)

    - 들여쓰기 무시하기 X

    → 때로는 간단한 if문, while문, 짧은 함수에서 들여쓰기 규칙 무시 유혹 생겨도 꼭 넣는거 추천

- **가짜 범위**
    - 빈 while문이나 for문 등 빈 블록도 올바로 들여쓰고 괄호로 감싸서 혼란 줄이기
    - 세미콜론은 새 행에다 제대로 들여써서 혼란 줄이기

    ```java
    while(dis.read(buf, 0, readBufferSize) != 1)
    ;
    ```

## 밥 아저씨의 형식 규칙

→ 코드 자체가 최고의 구현 표준 문서!!!!!

```java
import java.io.BufferedReader;
import java.io.File;
import java.util.List;
import java.util.Set;

public class CodeAnalyzer {
	//인스턴스 변수는 클래스 상위에 선언
	private int lineCount;
	private int maxLineWidth;
	private int widestLineNumber;
	private LineWidthHistogram lineWidthHistogram;
	private int totalChars;

	public CodeAnalyzer() {
		lineWidthHistogram = new LineWidthHistogram();
	}
	
	public static List<File> findJavaFiles(File parentDirectory) {
		List<File> files = new ArrayList<File>();
		findJavaFiles(parentDirectory, files);
		return files;
	}
	
	//개념적 유사도 + 종속 관계 수직 길이 가까이 유지
	public static void findJavaFiles(File parentDirectory, List<File> files) {
		for(File file : parentDirectory.listFiles()) {
			if (file.getName().endsWith(".java"))
				files.add(file);
			else if (file.isDirectory())
				findJavaFiles(file, files);
		}
	}
	
	public vvoid analyzeFile(File javaFile) throws Exception {
		BufferedReader br = new BufferedReader(new FileReader(javaFile));
		String line;
		while ((line = br.readLine()) != null)
			measureLine(line);
	}
	
	private void measureLine(String line) {
		lineCount++;
		int lineSize = line.length();
		totalChars += lineSize;
		lineWidthHistogram.addLine(lineSize, lineCount);
		recordWidestLine(lineSize);
	}
	
	private void recordWidestLine(int lineSize) {
		if (lineSize > maxLineWidth) {
			maxLineWidth = lineSize;
			widestLineNumber = lineCount;
		}
	}
	
	//개념적 유사한 함수 수직 길이 가까이 위치 (get*)
	public int getLineCount() {
		return lineCount;
	}
	
	public int getMaxLineWidth() {
		return maxLineWidth;
	}
	
	public int getWidestLineNumber() {
		return widestLineNumber;
	}
	
	public LineWidthHistogram getLineWidthHistogram() {
		return lineWidthHistogram;
	}
	
	public double getMeanLineWidth() {
		return (double)totalChars/lineCount;
	}
	
	public int getMedianLineWidth() {
		Integer[] sortedWidths = getSortedWidths();
		int cumulativeLineCount = 0;
		for (int width : sortedWidths) {
			cumulativeLineCount += lineCountForWidth(width);
			if (cumulativeLineCount > lineCount)
				return width;			
		}
		throws new Error("Cannot get here");
	}
	
	private int lineCountForWidth(int width) {
		return lineWidthHistogram.getLinesforWidth(width).size();
	}
	
	private Integer[] getSortedWidth() {
		Set<Integer> widths = lineWidthHistogram.getWidths();
		Integer[] sortedWidths = (widths.toArray(new Integer[0]));
		Arrays.sort(sortedWidths);
		return sortedWidths;
	}
	
}
```