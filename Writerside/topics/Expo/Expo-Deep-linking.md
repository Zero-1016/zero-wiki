# Expo Deep linking

[원본](https://docs.expo.dev/guides/deep-linking/)

> 표준 웹 URL에서 앱을 열기 위해 앱/유니버설 링크를 사용하는 방법을 알아보세요.

유니버설 링크는 [표준 딥 링크](https://docs.expo.dev/guides/linking/#linking-to-your-app)와 다르게 일반 HTTPS 링크를 사용하여 사용자를 앱의 특정 경로로
안내합니다. 사용자가 앱을 설치하지 않은 경우
링크는 관련된 웹사이트로 이동합니다. 이를 통해 데스크톱 웹 브라우저에서 원활하게 작동하는 링크가 포함된 알림 이메일을 보낼 수 있으며, 모바일에서는 앱에서 콘텐츠를 열 수 있습니다. Android에서는 이를 **앱
링크**라고 하고, iOS에서는 **유니버설 링크**라고 합니다. 이 가이드에서는 사용자 지정 URL 스킴을 사용하지 않는 유니버설 링크에 대해 설명합니다.

> 지연된 딥 링크는 [`react-native-branch`](https://github.com/expo/config-plugins/tree/main/packages/react-native-branch)를 사용하여
> 구현할 수 있습니다.

앱/유니버설 링크를 사용하기 전에 웹사이트와 앱 간의 양방향 연결을 설정해야 합니다.

1. **네이티브 앱 인증:** 대상 웹사이트 도메인(URL)을 참조하는 코드 서명이 필요합니다.
2. **웹사이트 인증:** **/.well-known** 디렉토리에 파일을 호스팅해야 합니다.

양방향 연결이 설정되면 앱에서 링크의 런타임 라우팅을 설정해야 합니다. 이는 JavaScript에서 수행되며 모든 경로에 대해 구성한 다음 웹과 네이티브 간에 조정해야 합니다. Expo는 이를
위해 [Expo Router](Expo-Router.md)라는 완전 자동화된 솔루션을 제공합니다.

> 유니버설 링크는 Expo Go 앱에서 테스트할 수 없습니다. [개발 빌드](https://docs.expo.dev/develop/development-builds/introduction/)를 생성해야 합니다.

## iOS에서의 유니버설 링크

iOS에서 유니버설 링크를 사용하려면 유료 Apple Developer 계정이 필요하며, 완전한 **Apple Developer Team ID**를 연결해야 합니다.

### 네이티브 Apple 구성

apple-app-site-association(AASA) 파일을 배포한 후 연결된 도메인을 사용하도록 앱을 구성해야 합니다:

[`expo.ios.associatedDomains`](https://docs.expo.dev/versions/latest/config/app/#associateddomains)
를 [앱 구성](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_associated-domains)에
추가하고 [Apple이 지정한 형식](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_associated-domains)
을 따르세요. URL에 프로토콜(`https`)을 포함하지 않도록 하세요. 이는 유니버설 링크가 작동하지 않게 하는 일반적인 실수입니다.

예를 들어, 관련된 웹사이트가 `https://expo.dev/`인 경우, 앱 링크는 다음과 같습니다.

```json
{
  "expo": {
    "ios": {
      "associatedDomains": [
        "applinks:expo.dev"
      ]
    }
  }
}
```

EAS Build를 사용하여 네이티브 앱을 빌드하여 Apple과 자격을 등록하세요.

#### **수동 네이티브 구성**

[Continuous Native Generation](https://docs.expo.dev/workflow/continuous-native-generation/) (`npx expo prebuild`)을 사용하지
않는 앱은 [수동으로 구성](https://docs.expo.dev/build-reference/ios-capabilities/#manual-setup)하여 번들 식별자에 대한 Associated Domains
기능을 설정해야 합니다.

[Apple Developer Console](https://docs.expo.dev/build-reference/ios-capabilities/#apple-developer-console)에서 활성화한
경우, `ios/[app]/[app].entitlements` 파일에 다음 자격을 추가하세요.

```xml
<key>com.apple.developer.associated-domains</key>
<array>
    <string>applinks:expo.dev</string>
</array>
```

### AASA 구성

웹 측에서는 **/.well-known/apple-app-site-association**(확장자 없이)에서 구성 파일을 호스팅해야 합니다. 이 파일 JSON은 Apple Developer Team ID, 번들러
ID 및
네이티브 앱으로 리디렉션할 지원 경로 목록을 지정합니다.

> 실험적 CLI 명령어 `npx setup-safari`를 실행하여 번들 식별자를 Apple 계정에 자동으로 등록하고, ID에 자격을 할당하며, 스토어에 iTunes 앱 항목을 생성할 수 있습니다. 로컬 설정이
> 출력되며 대부분의 다음 단계를 건너뛸 수 있습니다. 이는 iOS에서 유니버설 링크를 시작하는 가장 쉬운 방법입니다.

[Expo Router](Expo-Router.md)를 사용하여 웹사이트를 빌드하는 경우(또는 Remix, Next.js 등 다른 최신 React 프레임워크를 사용하는 경우), *
*public/.well-known/apple-app-site-association**에 파일을 생성합니다. 레거시 Expo 웹팩 프로젝트는 *
*web/.well-known/apple-app-site-association**에 파일을 생성할 수 있습니다.

```json
{
  // 이 섹션은 유니버설 링크를 활성화합니다
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "{APPLE_TEAM_ID}.{BUNDLE_ID}",
        // 리디렉션을 지원하는 모든 경로
        "paths": [
          "/records/*"
        ]
      }
    ]
  },
  // 이 섹션은 Apple Handoff를 활성화합니다
  "activitycontinuation": {
    "apps": [
      "{APPLE_TEAM_ID}.{BUNDLE_ID}"
    ]
  },
  // 이 섹션은 공유 웹 자격을 활성화합니다
  "webcredentials": {
    "apps": [
      "{APPLE_TEAM_ID}.{BUNDLE_ID}"
    ]
  }
}
```

이 스니펫은 iOS에 https://www.myapp.io/records/*로의 링크가 동일한 번들 식별자가 있는 앱에 의해 직접 열려야 함을 알려줍니다. 이는 팀 ID와 앱 번들 식별자의 조합입니다. 팀 ID는
Apple Developer 계정의 멤버십 세부 정보에서 찾을 수 있습니다.

> `activitycontinuation` 및 `webcredentials` 객체는 선택 사항이지만 권장됩니다.

AASA 파일을 설정한 후, 웹사이트를 HTTPS를 지원하는 서버에 배포하세요(대부분의 최신 웹 호스트가 지원).

AASA 형식에 대한 자세한
내용은 [Apple의 문서](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html)
를 참조하세요. Branch는 AASA가 올바르게 배포되고 유효한 형식을 가졌는지 확인하는 [AASA 검증기](https://branch.io/resources/aasa-validator/)를 제공합니다.

> `*` 와일드카드는 도메인 또는 경로 구분 기호(점 및 슬래시)를 일치시키지 않습니다.

iOS 13부터, 새로운 `details` 형식이 지원됩니다. 이를 통해 다음을 지정할 수 있습니다:

- 여러 앱을 동일한 구성에 연결하기 쉽게 하는 `appIDs` 대신 `appID`
- 프래그먼트 지정, 특정 경로 제외 및 주석 추가를 허용하는 `components` 배열

#### **Apple's 문서에서 제공하는 예제**

```json
{
  "applinks": {
    "details": [
      {
        "appIDs": [
          "ABCDE12345.com.example.app",
          "ABCDE12345.com.example.app2"
        ],
        "components": [
          {
            "#": "no_universal_links",
            "exclude": true,
            "comment": "프래그먼트가 no_universal_links인 모든 URL과 일치하며 이를 유니버설 링크로 열지 않도록 시스템에 지시"
          },
          {
            "/": "/buy/*",
            "comment": "/buy/로 시작하는 경로를 가진 모든 URL과 일치"
          },
          {
            "/": "/help/website/*",
            "exclude": true,
            "comment": "/help/website/로 시작하는 경로를 가진 모든 URL과 일치하며 이를 유니버설 링크로 열지 않도록 시스템에 지시"
          },
          {
            "/": "/help/*",
            "?": {
              "articleNumber": "????"
            },
            "comment": "/help/로 시작하는 경로를 가지고 'articleNumber'라는 이름의 쿼리 항목을 가지며 값이 정확히 4자인 경우의 모든 URL과 일치"
          }
        ]
      }
    ]
  }
}
```

모든 iOS 버전을 지원하려면 `details` 키에 위의 두 형식을 모두 제공할 수 있지만, 최신 iOS 버전에 대한 구성을 먼저 배치하는 것이 좋습니다.

iOS는 앱이 처음 설치되거나 App Store에서 업데이트될 때 AASA를 다운로드하지만, 더 자주 새로 고치지 않습니다. 프로덕션 앱에서 AASA의 경로를 변경하려면, App Store를 통해 전체 업데이트를
발행해야 하며, 이를 통해 모든 사용자의 앱이 AASA를 다시 가져오고 새로운 경로를 인식할 수 있습니다.

이제 모바일 디바이스에서 웹사이트로의 링크가 앱을 열어야 합니다. 그렇지 않은 경우, AASA가 유효한지, 경로가 AASA에 지정되었는지, Apple Developer Console에서 App ID를 올바르게
구성했는지 확인하세요. 앱이 열리면 [앱 내 링크 처리 섹션](https://chatgpt.com/guides/linking/#handling-links)으로 이동하여 들어오는 링크를 처리하고 사용자가 요청한
콘텐츠를 표시하는 방법에 대한 자세한 내용을 확인하세요.

### 애플 스마트 배너

사용자가 앱을 설치하지 않은 경우, 웹사이트로
이동합니다. [Apple Smart Banner](https://developer.apple.com/documentation/webkit/promoting_apps_with_smart_app_banners)를
사용하여 페이지 상단에 앱 설치를 유도하는 배너를 표시할 수 있습니다. 배너는 사용자가 모바일 디바이스에서 앱을 설치하지 않은 경우에만 표시됩니다.

배너를 활성화하려면, 웹사이트의 `<head>`에 다음 메타 태그를 추가하세요. {`ITUNES_ID}`를 앱의 iTunes ID로 대체합니다.

```html

<meta name="apple-itunes-app" content="app-id={ITUNES_ID}"/>
```

배너 설정에 문제가 있는 경우, 다음 명령어를 실행하여 프로젝트에 대한 메타 태그를 자동으로 생성하세요.

```Bash
npx setup-safari
```

[Expo Router를 사용하여 정적으로 렌더링된 웹사이트를 빌드](https://chatgpt.com/router/reference/static-rendering)하는
경우 [app/+html.js](https://chatgpt.com/router/reference/static-rendering#root-html) 파일의 `<head>`에 있는 구성 요소 에 이 HTML 태그를
추가하세요 .

```Typescript
export default function Root({ children }) {
  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta httpEquiv="X-UA-Compatible" content="IE=edge" />

        <meta name="apple-itunes-app" content="app-id={ITUNES_ID}" />

        {/* Other head elements... */}
      </head>
      <body>{children}</body>
    </html>
  );
}
```

다음에 웹사이트를 배포할 때, 앱이 설치되지 않은 모바일 디바이스에서 웹사이트를 방문하면 배너가 표시되어야 합니다.

## Android Deep link

Android에서 딥 링크를 구현하는 것은 (사용자 정의 URL 스킴 없이) iOS보다 다소 간단합니다. 앱 구성의 `android` 섹션에 `intentFilters`를 추가해야 합니다. 다음 기본
구성은 `myapp.io`로의 모든 기록 링크를 처리하기 위한 옵션으로 표준 Android 대화 상자에 앱을 표시합니다.

```json
{
  "expo": {
    "android": {
      "intentFilters": [
        {
          "action": "VIEW",
          /* 앱 링크를 활성화하려면 app config의 intent filter에 autoVerify 속성을 추가합니다. */
          "autoVerify": true,
          "data": [
            {
              "scheme": "https",
              "host": "*.myapp.io",
              "pathPrefix": "/records"
            }
          ],
          "category": [
            "BROWSABLE",
            "DEFAULT"
          ]
        }
      ]
    }
  }
}
```

### Android 앱 링크

도메인으로의 링크가 항상 앱을 열도록 하려면 (사용자에게 브라우저나 다른 핸들러를 선택하는 대화 상자를 표시하지 않고), Android 앱 링크를 구현할 수 있습니다. 이는 iOS의 유니버설 링크와 유사한 인증
과정을 사용합니다.

웹사이트 인증을 위한 JSON 파일(디지털 자산 링크 파일로도 알려져 있음)을 **public/.well-known/assetlinks.json**(또는 레거시 Expo 웹팩 웹사이트의 경우
**web/.well-known/assetlinks.json**)에 생성하고 다음 정보를 수집하세요.

- `package_name`: 앱의 Android 애플리케이션 ID (예: `com.bacon.app`). 이는 `app.json` 파일의 `expo.android.package` 아래에 있습니다.
- `sha256_cert_fingerprints`: 앱의 서명 인증서의 SHA256 지문. 이는 두 가지 방법 중 하나로 얻을 수 있습니다:
  EAS Build로 Android 앱을 빌드한 후, `eas credentials -p android`를 실행하고 지문을 얻으려는 프로필을 선택합니다. 지문은 `SHA256 Fingerprint` 아래에
  나열됩니다.
  Play Console 개발자 계정의 **Release > Setup > App Signing**에서 확인할 수 있습니다. 이 경우, 동일한 페이지에서 앱에 대한 올바른 디지털 자산 링크 JSON 스니펫도 찾을
  수
  있습니다. 값은 `14:6D:E9:83...`와 같이 표시됩니다.

```json
[
  {
    "relation": [
      "delegate_permission/common.handle_all_urls"
    ],
    "target": {
      "namespace": "android_app",
      "package_name": "{package_name}",
      "sha256_cert_fingerprints": [
        // 여러 앱 및 키에 대한 지문 지원
        "{sha256_cert_fingerprints}"
      ]
    }
  }
]
```

네이티브 앱을 디바이스에 설치하면 [Android 앱 인증](https://developer.android.com/training/app-links/verify-android-applinks#web-assoc)
프로세스가 트리거되며, 이는 최대 20초가 소요될 수 있습니다. 앱이 열리면, [앱 내 링크 처리](https://chatgpt.com/guides/linking/#handling-links) 섹션으로 이동하여
들어오는 링크를 처리하고 사용자가 요청한
콘텐츠를 표시하는 방법에 대한 자세한 내용을 확인하세요.

#### **수동 네이티브 구성**

EAS를 사용하여 코드 서명을 관리하지 않는 경우, 앱을 수동으로 빌드하고 제출한 후, [Google Play Console](https://play.google.com/console/) 개발자 계정의
**Release > Setup > App Signing**에서
sha256_cert_fingerprints를 찾을 수 있습니다. 이 경우, 동일한 페이지에서 앱에 대한 올바른 **디지털 자산 링크 JSON** 스니펫도 찾을 수 있습니다. 값은 `14:6D:E9:83...`와
같이
표시됩니다. 이 값을 `public/.well-known/assetlinks.json` 파일에 복사하세요.

## 디버깅

Expo CLI를 사용하면 웹사이트를 배포하지 않고도 유니버설 링크를 테스트할 수 있습니다. `--tunnel` 기능을 사용하여 개발 서버를 공개적으로 사용할 수 있는 https URL로 포워딩할 수 있습니다.

1. 환경 변수 `EXPO_TUNNEL_SUBDOMAIN=my-custom-domain`을 설정합니다. 여기서 "my-custom-domain"은 개발 중에 사용할 고유 문자열입니다. 이는 개발 서버 재시작 시에도
   터널 URL이 일관되도록 합니다.
2. 위에서 설명한 대로 유니버설 링크를 설정하되, 이번에는 Ngrok URL을 사용합니다. `my-custom-domain.ngrok.io`
3. `--tunnel` 플래그로 개발 서버를 시작합니다.

```Bash
npx expo start --tunnel --dev-client
```

디바이스에서 개발 빌드를 컴파일합니다.

```Bash
# 네이티브 Android 프로젝트 빌드
npx expo run:android

# 네이티브 iOS 프로젝트 빌드
npx expo run:ios
```

## 문제 해결

- Apple의 [유니버설 링크 디버깅 공식 문서](https://developer.apple.com/documentation/technotes/tn3155-debugging-universal-links)를
  읽어보세요.
- [검증 도구](https://branch.io/resources/aasa-validator/)를 사용하여 apple 앱 사이트 연결 파일이 유효한지 확인하세요.
- 웹사이트가 HTTPS를 통해 제공되는지 확인하세요.
- 압축되지 않은 `apple-app-site-association`
  파일은 [128kb를 초과할 수 없습니다](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html).
- [Android 앱 링크 검증](https://developer.android.com/training/app-links/verify-android-applinks)
- 두 웹사이트 검증 파일이 `application/json` 콘텐츠 유형으로 제공되는지 확인하세요.
- Android 검증은 최대 20초가 소요될 수 있습니다.
- 웹 파일을 업데이트하면, 공급 업체 측(Google, Apple)에서 서버 업데이트를 트리거하려면 네이티브 앱을 다시 빌드하세요.

## 딥 링크를 사용하지 않아야 할 때

딥 링크를 설정하는 가장 쉬운 방법입니다. 최소한의 구성이 필요합니다.

주요 문제는 사용자가 앱을 설치하지 않았고 사용자 정의 스킴으로 앱에 링크를 따르는 경우, 운영 체제가 페이지를 열 수 없음을 나타내지만 추가 정보를 제공하지 않습니다. 이는 좋은 경험이 아닙니다. 브라우저에서 이를
해결할 방법은 없습니다.

또한, 많은 메시징 앱이 사용자 정의 스킴을 가진 URL을 자동으로 링크하지 않습니다. 예를 들어, `exp://u.expo.dev/[project-id]?channel-name=[channel-name]
&runtime-version=[runtime-version]`는 브라우저에서 일반 텍스트로 표시될 수 있으며 링크가 아닙니다 (exp://u.expo.dev/[project-id]
?channel-name=[channel-name]&runtime-version=[runtime-version]).

예를 들어 Gmail은 대부분의 앱 링크에서 href 속성을 제거합니다. 대신 일반 HTTPS URL로 링크를 지정하면 사용자의 웹 브라우저가 열립니다. 브라우저는 일반적으로 href 속성을 제거하지 않으므로
온라인에서 파일을 호스팅하여 사용자를 앱의 사용자 정의 스킴으로 리디렉션할 수 있습니다.

`example://path/into/app`로 링크하는 대신, `https://example.com/redirect-to-app.html`로 링크할 수 있으며, `redirect-to-app.html`은 다음 코드를
포함합니다.

```html
<script>
  window.location.replace('example://path/into/app');
</script>
```