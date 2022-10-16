# 9. 목처리에 대한 모범사례

**시스템 끝에서 상호 작용 검증하기**

목을 따를 때 지침 : 시스템 끝에서 비관리 의존성과 상호 작용을 검증하라.

```swift
public class UserController
    {
        private readonly Database _database;
        private readonly EventDispatcher _eventDispatcher;
        public UserController(
            Database database,
            IMessageBus messageBus,
            IDomainLogger domainLogger)
        {
            _database = database;
            _eventDispatcher = new EventDispatcher(messageBus, domainLogger);
    }
    
    public string ChangeEmail(int userId, string newEmail)
    {
        object[] userData = _database.GetUserById(userId);
        User user = UserFactory.Create(userData);
        
        string error = user.CanChangeEmail();
        if (error != null)
           return error;
        
        object[] companyData = _database.GetCompany();
        Company company = CompanyFactory.Create(companyData);
        
        user.ChangeEmail(newEmail, company);
        
        _database.SaveCompany(company);
        _database.SaveUser(user);
        _eventDispatcher.Dispatch(user.DomainEvents);

        return "OK";
    }
}
```

위 예제를 테스트하기 위해

```swift
[Fact]
public void Changing_email_from_corporate_to_non_corporate()
{
    // 준비
    var db = new Database(ConnectionString);
    User user = CreateUser("user@mycorp.com", UserType.Employee, db);
    
    CreateCompany("mycorp.com", 1, db);
    var messageBusMock = new Mock<IMessageBus>();
    var loggerMock = new Mock<IDomainLogger>();
    var sut = new UserController(
            db, messageBusMock.Object, loggerMock.Object);

    // 실행
    string result = sut.ChangeEmail(user.UserId, "new@gmail.com");

    // 검증
    Assert.Equal("OK", result);
    object[] userData = db.GetUserById(user.UserId);
    User userFromDb = UserFactory.Create(userData);
    Assert.Equal("new@gmail.com", userFromDb.Email);
    Assert.Equal(UserType.Customer, userFromDb.Type);
    
    object[] companyData = db.GetCompany();
    Company companyFromDb = CompanyFactory.Create(companyData);
    Assert.Equal(0, companyFromDb.NumberOfEmployees);

//목 상호 작용 검증
    messageBusMock.Verify(
        x => x.SendEmailChangedMessage(
            user.UserId, "new@gmail.com"),
        Times.Once);

    loggerMock.Verify(
        x => x.UserTypeHasChanged(
            user.UserId,
            UserType.Employee,
            UserType.Customer),
        Times.Once);
}
```

이 테스트는 IMEssageBus에 초점을 맞춰본다.

위 테스트의 messageBusMock의 문제는 IMessageBus인터페이스가 끝에 있지 않다는 것이다.

MessageBUst 와 IBus 인터페이스 둘 다 프로젝트 코드베이스에 속한다. 

IMessageBus는 IBus 위에 있는  Rapper로 , 도메인과 관련된 메시지를 정의한다.

둘을 합칠수 있지만 , 책임이 분리된 편이 좋기때문에 따로가 좋다.

<img width="603" alt="스크린샷 2022-10-16 오후 4 09 37" src="https://user-images.githubusercontent.com/78361650/196023922-7c9b963a-4e7c-4bdd-9c5e-423f5b37f5fe.png">

**목을 스파이로 대체하기**

스파이와 목의 차이: 스파이는 수동으로 작성하는 반면, 목은 프레임워크를 도움받아 생성한다.

시스템 끝에 있는 클래스의 경우 스파이가 목보다 낫다.

목은 비관리 의존성에만 해당하며 컨트롤러만 이러한 이존성을 처리하는 코드이기 때문에 통합 테스트에서 컨트로러를 테스트할 때만 목을 적용해야 한다.

테스트에서 사용된 목의 수는 관계 없이 의존성 수에 따라 달라진다.

목이 예상되는 호출이 있는지 없는지 확인하라.

보유타입만 목으로 처리하라.기본타입 대신 해당 어댑터를 목으로 처리하라.
