---
layout: single
title: "[컴퓨터 비전] OpenCV 주요 함수 설명"
categories: ComputerVision
tag: [Computer Vision, OpenCV, AI]
toc: true
---

## OpenCV 주요 함수 설명

### imread() 함수

 **imread()** 함수는 영상 파일을 불러올 때 사용하는 함수이다. **imread()**함수의 원형은 아래와 같다.

 ```cpp
 Mat imread(const String& filename, int flags = IMREAD_COLOR);
 // filename : 불러올 영상 파일 이름
 // flags : 영상 파일 불러오기 옵션 플래그, ImreadModes 열거형 상수를 지정한다.
 // 반환값 : 불러온 영상 데이터(Mat 객체)
 ```

 **imread()** 함수는 fiilename 영상 파일을 불러와 Mat 객체로 변환하여 반환한다. filename 인자의 타입으로 지정된 String은 std::string의 이름 재정의이다. **filename**에 "lenna.bmp"처럼 **파일 이름만 지정하면 프로그램 작업 폴더에 위치한 "lenna.bmp" 파일을 불러온다.** 
 
 만약 다른 폴더의 파일을 불러오려면 절대 경로 또는 상대 경로 형식으로 파일 위치를 지정해야 한다. 예를 들어 C 드라이브 최상위 폴더에 lenna.bmp 파일이 존재한다면 "C:\\lenna.bmp" 문자열을 filename 인자로 설정한다.

 **imread()**함수는 **BMP, JPG, TIF, PNG**와 같이 널리 사용되는 대부분의 영상 파일 형식을 지원한다. 만약 filename으로 지정된 파일이 존재하지 않거나 잘못된 형식의 영상 파일이라면 비어 있는 Mat 객체를 반환한다. 그러므로 **imread()**함수를 사용한 후에는 Mat 클래스의 멤버 함수 **Mat::empty()**를 이용하여 **Mat 객체가 제대로 생성되었는지를 확인**하는 것이 좋다.

 **imread()** 함수의 두 번째 인자 flags는 영상 파일을 불러올 때 사용할 컬러 모드와 영상 크기를 지정하는 플래그이다. flags 인자에는 ImreadModes 열거형 상수를 지정할 수 있다. flags 인자는 기본값으로 IMREAD_COLOR가 지정되어 있기 때문에 imread() 함수 호출 시 두 번째 인자를 지정하지 않으면 자동으로 3채널 컬러 영상 형식으로 영상을 불러온다.

 ![Alt text](/assets/CVimages/imreadModesflags.png)

### imwrite() 함수

 Mat 객체에 저장되어 있는 영상 데이터를 파일로 저장하기 위해서는 **imwrite()** 함수를 사용한다. **imwrite()** 함수의 원형은 아래와 같다.

 ```cpp
 bool imwrite(const String& filename, InputArray img, const std::vector<int>& params = std::vector<int>());
 // filename : 저장할 영상 파일 이름
 // img : 저장할 영상 데이터(Mat 객체)
 // params : 저장할 영상 파일 형식에 의존적인 파라미터(플래그 & 값) 쌍 (param_Id_1, paramValue_1, paramId_2, paramValue_2,...)
 // 반환값 : 정상적으로 저장하면 true, 실패하면 false를 반환한다.
 ```

 **imwrite()** 함수는 img 변수에 저장되어 있는 영상 데이터를 filename 이름의 **파일**로 저장한다. 영상 파일 형식은 filename 문자열에 포함된 **파일 확장자에 의해 결정**된다. params 인자에는 저장할 파일 형식에 의존적인 별도의 옵션을 지정할 수 있다. params 인자의 형식은 **std::vector<int>** 타입으로 지정하며, 옵션 플래그와 실제 값을 정수 값 두 개의 쌍으로 지정해야 한다. 예를 들어 img 변수에 저장된 영상을 lenna.jpg 파일로 저장할 때 JPEG 압축률을 95%로 지정하고 싶다면 다음과 같이 코드를 작성한다.

 ```cpp
 vector<int> params;
 params.push_back(IMWRITE_JPEG_QUALITY);
 params.push_back(95);
 imwrite("lenna.jpg", img, params);
 ```

 위 코드에서 **IMWRITE_JPEG_QUALITY** 플래그가 **JPEG 압축률**을 나타내는 옵션 플래그이다. **imwrite()** 함수의 params 인자에 사용할 수 있는 전체 옵션 플래그는 OpenCV 문서 사이트를 참고하면 된다.

### empty() 함수

 imread() 함수를 이용하여 lenna.bmp 파일을 불러온 후, **영상 데이터가 정상적으로 불러왔는지를 확인**하기 위해 **Mat::empty()** 함수를 사용했다. **Mat::empty()** 함수는 Mat 클래스의 멤버 함수이며, 함수 원형과 동작 방식은 아래와 같다.

 ```cpp
 bool Mat::empty() const
 // 반환값 : 행렬의 rows 또는 cols 멤버 변수가 0이거나, 또는 data 멤버 변수가 NULL이면 true를 반환한다.
 ```

