# Objective-C

## X-code Error
- Metal API Validation Enabled
    - Scheme > Edit Scheme... > Run > Diagnostics > Metal API Validation.

## 기본정보
- 작명규칙
    - 클래스 : 대문자로 시작하는 명사형
    - 인스턴스/멤버면수 : 소문자로 시작하는 명사형
    - 메소드/함수 : 소문자로 시작하는 동사형
- 상속으로 메서드를 제거하거나 삭제할 순 없지만 '재정의'하여 메서드의 정의를 변경할 수는 있다.
    - 메소드를 호출 할 경우 해당 클래스에서 메서드를 찾은 후 찾지 못한 경우 상위 클래스에서 메서드를 찾아 호출하기 때문
- objc 장점
    - 동적 타이핑이 가능하다. : 객체가 속한 클래스를 알아내는 단계를 프로그램이 실행될 때로 미룬다.
    - 동적 바인딩이 가능하다. : 객체에 호출되는 실제 메서드를 알아내는 시기를 프로그램 실행 중으로 미룬다.
- '객체가 아니다'란
    - 미리 정의된 사이즈가 존재하기 때문에 힙 메모리에 동적할당 되지 않는다.
    - 즉, 포인터로 변수에 접근하지 않아도 된다.  

## nil
- 아무것도 가르키지 않는 상태

## NS 원시형 
- NSInteger
- NSUInteger

## 객체, 구조체 반환
- 객체
```objc
-(NSString *)uppercaseString;
```
- 구조체
```objc
-(NSString)uppercaseString;
```
- EX
```objc
NSString *str = @"Hello ObjC";
NSUInteger len = [str length];
NSLOG(@"Length of str is : %lu",len);   // @문자열 == NSString 객체
```

## 객체생성
```objc
// Origin
[["ClassName" alloc] init]
// ex
NSObject *obj = [[NSObject alloc] init];
NSLog(@"obj : %@", obj);    // 주소 출력
```

## 파라미터가 정의된 함수(메소드) 호출
```objc
NSString *str = @"1";
NSComparisonResult result = [str compare:@"9"];
NSLog(@"Result : %ld", result);
```

## 클래스

### 클래스 선언
- 형식
```objc
@interface ClassName : SuperClass
@end
```
- Test.h
```objc
// @interface에선 상속, 변수선언, 함수선언등의 작업이 이루어 진다.
@interface Rectangle : NSObject{
    int height;
}
-(int)size;
@end
```

### 클래스 구현
- 형식
```objc
@implementation ClassName
@end
```
- Test.m
```objc
// @implementation에선 선언했던 함수의 구현이 이루어 진다.
#import "Test.h"
@implementation Rectangle{
    int width;
}
-(int)size{
    return width*height;
}
@end
```

## 인스턴스 메소드 VS 클래스 메소드

||인스턴스 메소드|클래스 메소드|
|------|---|---|
|구별하기|- 기호로 시작|+ 기호로 시작|
|메세지 리시버|객체|클래스|
|멤버 변수 접근|가능|불가능|
|선언 예제|-(void)method1;|+(void)method2;|
|사용 예제|[obj method1]|[Class method2]|

### 인스턴스 메소드
- 인스턴스 메소드 선언
```objc
@interface ClassName : SuperClass
-(void)instanceMethod;
@end
```
- 인스턴스 메소드 사용, 객체 생성 필요
```objc
Rectangle *rect = [[Rectangle alloc] init];
int area = [rect size];
```
- 멤버변수 접근 가능

```objc
-(int)size{
    return width*height;
}
```

- __nullable

```objc
-(void)updateTable:(NSInteger)index success:(NSInteger)success port:(NSString* __nullable)port time:(NSString* __nullable)time fileSize:(NSString* __nullable)fileSize groupName:(NSString* __nullable)groupName contents:(NSString* __nullable)contents;
```

### 클래스 메소드
- 클래스 메소드 선언
```objc
@interface MyClass : NSObject
+(void)classMethod;
@end
```
- 클래스 메소드 사용, 객체 생성 불필요
```objc
[MyClass classMethod];
```
- 멤버변수 접근 불가 ERROR
```objc
+(int)size{
    return width*height;    // ERROR
}
```

