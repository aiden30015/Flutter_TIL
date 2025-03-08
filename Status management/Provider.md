# Provider
`InheritedWidget`을 더 쉽게 사용하고 보다 쉽게 재사용 할 수 있도록 만들었다.           

플러터 전용으로 구축된 라이브러리로, 의존성을 주입하여 상태의 변화를 효과적으로 관리하고 UI에 쉽게 반영할 수 있게 해준다.         

데이터의 흐름을 위젯 트리를 통해 쉽게 제어할 수 있도록 설계되었다           

구글에서 추천하는 상태관리이다.         
원래 구글은 `Bloc`  패턴을 제공했으나 **비교적 사용이 어렵고** 단순한 로직을 구성하려해도도 최소 4개의 클래스를 만들어야 했다.

상태 관리를 설정하고 사용하기 위한 보일러플레이트 코드가 반복되어 많이 필요할 수 있다.
## 사용법
플러터 패키지를 설치해준다.
`Provider` 상태관리 파일을 따로 만들어주고          
클래스를 선언한뒤 `ChangeNotifier`로 확장해주면 된다.          

**예제코드**
```dart
class CounterProvider extends ChangeNotifier {
  int _count = 0; // 상태

  int get count => _count;

  add() {
    _count++; //상태 변경
    notifyListeners(); // 상태 변경 된 것을 알림
  }

  decrese() {
    _count--; //상태 변경
    notifyListeners(); // 상태 변경 된 것을 알림
  }
}
```
`notifiListeners`를 통해 상태가 변했음을 알린다.               

이제 `Provider` 선언만 해주면 된다.
           
**예제코드**

**단일 Provider**
```dart
MaterialApp(
  title: 'Flutter Provider Demo',
  home: ChangeNotifierProvider(
    create: (BuildContext context) => CounterProvider(),
    child: Home(),
  ),
);
```
`ChangeNotifierProvider`는 한개의 `Provider`만 선언이 가능하다.
그러면 여러개를 선언하려면 `MultiProvider`를 사용하면 된다.            

**예제코드**
```dart
MaterialApp(
  title: 'Flutter Provider Demo',
  home: MultiProvider(
    providers: [
      ChangeNotifierProvider(
        create: (BuildContext context) => CounterProvider(),
      ),
      ChangeNotifierProvider(
        create: (BuildContext context) => TodoProvider(),
      ),
    ],
    child: Home(),
  ),
);
```
이런식으로 상태관리를 기능별로 나누어서 사용할때 사용하면 된다.          
그럼 이제 선언도 했으니 사용을 해야한다          

**예제코드**

```dart
@override
Widget build(BuildContext context) {
  final counter = Provider.of<CounterProvider>(context, listen: false);
  return Center(
    child: Text(
      counter.count.toString(),
    ),
  );
}
```
`counter` 변수를 선언한 구문에서 `listen`이라는 생성자가 보인다.          
`listen`이 `true`라면 상태가 변경되면 `build` 함수를 실행시킨다        
만약 `false`라면 상태가 변경되어서 rebuild 시키지 않는다.        
그래서 `false`일 경우는 `onPressed`와 같이 특정 함수를 필요로 하는 경우 쓰인다.         

*예제코드**
```dart
return Center(
  child: Consumer<CounterProvider>(
    builder: (context, provider, child) {
      return Column(
        children: [
          Text(
            provider.count.toString(),
          ),
          Container(child: child),
        ],
      );
    },
    child: const Text('hello'),
  ),
);
```
`Consumer`를 사용했는데 이건 `builder`를 통해 return한 위젯만 상태가 변경되면 다시 빌드 한다.         
`child`인 `Text('hello'),`이 위젯은 상태가 변경되어도 리빌드 되지 않는다.