### namedWindow() 함수
 Mat 클래스 객체에 저장되어 있는 영상 데이터를 화면에 나타내기 위해서는 먼저 영상 출력을 위한 빈 창을 생성해야 한다. 이때 사용하는 함수가 **namedWindow()**이며, 이 함수의 원형은 아래와 같다.

 ```cpp
 void namedWindow(const String& winname, int flags = WINDOW_AUTOSIZE);
 // winname : 영상 출력 창 상단에 출력되는 창 고유 이름, 이 문자열로 창을 구분한다.
 // flags : 생성되는 창의 속성을 지정하는 플래그, WindowFlags 열거형 상수를 지정한다.
 ```

 **namedWindow()** 함수는 두 개의 인자로 구성되어 있지만, 두 번째 인자는 기본 인자가 있으므로 winname 문자열 하나만 지정하여 사용할 수 있다. 원래 Windows 운영 체제에서는 각각의 창을 구분하기 위해 핸들<sup>Handle</sup>이라는 숫자 값을 사용하지만, OpenCV에서는 각각의 창에 고유한 문자열을 부여하여 각각의 창을 구분한다. 그러므로 새로운 창을 만들 때에는 winname 인자에 고유한 문자열을 지정해야 한다. winname으로 지정한 창의 고유 이름은 실제 생성되는 창의 상단 제목 표시줄에 출력된다.

 **namedWindow()** 함수의 두 번쨰 인자 flags는 새로 생성하는 창의 속성을 지정하는 용도로 사용된다. flags 인자에는 WindowFlags 열거형 상수를 지정할 수 있다.

 ![Alt text](/assets/CVimages/WindowFlags.png)

 **namedWindow()** 함수의 flags 인자 기본 값은 **WINDOW_AUTOSIZE**이기 때문에, flags 인자를 지정하지 않고 만들어진 창의 크기는 자동으로 영상 크기에 맞게 조정된다. 만약 여러분이 사용하고 있는 모니터 해상도보다 큰 영상을 화면에 출력하려고 하는 경우, **WINDOW_AUTOSIZE** 속성으로 생성된 창에서는 영상의 일부가 화면에 표시되지 않을 수도 있으니 주의해야 한다. 만약 새로 생성한 창 크기를 마우스 또는 **resizeWindow()**함수를 이용하여 변경하고 싶다면 flags 인자에 **WINDOW_NORMAL**을 지정해야 한다.

### destroyWindow(), destroyAllWindows() 함수
 **namedWindow()** 함수에 의해 생성된 영상 출력 창은 **destroyWindow()**또는 **destroyAllWindows()**함수를 이용하여 닫을 수 있다. **destroyWindow()**함수는 하나의 창을 닫을 때 사용하고, **destroyAllWindows()**함수는 열려 있는 모든 창을 닫을 때 사용한다. 이 두 함수의 원형은 아래와 같다.

```cpp
void destroyWindow(const String& winname);
void destroyAllWindow();
//winname : 소멸시킬 창 이름
```

 일반적으로 OpenCV 응용 프로그램이 완전히 종료되는 경우에는 운영 체제에 의해 OpenCV 응용 프로그램이 사용하던 모든 자원이 해제되며, namedWindow()함수에 의해 만들어진 창도 모두 자동으로 닫힌다. destroyWindow(), destroyAllWindows()함수를 명시적으로 호출하지 않아도 프로그램 종료 시 영상 출력 창이 자동으로 닫힌다. 그러나 **프로그램 동작 중**에 창을 닫고 싶을 때에는 **destroyWindow()** 또는 **destroyAllWindows()**함수를 이용해야한다.

### moveWindow()

 OpenCV의 영상 출력 창과 관련된 함수 중, 창 위치를 변경하는 함수이다.

```cpp
void moveWindow(const String& winname, int x, int y);
//winname : 위치를 이동할 창 이름
//x : 창이 이동할 위치의 x좌표
//y : 창이 이동할 위치의 y좌표
```

 **moveWindow()** 함수는 winname 이름의 창을 (x, y) 좌표 위치로 이동시킨다. 여기서 (x, y) 좌표는 모니터 전체 화면에서의 좌표를 나타내며, 모니터 좌측 상단을 원점으로 간주한다.

### resizeWindow()

 프로그램 동작 중에 영상 출력 창의 크기를 변경하는 함수이다.