## 초기화 메서드
```objc
-(id)init{
    self = [super init];
    if(self){   // 초기화가 불가능할 경우 nil을 리턴하기 때문
        firstNumber = ((int)random() % 100 );
        secondNumber = ((int)random() % 100 );
    }
    return self;
}
```
- 만약 새로운 init 메서드를 작성하고 싶다면
```objc 
-(id)init{
    return [self initWithEntryDate:[NSdate date]];
}
-(id)initWithEntryDate:(NSDate *)theDate{
    self = [super init];
    if(self){   
        entryDate = theDate;
        firstNumber = ((int)random() % 100 );
        secondNumber = ((int)random() % 100 );
    }
    return self;
}
```

## 프로퍼티(인스턴스 변수 접근)
- interface 영역
```objc
@interface Example : NSObject{
   float value;
}
@property (copy, nonatomic) (readwrite) float value;
@end
```
- implementation 영역
```objc
@implementation Example
@synthesize value;  // 자료형을 제외한 변수명만 작성한다.
@end
```
- 사용
```objc
int mian(){
    Example *ex = [[Example alloc]init];
    ex.value = 3;
    
    return 0;
}
```
- 옵션
    - 값 설정 방법
        - assign : 값을 복사 설정, 기본값, 프리미티브 자료형에 적합
        - retain : 멤버변수가 인스턴스 포인터 값을 가질 때, 오너십을 위해 retain 증가
        - copy : 멤버변수가 인스턴스 포인터 값을 가질 때, 인스턴스 복사
        - strong : 강한참조, 포인터 값이 설정되어 있는 동안 메모리 해제 방지
        - weak : 약한참조, assign과 비슷하지만 객체가 메모리에서 할당되면 해당 객체를 가리키던 프로퍼티가 nil로 
    - 동시성
        - atomic : getter/setter는 선형 스레드를 통해 독립 실행
        - nonatomic : getter/setter는 다중 스레드를 통해 독립 실행
##  self
- 자기 자신의 인스턴스 메소드 호출하기
```objc
// EX
@implementation Minus

-(void)minusAB{
    NSLog(@"A - B : %i", a-b);
    [self plusAB];
}
-(void)plusAB{
    NSLog(@"plus AB!");
}

@end
```

## @class 지시어
```objc
#import XYPoint.h == @class XYPoint
```

## 동적바인딩과 id형
- id는 어떤 클래스 객체든 저장하여 사용할 수 있다.
```objc
id dataValue;   // *를 붙이지 않는다.
Fraction *f1 = [[Fraction alloc]init];
Complex *c1 = [[Complex alloc]init];

[f1 setTo:2 over:5];
[c1 setReal:10.0 andImaginary:2.5];

dataValue = f1;
[dataValue print];

dataValue = c1;
[dataValue print];
```

## @selector

## @try
```objc
// Ex
int main(){
    NSAutoreleasePool * pool = [[NSAutoreleasePool alloc]init];
    Fraction *f = [[Fraction alloc]init];
    
    @try {  // try실패시 프로그램 종료가 아닌 catch 실행
        [f noSuchMethod];
    }

    @catch (NSException * e) {
        NSLog(@"Error: %@%@", [e name], [e reason]);
    }
    @finally {
        NSLog(@"Finally executes no matter what");
    }
    
    [f release];
    [pool drain];
    return 0;
```
- @finally : @try 블록에서 예외의 싱행 유무와 상관없이 실행 할 코드를 작성할 수 있다.
- @throw : 직접 만든 예외를 던질 수 있다.

## 인스턴스 변수의 범위
- @protected : 어떤 클래스에서 인스턴스 변수가 정의되었을 때, 그 클래스와 그 서브클래스에 정의된 메서드는 이 인스턴스 변수에 바로 접근할 수 있다. 이것이 기본값 이다.
- @private : 클래스에 정의된 메서드는 인스턴스 변수에 바로 접근할 수 있지만, 서브클래스의 메서드는 바로 접근할 수 없다.
- @public : 인스턴스 변수가 정의된 클래스와 그 밖에 클래스 그리고 모듈에 정의된 메서드라면, 인스턴스 변수에 바로 접근할 수 있다.
- @package : 64비트 이미지의 경우, 그 클래스를 구현하는 이미지 안에서든 어디든 인스턴스 변수에 접근할 수 있다.

## 식별자
- extern
    - 전역변수를 다른 프로그램 파일에서도 함께 사용할 수 있도록 해주는 선언 방법 == 외부변수
```objc
extern int gMoveNumber;
```
- static
    - 정적변수
```objc
static int gGlobalVar = 0;
```
- const
    - 값이 변하지 않는 변수 == 상수
