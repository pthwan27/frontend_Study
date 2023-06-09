# PWA와 Push 알림

## Q1. 프로젝트에서 PWA를 선택한 이유는 무엇인가요?(PWA의 장점)

PWA를 선택한 주된 이유는 **웹과 네이티브 앱의 장점을 모두 가져올 수 있는 유연성** 때문이었습니다.

첫째, PWA는 사용자에게 앱 설치 과정을 생략하게 해서 접근성이 좋습니다. 일반적인 웹 사이트처럼 URL을 통해 접근하면 되므로, 사용자는 별도의 설치 과정 없이 서비스를 이용할 수 있습니다.

둘째, PWA는 네이티브 앱과 같은 사용자 경험을 제공할 수 있습니다. 홈 화면에 앱 아이콘을 추가할 수 있고, 푸시 알림을 받을 수 있습니다.

셋째, 개발과 유지 보수에 있어서 효율적입니다. 하나의 코드 베이스로 모든 플랫폼에서 동작하는 앱을 만들 수 있으므로, 별도로 iOS 앱과 안드로이드 앱을 개발할 필요가 없습니다.

## Q2.PWA를 어떻게 사용하셨나요?

PWA를 웹앱의 구조를 구성하는데 사용하였습니다. **PWA의 핵심 요소인 Manifest 파일과 Service Worker를 이용하여 앱의 기본 설정을 정의하고, 백그라운드에서 동작하는 기능을 구현하였습니다.**

Manifest 파일을 이용하여 앱의 이름, 아이콘, 시작 URL, 디스플레이 타입 등의 기본 설정을 정의하였습니다. 이를 통해 앱이 홈 화면에 추가될 때 어떤 아이콘이 표시되고, 앱을 시작할 때 어떤 화면이 표시될지 등을 지정하였습니다.

Service Worker는 백그라운드에서 동작하여 네트워크 요청을 가로채는 등의 역할을 합니다. 프로젝트에서 Service Worker를 이용하여 FCM으로부터 푸시 알림을 받는 기능을 구현하였습니다.

## Q3. Service Worker를 어떻게 사용하셨나요?

이번 프로젝트에서 Service Worker는 주로 FCM을 통한 푸시 알림을 처리하는데 사용되었습니다. 프로젝트에서 FCM에서 보낸 메시지를 Service Worker에서 받아 사용자에게 알림을 표시하였습니다. 이를 통해 앱이 백그라운드에 있거나 완전히 종료된 상태에서도 사용자에게 푸시 알림을 전달할 수 있었습니다.

## Q4. 프로젝트에서 FCM을 왜 사용하셨나요?

FCM은 클라우드에서 사용자의 장치로 메시지를 효과적으로 보내는 서비스입니다. 이번 프로젝트에서 FCM을 선택한 주요 이유는 **푸시 알림 기능을 신속하고 간편하게 구현할 수 있다는 점과, 다양한 플랫폼에서 확장성 있게 사용할 수 있다는 점**입니다.

FCM은 안드로이드, iOS, 웹 등 다양한 플랫폼을 지원하며, 애플리케이션이 비활성 상태거나 백그라운드에 있을 때도 사용자에게 메시지를 전달할 수 있습니다. 따라서 앱 사용자에게 실시간으로 중요한 정보를 전달하거나, 앱 사용을 유도하는 등의 목적을 가진 푸시 알림 기능 구현에 사용하였습니다.

또한 FCM은 크로스 플랫폼 메시징 솔루션이기 때문에 한 번의 구현으로 여러 플랫폼에 동일한 기능을 제공할 수 있기 때문에 개발 시간을 단축시킬 수 있었습니다.

이런 이유로, FCM을 프로젝트에 사용하였습니다.

## Q5. FCM을 어떻게 사용하셨나요?

FCM을 이용하여 서버에서 클라이언트로 푸시 알림을 전송하였습니다.

먼저, 사용자가 앱에 접속하면 앱은 FCM으로부터 고유한 토큰을 받습니다. 이 토큰은 FCM이 특정 클라이언트에게 메시지를 보낼 때 사용합니다.

다음으로, 서버는 이 토큰을 이용하여 FCM에 푸시 알림을 요청합니다. FCM은 이 요청을 받아 해당 토큰에 연결된 클라이언트에게 메시지를 전송합니다.

이 때, 클라이언트에서는 Service Worker를 이용하여 FCM으로부터 메시지를 받습니다. Service Worker는 이 메시지를 받아 사용자에게 알림을 표시합니다. 이 과정은 앱이 백그라운드에 있거나 완전히 종료된 상태에서도 동작하므로, 언제든지 사용자에게 알림을 전달할 수 있습니다.