```cpp
void resizeWindow(const String& winname, int width, int height);
//winname : 크기를 변경할 창 이름
//width : 창의 가로 크기
//height : 창의 세로 크기
```

 **resizeWindow()** 함수는 winname에 해당하는 창 크기를 가로 width, 세로 height 크기에 맞게 변경한다. 이때 함수의 인자로 전달하는 width와 height 크기를 창 전체 크기가 아니라 창의 뷰<sup>view</sup>영역에 나타나는 영상 크기를 의미한다. 그러므로 **resizeWindow()** 함수에 의해 변경된 창 크기는 창의 제목 표시줄, 경계선 두께로 인해 width와 height 크기보다 약간 큰 형태로 결정된다. 다만 **WINDOW_AUTOSIZE** 플래그를 사용하여 만들어진 영상 출력 창은 **resizeWindow() 함수로 크기를 변경할 수 없다.**

### imshow()

 Mat 클래스 객체에 저장된 영상 데이터를 화면에 출력하는 imshow() 함수이다.

 ```cpp
 void imshow(const String& winname, InputArray mat);
 //winname : 영상을 출력할 대상 창 이름
 //mat : 출력할 영상 데이터(Mat 객체)
 ```

 **imshow()** 함수는 winname 창에 mat 인자로 전달된 영상 데이터를 출력한다. mat 객체에 저장된 영상이 1채널 8비트 uchar 자료형으로 구성된 그레이스케일 영상이라면 픽셀 값을 그대로 그레이스케일 밝기 형태로 나타낸다. mat 객체에 저장된 영상이 uchar 자료형을 사용하는 3채널 컬러 영상이라면 색상 채널이 파란색, 녹색, 빨간색 순서로 되어 있다고 간주하여 색상을 표현한다. 만약 mat 객체가 부호 없는 16비트 또는 32비트 정수형이라면 행렬 원소 값을 256으로 나눈 값을 영상의 밝기 값으로 사용한다. 반면에 mat 객체가 32비트 또는 64비트 실수형 행렬이라면 행렬 원소에 255를 곱한 값을 밝기 값으로 사용한다.

 그런데 **imshow()** 함수의 두 번째 인자 자료형이 InputArray라고 되어 있는 점이 조금 특이하다. 
 
 ```cpp
 Mat img = imread("lenna.bmp");

 namedWindow("image");
 imshow("image", img);
 ```

 위 코드에서 img는 Mat 클래스 타입의 변수이지만 **imshow()** 함수의 두 번쨰 인자로 전달된 것을 볼 수 있다. 사실 InputArray 타입은 Mat, vector<T> 등 다양한 객체를 표현할 수 있는 인터페이스 클래스이며, 주로 OpenCV 함수 입력에 해당하는 인자의 자료형으로 사용된다. 그러므로 OpenCV 함수 설명에서 인자 타입이 InputArray라고 되어 있으면 대부분 Mat 클래스 타입의 변수를 전달한다고 생각하여도 무방하다. 

 만약 **imshow()** 함수가 호출되는 시점에 winname에 해당하는 창이 없으면 imshow() 함수는 자동으로 WINDOW_AUTOSIZE 속성의 창을 새로 만들어서 영상을 출력한다. 참고로 Windows 운영 체제에서는 **ctrl + c** 키를 눌러 영상 출력 창에 나타난 영상 데이터를 비트맵 형식으로 클립보드로 복사할 수 있으며, **ctrl + s** 키를 눌러서 파일 형태로 저장할 수 있다.

### waitKey()

 사용자로부터 키도르 입력을 받는 용도로 사용되는 함수이다.

 ```cpp
 int waitKey(int delay = 0);
 //delay : 키 입력을 기다릴 시간(밀리초 단위), delay <= 0이면 무한히 기다린다.
 //반환값 : 눌린 키 값. 지정한 시간 동안 키가 눌리지 않았으면 -1을 반환한다.
 ```

 **waitKey()** 함수는 delay에 해당하는 밀리초 시간 동안 키 입력을 기다린다. 만약 지정한 delay 시간 동안 키 입력이 있으면 해당 키의 아스키 코드 값을 반환한다. 만약 지정한 시간 동안 키 입력이 없으면 **waitKey()** 함수는 -1을 반환한다. 만약 delay 인자에 기본값으로 설정되어 있는 0이 전달되면 사용자가 키를 입력할 때까지 무한히 기다린다.

 **imshow()**함수가 영상을 화면에 나타내는 함수라고 설명했지만, 실제로는 **imshow()**함수만 사용해서는 영상이 화면에 나타나지 않는다. **imshow()** 함수를 호출한 후 **waitKey()** 함수를 호출해야 화면 그리기 이벤트가 동작하여 영상이 화면에 정상적으로 출력된다. 그러므로 대부분의 OpenCV 소스 코드에서 **imshow()** 함수와 **waitKey()** 함수는 연속하여 호출하는 형태로 사용된다.

 