- volatile
    - 값이 변할 예정인 변수

## 열거 데이터 형
```objc
enum direction{up, down, left = 10, right };
// up == 0
// down == 1
// left == 10
// right == 11
```

## typedef
```objc
typedef Number *NumberObject;   // typedef 기존이름 새이름
// Ex
typedef int Counter;
Counter j;  // == int j
```

## 카테고리
- 클래스 정의를 그룹짓거나, 연관된 메서드를 카테고리로 쉽게 모듈러화할 수 있게 해준다. 또한 원본 소스코드에 접근하거나 서브클래스 생성 없이 현존한느 클래스의 정의를 쉽게 확장하는 방법이다.
```objc
// 정의부
//NSString+Reorder
#import <Foundation/NSString.h>
@interface NSString (Reorder)
    -(NSString *)reversedString;
@end
// 구현부
#import "NSString+Reorder.h"
#import "NSString+PathComp.h"
@implementation NSString
-(NSString *)reversedString
{
    ...
}
@end
```
- 카테고리는 클래스에 인스턴스 변수를 추가할 수 없으며 메소드만 추가할 수 있습니다. 대신 클래스 메소드와 인스턴스 메소드를 모두 선언할 수 있습니다.

## 익스텐션(익명 카테고리)
- 익스텐션을 사용하면 클래스의 @interface 부분을 나누어 사용할 수 있지만 @implementation 부분은 분리할 수 없습니다. 일부 메소드의 프로토타입을 인터페이스로 공개하지 않게 구성할 때 유용합니다.
```objc
// 익스텐션은 @interface에 카테고리 이름을 지정하지 않으며 원래 클래스의 @implementation 부분에 구현해야 합니다.
// Ship.h
#import <Foundation/Foundation.h>
#import "Person.h"
 
@interface Ship : NSObject
 
@property (strong, readonly) Person *captain;
 
-(id)initWithCaptain:(Person *)captain;
 
@end

// Ship.m
#import "Ship.h"
 
// The class extension.
@interface Ship()
 
@property (strong, readwrite) Person *captain;
 
@end
 
 
// The standard implementation.
@implementation Ship
 
@synthesize captain = _captain;
 
-(id)initWithCaptain:(Person *)captain {
    self = [super init];
    if (self) {
        // This WILL work because of the extension.
        [self setCaptain:captain];
    }
    return self;
}
 
@end
```

## 프로토콜
- “선언만 되고 구현되지 않은” 메소드
    - 다른 객체가 구현해주면 되는 메소드를 선언
    - 클래스를 숨기고 인터페이스만 선언하고자 할 때
    - 상속관계가 아니지만 비슷한 인터페이스를 만들고자 할 때
```objc
@protocol NSCopying <protocolName>
-(id)copyWithZone:(NSZone*)zone;
// @required : 반드시 구현해야 하는 메소드
// @optional : 구현해도 되고 안해도 되는 메소드
@end
////////////////////////////////
@interface AddressBook: NSObject <NSCopying, ...>
```
- 객체가 프로토콜에 따르는지 알아보는 방법
```objc
id currentObject;
    ...
if ([currentObject conformsToProtocol: @protocol (Drawing)] == YES){
    // currentObject에 존재하는 프로토콜 인스턴스에 메세지를 보낸다.
    ...
}
```
- 프로토콜을 따르는 변수 표기
```objc
id <Drawing, NSCopying> myDocument;
```

## 전처리기
- #define
- #연산자 ##연산자
- #import
- #ifdef, #endif, #else, #ifndef
- #if, #elif
- #undef

## 구조체
```objc
strct date  [
    int month;
    int day;
    int year;
}Date;

//strct date today;
Date today;
today.day = 21;

## 공용체
```objc
union mixed{
    char c;
    float f;
    int i;
};
```
## DataSource(보여지는 것 == MVC 중 View에 해당)
- Data를 받아 View에 그려주는 역할


## Delegate(행해지는 것 == MVC 중 Control에 해당)
- 하나의 객체가 모든 일을 처리하는 것이 아니라 처리해야 할 일부를 다른 객체에 넘기는 것
- 모든 메서드를 구현할 필요가 없다. 하지만 메서드를 구현하면 그 메서드를 호출한다.


# Foundation

## NSNumber (숫자 객체)
- Foundation.NSValue.h
```objc
NSNumber *myNumber;
NSInteger myInt;    // NSInteger는 객체가 아니다
myNumber = [NSNumber numberwithInteger:100];
myInt = [intNumber intergerValue];  // NSInteger는 객체가 아니다
```

## NSString, NSMutableString(스트링 객체)
- Foundation.NSString.h
```objc
NSString *str = @"Programmin is fun!";  // 수정 불가능한 객체
NSMutableString *mstr;  // 수정 가능한 객체
NSLog(@"%@", str);

