### 1. SDK

##### <a id="FUNCTION"></a> a) 개요
-> BLE 장치 스캔과 연결을 진행해주는 라이브러리

##### <a id="FUNCTION"></a> b) 라이브러리 지원 레벨

-> minSdkVersion 21 (LOLLIPOP) 이상에서 동작

-> androidx 패키지 사용

### 2. Install

##### <a id="DOWNLOAD"></a> a) SDK Download

-> Project 레벨 build.gradle 파일 maven 주소 추가
```gradle
allprojects {
    repositories {
        google()
        jcenter()
        maven { url 'https://dl.bintray.com/dynamicmang/maven' } // maven 주소 추가
    }
}
```

-> app/build.gradle 파일 dependencies 불록에 의존성 추가
```gradle
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar']) 
    ....
    implementation 'com.bluetooth.sdk:bluetooth-beta:0.0.9' // 의존성 추가
}
```

### 3. API

##### <a id="API_1"></a> a) 장치 스캔

-> 장치 스캔 상태를 전달 받을 콜백 인터페이스를 파라미터에 넣어 스캔 API 호출

```java
// 장치 스캔을 희망하는 곳에서 장치 스캔 API 호출
ServiceUtilKt.startScanService(context, iScanResult);
```

```java
private IScanResult iScanResult = new IScanResult() {
    @Override
    public void onScanResult(boolean isScanned, BluetoothDevice device) {
        // 스캔 상태 여부와 스캔된 장치 리턴
        // isScanned = true -> 스캐 성공
        // isScanned = false -> 스캔 실패
    }
};
```

##### <a id="API_2"></a> b) 장치 연결

-> 장치 연결 상태를 전달 받을 콜백 인터페이스와 연결할(스캔된) 'BluetoothDevice'를 파라미터에 넣어 연결 API 호출

```java
// 장치 연결을 희망하는 곳에서 장치 연결 API 호출
ServiceUtilKt.startConnectService(context, iConnectResult, bluetoothDevice);
```
```java
private IConnectResult iConnectResult = new IConnectResult() {

  @Override
  public void onConnectResult(boolean isConnect, BluetoothDevice device) {
      // 연결 상태 여부와 연결 장치 리턴
      // isConnect = true -> 장치 연결 성공
      // isConnect = false -> 장치 연결 해제
  }

  @Override
  public void onTagCalled(String message) {
      // 태그 호출시 전달 받은 메시지 값 전달
      if (!TextUtils.isEmpty(message) && message.equals("ps01")) {
          // 예시는 ps01 값이 들어올 경우 알람 생성
          // 수신 받은 메시지 상태에 따라 알람 API 호출
          ServiceUtilKt.startAlarm();
      }
  }

};
```
