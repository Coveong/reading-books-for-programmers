# 14장 점진적인 개선

# Contents

## Args 구현

```java
public class Args {
  private Map<Character, ArgumentMarshaler> marshalers;
  private Set<Character> argsFound;
  private ListIterator<String> currentArgument;
  
  public Args(String schema, String[] args) throws ArgsException {
    marshalers = new HashMap<>();
    argsFound = new HashSet<>();
    
    parseSchema(schema);
    parseArgumentStrings(Arrays.asList(args));
  }

  private void parseSchema(String schema) throws ArgsException {
    for (String element : schema.split(",")) {
      if (element.length() > 0) {
        parseSchemaElement(element.trim());
      }
    }
  }

  private void parseSchemaElement(String element) throws ArgsException {
    char elementId = element.charAt(0);
    String elementTail = element.substring(1);
    
    validateSchemaElementId(elementId);
    
    if (elementTail.length() == 0) {
      marshalers.put(elementId, new BooleanArgumentMarshaler());
    } else if (elementTail.equals("*")) {
      marshalers.put(elementId, new StringArgumentMarshaler());
    } else if (elementTail.equals("#")) {
      marshalers.put(elementId, new IntegerArgumentMarshaler());
    } else if (elementTail.equals("##")) {
      marshalers.put(elementId, new DoubleArgumentMarshaler());
    } else if (elementTail.equals("[*]")) {
      marshalers.put(elementId, new StringArrayArgumentMarshaler());
    } else {
      throw new ArgsException(INVALID_ARGUMENT_FORMAT, elementId, elementTail);
    }
  }

  private void validateSchemaElementId(char elementId) throws ArgsException {
    if (!Character.isLetter(elementId)) {
      throw new ArgsException(INVALID_ARGUMENT_NAME, elementId, null);
    }
  }

  private void parseArgumentStrings(List<String> argsList) throws ArgsException {
    for (currentArgument = argsList.listIterator(); currentArgument.hasNext();) {
      String argString = currentArgument.next();
      if (argString.startsWith("-")) {
        parseArgumentCharacters(argString.substring(1));
      } else {
        currentArgument.previous();
        break;
      }
    }
  }
  
  private void parseArgumentCharacters(String argsChars) throws ArgsException {
    for (int i = 0; i < argsChars.length(); i++) {
      parseArgumentCharacter(argsChars.charAt(i));
    }
  }

  private void parseArgumentCharacter(char argChars) throws ArgsException {
    ArgumentMarshaler m = marshalers.get(argChars);
    
    if (m == null) {
      throw new ArgsException(UNEXPECTED_ARGUMENT, argChars, null);
    } else {
      argsFound.add(argChars);
      try {
        m.set(currentArgument);
      } catch (ArgsException e) {
        e.setErrorArgumentId(argChars);
        throw e;
      }
    }
  }
  
  public boolean has(char arg) {
    return argsFound.contains(arg);
  }
  
  public int nextArgument() {
    return currentArgument.nextIndex();
  }
  
  public boolean getBoolean(char arg) {
    return BooleanArgumentMarshaler.getValue(marshalers.get(arg));
  }
  
  public String getString(char arg) {
    return StringArgumentMarshaler.getValue(marshalers.get(arg));
  }

  public int getInt(char arg) {
    return IntegerArgumentMarshaler.getValue(marshalers.get(arg));
  }

  public double getDouble(char arg) {
    return DoubleArgumentMarshaler.getValue(marshalers.get(arg));
  }

  public String[] getStringArray(char arg) {
    return StringArrayArgumentMarshaler.getValue(marshalers.get(arg));
  }
}
```

주목할 점! 🤔

- 위에서 아래로 코드가 읽힌다.
- 날짜 인수나 복소수 인수 등 새로운 인수 유형을 추가하는 방법이 명백하다.

### 어떻게 짰을까?

- 깨끗한 코드를 짜려면 먼저 지저분한 코드를 짠 뒤에 정리해야 한다.
- '돌아가는' 프로그램을 만들면 다음 업무로 바로 넘어가지 말고, 점진적인 개선이 필요하다.

### 점진적인 개선

- TDD 적용 (작은 구현 → 테스트 돌리기 → 실패하면 실패한거 먼저 수정)
- 공통된 부분을 찾아 하나의 클래스로 변경
- 예외를 하나의 종류 (ArgsException)만 던지게 처리

## 결론

- 돌아가는 코드만으로는 부족하다.
- 코드는 언제나 최대한 깔끔하고 단순하게 정리하는게 좋다.

# Reference

[Clean Code](https://book.naver.com/bookdb/book_detail.nhn?bid=7390287)