NSString *strInt = @"123";
NSLog(@"%d", [strInt intValue]);    // str to int

int intStr = 123;
NSLog(@"%@", [NSString stringWithFormat:@"%d", intStr]);    // int to str

// 중간 글자 
[lineContents substringWithRange:NSMakeRange(1, [lineContents length] - 2)];
```

## NSArray(배열 객체)
- Foundation.NSArray.h

|메소드|내용|
|------|---|
|count|자료의 갯수를 리턴한다.|
|initWithObjects:|값을 설정한다|
|objectAtIndex:|주어진 index의 위치한 객체를 리턴한다.|
|containsObject:|주어진 객체가 자료구조 안에 있는지 알아본다.|
|@[...]|[[NSArray alloc]initWithObject:..., nil]에 대응된다.|
|-(BOOL)isEqualToArray:(id)anObject :|배열의 요소 개수와 모든 멤버가 동일하면 YES 아니면 NO를 리턴|
|-(id)firstObjectCommonWithArray:(NSArray *)otherArray :|일치하는 첫번째 인스턴스 리턴|

```objc
NSArray *monthNames = [NSArray arrayWithObjects:
    @"January", @"February", @"March" ... , nil];   // nil은 배열의 끝을 알린다.
for(int i = 0; i < [monthNames count]; ++i){
    NSLog(@"2i  %@", i+1, [monthNames objectAtIndex : i]);
```

- Dictionary가 들어있는 Array Key 값을 기준으로 정렬하기

```objc
    NSSortDescriptor * descriptor = [[NSSortDescriptor alloc] initWithKey:@"Key_Name" ascending:YES(NO)];
    Array = [Array sortedArrayUsingDescriptors:@[descriptor]];
```
```objc
// 정렬
_sertList = [_sertList sortedArrayUsingComparator:^(id a,id b){
       return [a compare:b];
    }];
```

## NSMutableArray
|메소드|내용|
|------|---|
|addObject:|끝에 자료를 추가한다.|
|removeObject:|주어진 객체를 찾아서 제거한다.|
|exchangeObjectAtIndex:withObjectAtIndex|객체를 교환한다.|
|replaceObjectAtIndex:withObject|주어진 위치의 객체를 새로 입력되는 객체로 교체한다.|
|sortUsingComparator|익명함수를 비교자로 사용하여 자료를 정렬한다|
|-(void)insertObject:(id)anObject atIndex:(NSUinter)index|index 번째에 anObject 추가|

```objc
@property NSMutableArray *tableArr;

_tableArr = [NSMutableArray arrayWithObjects:@"Process1",@"Process2",@"Process3",@"Process4",@"Process5",@"Process6", nil]; // data init

[_testArray addObject:[NSString stringWithFormat:@"Process%lu", _testArray.count+1]];   // data add
[_testArray removeLastObject];  // last objct del

NSMutableArray *primes = [NSMutableArray arrayWithCapacity:20];

// Array에 Dictionary 
NSDictionary *errorCodeDic= [NSDictionary dictionaryWithObjectsAndKeys:
                                 @"000", @"000 Error",
                                 @"001", @"001 Error",nil];

    _errorCodeList = [NSMutableArray arrayWithObjects:errorCodeDic,nil];
```

## NSMutableDictionary(딕셔너리 객체)
- Foundation.NSDictionary.h
- Key는 객체로 얻는다.
- Value가 nil이여선 안된다.
- 딕셔너리 객체는 순서가 없다.

|메소드|내용|
|------|---|
|setObject:forKey:|key, value를 추가한다. 해당 key가 존재한다면 값이 수정된다.|
|objectForKey:|해당 key를 찾아 객체를 리턴한다. key를 찾지 못한다면 nil을 리턴한다.|
|count|자료의 갯수를 리턴한다.|
|removeObjectForKey:|해당하는 key를 찾아 그 객체와 같이 제거한다.|
|removeAllObjects|모든 자료를 제거한다.|
|allKeys|모든 key를 NSArray애 담아 리턴한다.|
```objc
NSMutableDictionary *glossary = [NSMutableDictionary dictionary];
[glossary setObject: @"A class" forKey:@"abstract class"];
```

## NSSet, NSMutableSet(세트 객체)
- Foundation.NSSet.h
- 유일한 객체들의 모임

## NSTableView
- view base, cell base 조심
```objc
@property (weak) IBOutlet NSTableView *tableView;

[_tableView reloadData];    // table reload

- (instancetype)initWithWindowNibName:(NSNibName)windowNibName {    // table init
    self = [super initWithWindowNibName:windowNibName];
    if(self) {
        NSLog(@"init");
//        self.testArray = @[@"Process1",@"Process2",@"Process3",@"Process4",@"Process5",@"Process6"];
    }
    return self;
}

#pragma mark tableview delegate
- (NSInteger)numberOfRowsInTableView:(NSTableView *)aTableView1 // table row count
{
    NSLog(@"datasource count");
    return [_testArray count];
}

- (id)tableView:(NSTableView *)aTableView objectValueForTableColumn:(NSTableColumn *)aTableColumn row:(NSInteger)rowIndex   // table data set?
{
    NSLog(@"datasource item");
    return [_testArray objectAtIndex:rowIndex];
}
```

## NSOutlineView


## NSFileManager(파일 다루기)
- NSData(버퍼생성)
- NSPathUtilities.h(경로 다루기)
- NSProcessInfo
- NSFileHandle(기본 파일 작업)

## 메모리 관리
- PASS

## Copy, mutalbeCopy
```objc
NSMutableArray *dataArray = [NSMutableArray alloc]init];
NSMutableArray *dataArray2 = [NSMutableArray alloc]init];
dataArray2 = dataArray;     // 얕은 복사
dataArray2 = [dataArray mutalbeCopy];   // 깊은 복사
```

## 아카이빙
- archive : 객체를 바이트로, unarchive : 바이트를 객체로
- 하나 이상의 객체를 나중에 복구할 수 있는 형식으로 저장하는 절차

### NSCoder
- 바이트 스트림을 추사화한 것


|메서드|내용|
|---|---|
|-(id)initWithCoder:(NSCoder *)coder;|coder에서 데이터를 읽어오고 이 데이터를 객체의 인스턴스 변수들로 저장한다.|
|-(void)encodeWithCoder:(NSCoder *)coder;|인스턴스 변수들의 값을 읽어 코더에 그 값을 저장한다.|


### NSCoding
|메서드|내용|
|---|---|
|-(void)encodeObject:(id)anObject forKey:(NSString *)aKey|anObject를 기록하고 해당 객체에 대한 키로 aKey를 설정한다.|
|||

### XML 프로퍼치 리스트로 아카이빙하기
- 사용하는 객체가 NSString, NSDictionary, NSArray, NSDate, NSData, NSNumber라면, 이 클래스에 구현된 wirteToFile:atomically: 메서드를 사용하여 데이터를 파일에 기록할 수 있다.

### NSKeyedUnarchiver로 아카이빙하기
- 데이터의 객체를 읽어오기

### NSKeyedArchiver


### 인코딩 메서드와 디코딩 메서드 작성하기

## 시간 관리하기
```objc
-(void)startStopwatch{
    NSTimeInterval seconds = [NSDate timeIntervalSinceReferenceDate];
    _processRunTimeToMilliseconds = seconds*1000;
}

