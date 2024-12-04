# HTTP 통신/REST API 호출하기
HTTP 통신으로 API 호출하는 방법
## HTTP 의존성 추가하기
`pubspec.yaml` 파일에 `https: ^버전` 추가
## MODEL로 API 불러오기
```
PiscumPhotoModel.fromJson(Map<String, dynamic> json)
      : id = json["id"],
        author = json["author"],
        width = json["width"],
        height = json["height"],
        url = json["url"],
        downloadUrl = json["download_url"];
factory PiscumPhotoModel.fromJson(Map<String, dynamic> json) {
    return PiscumPhotoModel(
      id: json["id"],
      author: json["author"],
      width: json["width"],
      height: json["height"],
      url: json["url"],
      downloadUrl: json["download_url"],
    );
  }
```
여기 두개의 동일 한 클래스가 있지만 형태가 다릅니다.         
위에 있는 친구는 값을 초기화 합니다. 성능이 좀더 좋습니다.         

아래 있는 친구는 `factory`를 사용해 값을 **반환**하고 있습니다. 이친구는 에러처리 및 기본 값을 설정할 수 있습니다(유연성이 뛰어나다) 하지만 성능 면에선 뒤떨어질 수 있습니다.
## 예제 코드
```
class PiscumPhotoModel {
  final String id;
  final String author;
  final int width;
  final int height;
  final String url;
  final String downloadUrl;

  PiscumPhotoModel({
    required this.id,
    required this.author,
    required this.width,
    required this.height,
    required this.url,
    required this.downloadUrl,
  });
}
// 모델 코드
// 여기에는 위에 json 변환 코드
```
이 코드는 모델 클래스 코드이다.
```
body: NotificationListener<ScrollUpdateNotification>(
              onNotification: (ScrollUpdateNotification notification) {
                state.scrollListerner(notification);
                return false;
              },
              child: ListView.builder(
                  itemCount: state.photos.length,
                  itemBuilder: ((context, index) {
                    return Padding(
                      padding: const EdgeInsets.symmetric(
                          horizontal: 20, vertical: 8),
                      child: Column(
                        children: [
                          SizedBox(
                            child: Row(
                              crossAxisAlignment: 
                              CrossAxisAlignment.start,
                              children: [
                                SizedBox(
                                  width:
                                      MediaQuery.of(context).size.width * 0.2,
                                  height:
                                      MediaQuery.of(context).size.width * 0.2,
                                  child: ClipRRect(
                                    borderRadius: BorderRadius.circular(12),
                                    child: Image.network(
                                      state.photos[index].downloadUrl,
                                      fit: BoxFit.cover,
                                      frameBuilder: (BuildContext context,
                                          Widget child,
                                          int? frame,
                                          bool wasSynchronouslyLoaded) {
                                        return Container(
                                          decoration: BoxDecoration(
                                            borderRadius:
                                                BorderRadius.circular(12),
                                            color: const Color.fromRGBO(
                                                91, 91, 91, 1),
                                          ),
                                          child: child,
                                        );
                                      },
                                      loadingBuilder: (BuildContext context,
                                          Widget child,
                                          ImageChunkEvent? loadingProgress) {
                                        if (loadingProgress == null) {
                                          return child;
                                        }
                                        return Container(
                                          decoration: BoxDecoration(
                                            borderRadius:
                                                BorderRadius.circular(12),
                                            color: const Color.fromRGBO(
                                                91, 91, 91, 1),
                                          ),
                                          child: const Center(
                                            child: CircularProgressIndicator(
                                              color: Colors.amber,
                                            ),
                                          ),
                                        );
                                      },
                                    ),
                                  ),
                                ),
                                const SizedBox(width: 12),
                                Column(
                                  crossAxisAlignment: CrossAxisAlignment.start,
                                  children: [
                                    _content(
                                        url: state.photos[index].url,
                                        title: "ID : ",
                                        content: state.photos[index].id),
                                    _content(
                                        url: state.photos[index].url,
                                        title: "Author : ",
                                        content: state.photos[index].author),
                                    _content(
                                        url: state.photos[index].url,
                                        title: "Width : ",
                                        content:
                                            "${state.photos[index].width}"),
                                    _content(
                                        url: state.photos[index].url,
                                        title: "Height : ",
                                        content:
                                            "${state.photos[index].height}"),
                                  ],
                                )
                              ],
                            ),
                          ),
                          if (state.photos.length - 1 == index &&
                              state.isAdd) ...[
                            const SizedBox(
                              height: 100,
                              child: Center(
                                  child: CircularProgressIndicator(
                                color: Colors.deepOrange,
                              )),
                            ),
                          ],
                        ],
                      ),
                    );
                  })),
            ),
GestureDetector _content({
    required String title,
    required String content,
    required String url,
  }) {
    return GestureDetector(
      onTap: () async {
        if (await canLaunchUrlString(url)) {
          await launchUrlString(url, mode: LaunchMode.externalApplication);
        }
      },
      child: Row(
        children: [
          Text(
            title,
            style: const TextStyle(
              fontSize: 14,
              fontWeight: FontWeight.bold,
            ),
          ),
          Text(
            content,
            style: const TextStyle(
                fontSize: 14, color: Color.fromRGBO(215, 215, 215, 1)),
          ),
        ],
      ),
    );
  }
```
UI 코드이다.           
`GestureDetector`는 사용자 동작을 감지하는 위젯이다.          
`onPressed`과 `onTap` 대신에 쓸 수 있으며 더 많은 기능을 제공한다.
```
class HttpProvider extends ChangeNotifier {
	List<PiscumPhotoModel> photos = [];
	int currentPageNo = 1;
	bool isAdd = false;
    ...
  }
```
상태관리로 `Provider`를 사용했다.          
```
 Future<void> started() async {
    await _getPhotos(); // getPhotos() 함수 호출
  }
```
페이지 진입시 API 호출을 통해서 데이터를 세팅하기 위한 함수이다.
```
Future<void> _getPhotos() async {
    List<PiscumPhotoModel>? _data = await _fetchPost(pageNo: currentPageNo);
    photos = _data;
    currentPageNo = 2;
    logger.e(currentPageNo);
    notifyListeners();
  }
```
무한 스크롤 코드도 있던데 관련이 없어서 뺏다.             

참고자료:            
https://velog.io/@tygerhwang/Flutter-Http-%ED%86%B5%EC%8B%A0-Rest-API-%ED%98%B8%EC%B6%9C%ED%95%98%EA%B8%B0-1%ED%8E%B8-Http