-(void)stopStopwatch{
    NSTimeInterval seconds = [NSDate timeIntervalSinceReferenceDate];
    _processRunTimeToMilliseconds = (seconds*1000) - _processRunTimeToMilliseconds;
}

-(NSString *)returnRealTime{
    NSDateFormatter * today = [[NSDateFormatter alloc] init];
//    [today setDateFormat:@"yyyy-MM-dd-HH-mm-ss"];
    [today setDateFormat:@"HH:mm:ss"];
    NSString * date = [today stringFromDate:[NSDate date]];
    return date;
}

-(NSString *)returnDateTime{
    NSDateFormatter * today = [[NSDateFormatter alloc] init];
    [today setDateFormat:@"yyyy_MM_dd_HH_mm_ss"];
    NSString * date = [today stringFromDate:[NSDate date]];
    return date;
}
```

## 파일 관리하기

```objc
-(void)openFolder:(NSString*)path{
    _fileURL = [NSURL fileURLWithPath:path];
    
    NSURL *folderURL = [_fileURL URLByDeletingLastPathComponent];
    [[NSWorkspace sharedWorkspace] openURL: folderURL];
}

-(BOOL)chekcfileExists:(NSString*)fileName fileType:(NSString*)fileType{
    NSString *filePath =  [[NSBundle mainBundle] pathForResource:fileName ofType:fileType];
    BOOL fileExists = [[NSFileManager defaultManager] fileExistsAtPath:filePath];
   
    return fileExists;
}

-(NSString*)readFile:(NSString*)filePath fileType:(NSString*)fileType{
        
    NSString *filepath = [[NSBundle mainBundle] pathForResource:filePath ofType:fileType];
    NSError *error;
    
    NSString *fileContents = [NSString stringWithContentsOfFile:filepath encoding:NSUTF8StringEncoding error:&error];

    if (error){
        NSLog(@"Error reading file: %@", error.localizedDescription);
        return nil;
    }
    
    return fileContents;

}

-(NSArray*)fileContentToArray:(NSString*)fileContent{

    // Separate data by line
    NSArray* allLinedStrings =
          [fileContent componentsSeparatedByCharactersInSet:
          [NSCharacterSet newlineCharacterSet]];
    
    return allLinedStrings;
}

-(NSString *)checkFileSize:(NSString*)path time:(float)time{
    NSString *filePath = [path stringByExpandingTildeInPath];
    NSURL *fileURL = [NSURL fileURLWithPath:filePath];
    NSNumber *fileSizeValue = nil;
    NSError *fileSizeError = nil;
    [fileURL getResourceValue:&fileSizeValue
                       forKey:NSURLFileSizeKey
                        error:&fileSizeError];
//    if (fileSizeValue) {
//        NSLog(@"value for %@ is %@ byte", fileURL, fileSizeValue);
//    }
//    else {
//        NSLog(@"error getting size for url %@ error was %@", fileURL, fileSizeError);
//    }
    
    float byteFileSize = [fileSizeValue intValue];
    if(time <= 0){
        byteFileSize = byteFileSize;
    }else{
        byteFileSize = byteFileSize / (time*0.001);
    }
    
    // byte data to KB or MB
    NSString * fileSize;
    if(byteFileSize < 1500000){
        byteFileSize = byteFileSize * 0.001;
        fileSize = [NSString stringWithFormat:@"%6.1fKB/s", byteFileSize];
    }else{
        byteFileSize = byteFileSize * 0.001;
        byteFileSize = byteFileSize * 0.001;
        byteFileSize = byteFileSize / time;
        fileSize = [NSString stringWithFormat:@"%6.1fMB/s", byteFileSize];
    }
    
    return fileSize;
}

```

## Base64
```objc
@implementation Base64
-(NSString*)Base64Encoding:(NSString *)plainText{

    NSData *plainData = [plainText dataUsingEncoding:NSUTF8StringEncoding];
    NSString *encodedText = [plainData base64EncodedStringWithOptions:0];

    NSLog(@"encodedText : %@", encodedText);
    
    return encodedText;

}

-(NSString*)Base64Decoding:(NSString *)encodedText{

    NSData *encodedData = [[NSData alloc]initWithBase64EncodedString:encodedText options:0];
    NSString *decodedText = [[NSString alloc] initWithData:encodedData encoding:NSUTF8StringEncoding];

    NSLog(@"decodedText : %@", decodedText);

    return decodedText;
}
@end
```
## 시그널 (SIGNAL)
* 의미전달에 사용되는 대표적인 방법은 메세지와 신호입니다. 메세지는 여러가지 의미를 갖을 수 있지만 복잡한 대신 , 신호는 1:1로 의미가 대응되기 때문에 간단합니다. 

```c
// 원형
void (*signal(int signum, void (*handler)(int)))(int);

// 참고 (https://reakwon.tistory.com/46)
// 1. SIGHUP : 터미널과 연결이 끊겼을 때 발생합니다. 기본적인 처리는 프로세스가 종료되는 것입니다.
// 2. SIGINT : 인터럽트가 발생했을때 발생합니다. 기본으로 프로세스가 종료됩니다.
// 9. SIGKILL : 프로세스를 무조건 종료합니다. 절대 무시할 수 없으며 제어될 수도 없습니다.
// 11. SIGSEGV : 프로세스가 잘못된 메모리를 참조했을 때 발생합니다. 기본 동작은 코어덤프를 남기고 종료합니다.
// 19 SIGSTOP : 프로세스를 중단시킵니다. 종료한 상태는 아닙니다. 이 신호 역시 제어될 수 없습니다.
```
