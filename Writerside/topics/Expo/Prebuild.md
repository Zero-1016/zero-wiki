# Prebuild

[원문](https://docs.expo.dev/workflow/prebuild/)

> Expo CLI가 프로젝트의 네이티브 코드를 컴파일하기 전에 생성하는 방법을 알아보세요.

해당 글을 본다면 Prebuild가 무슨 역할을 하는지 알 수 있습니다.

네이티브 앱을 컴파일하기 전에 네이티브 소스 코드를 생성해야 합니다. Expo CLI는 prebuild 라는 고유하고 강력한 시스템을 제공하며 , 이는 네 가지 요인을 기반으로 프로젝트의 네이티브 코드를 생성합니다.

1. 앱 구성 파일 **(app.json, app.config.js)**
2. `npx expo prebuild` 명령어에 전달된 인수
3. 프로젝트에 설치된 `expo` 버전과 해당 사전 빌드 템플릿
4. **package.json**에서 찾은 네이티브 모듈을 링크합니다.

## 사용법

Prebuild는 다음 명령어를 실행하여 사용할 수 있습니다.

```Bash
npx expo prebuild
```

이 명령어는 **android** 및 **ios** 디렉토리를 생성하여 React 코드를 실행합니다. 생성된 디렉토리를 수동으로 수정하면 `npx expo prebuild --clean`을 실행할 때 변경 사항을
잃을 위험이 있습니다. 대신, 구성 플러그인을 생성하여 prebuild 동안 네이티브 프로젝트를 수정해야 합니다.

prebuild 시스템을 사용하는 이유는 [pitch](#pitch) 섹션에 나열된 이유로 매우 권장되지만, 시스템은 완전히 선택적이며 언제든지 사용을 중지할 수 있습니다.

## EAS Build와 함께 사용

프로젝트에 **android** 및 **ios** 디렉토리가 포함되어 있지 않으면, EAS Build는 컴파일 전에 이러한 네이티브 디렉토리를 생성하기 위해 Prebuild를 실행합니다.
이는 `npx create-expo-app`을 사용하여 생성된 모든 프로젝트의 기본 동작입니다.

**android** 및 **ios** 디렉토리가 있는 프로젝트의 경우, EAS Build는 네이티브 디렉토리의 변경 사항을 덮어쓰지 않기 위해 기본적으로 Prebuild를 실행하지 않습니다.

앱을 [로컬에서 컴파일](https://docs.expo.dev/guides/local-app-development#local-app-compilation)하여 문제를 해결하는
경우(`npx expo prebuild`, 또는 `npx expo run:android` 또는 `npx expo run:ios` 실행), 빌드 과정에서 새로운 네이티브 디렉토리를 생성하기 위해 EAS Build와
함께 Prebuild를 계속 사용할 수 있습니다. 이 경우, **android** 및 **ios** 디렉토리를 **.gitignore** 또는 **.easignore** 파일에 추가하세요:

```text
+ /android
+ /ios
```

## Expo CLI 실행 명령어와 함께 사용

다음 명령어를 실행하여 로컬에서 네이티브 빌드를 수행할 수 있습니다.

```Bash
# Build Android project
npx expo run:android

# Build Ios project
npx expo run:ios
```

네이티브 빌드 디렉토리가 없는 경우, 특정 플랫폼에서 `npx expo prebuild`가 한 번 실행됩니다. 이후 실행 명령어를 사용할 때마다 로컬 구성과 네이티브 코드가 새로 동기화되었는지
확인하려면 `npx expo
prebuild`를 수동으로 실행해야 합니다.

## 플랫폼 지원

Prebuild는 현재 Android와 iOS를 지원합니다. 웹은 네이티브 프로젝트를 생성할 필요가 없기 때문에 지원되지 않습니다. 개별 플랫폼을 빌드하려면 `--platform` 플래그를 사용할 수 있습니다.

```Bash
npx expo prebuild --platform ios
```

## 종속성

prebuild의 첫 번째 단계는 템플릿에서 새로운 네이티브 프로젝트를 초기화하는 것입니다. 각 Expo SDK 버전에는 템플릿이 있으며, 각 Expo SDK 버전은 특정 React 및 React Native 버전에
해당합니다. 프로젝트의 React 및 React Native 버전은 [템플릿](https://docs.expo.dev/workflow/prebuild#templates)
package.json의 `dependencies` 필드에서 사용하는 버전과 동일하지 않으면 업데이트됩니다.

## 패키지 관리자

[종속성](https://docs.expo.dev/workflow/prebuild#dependencies)이 변경되면, prebuild는 현재 프로젝트에서 사용되는 패키지 관리자를 사용하여 패키지를 다시 설치합니다(
이는 lockfile에서 추론됩니다). 특정 패키지 관리자를 강제로 지정하려면 `--npm`,
`--yarn`, `--pnpm` 중 하나를 제공할 수 있습니다.

모든 설치를 건너뛰려면 `--no-install` 명령어를 사용하면 됩니다. 이는 생성 테스트를 빠르게 수행하는 데 유용합니다.

## 클린

`--clean` 플래그는 기존 네이티브 디렉토리를 삭제한 후 생성합니다.

`--clean` 플래그 없이 `npx expo prebuild`를 다시 실행하면 기존 파일 위에 변경 사항이 적용됩니다. 이는 처음부터 다시 생성하는 것보다 빠르지만, 일부 경우에는 동일한 결과를 생성하지 않을 수
있습니다. 예를 들어, 모든 구성 플러그인이 멱등성이 있는 것은 아닙니다. 프로젝트가 애플리케이션 코드에 정규 표현식 변경을 추가하는 것과 같은 많은 "위험한 수정"을 수행하는 경우, 때때로 예상치 못한 동작을
초래할 수 있습니다. 따라서 `--clean` 플래그를 사용하는 것이 가장 안전한 방법이며, 대부분의 경우 권장됩니다.

플래그의 파괴적인 특성 때문에, `--clean` 플래그를 사용할 때는 깨끗한 git 상태를 유지하도록 경고합니다. 이 프롬프트는 선택 사항이며 CI에서 만날 때는 건너뛰게 됩니다.

원하는 경우, 환경 변수 `EXPO_NO_GIT_STATUS=1`을 활성화하여 확인을 비활성화할 수 있습니다.

프롬프트의 목적은 관리 워크플로우 사용자가 프로젝트의 **.gitignore**에 **android** 및 **ios** 디렉토리를 추가하도록 유도하여 프로젝트가 항상 관리되도록 하는 것입니다. 그러나 이는 사용자
정의 구성 플러그인을 개발하는 것을 더 어렵게 만들 수 있으므로, 이 동작을 강제할 메커니즘을 도입하지 않았습니다.

개발자가 워크플로우 간에 자주 전환해야 하는 경우가 있습니다. 예를 들어, Xcode 및 Android Studio에서 네이티브로 사용자 정의 기능을 빌드한 후 해당 기능을 로컬 구성 플러그인으로 이동하고 싶을 수
있습니다.

> 이론적으로는 깨끗한 빌드가 분 단위가 아닌 초 단위로 수행될 수 있어 --clean이 기본 동작이 될 수 있습니다.

## 템플릿

관리 워크플로우를 유지하면서 네이티브 폴더가 생성되는 방식을 사용자 정의하려면 [구성 플러그인](https://docs.expo.dev/config-plugins/introduction)을 빌드하면 됩니다. 많은
구성 플러그인이 이미 다양한 수정을 위해
존재합니다. [인기 있는 플러그인 목록][https://docs.expo.dev/eas]을 확인하여 자세한 정보를 얻을 수 있습니다.

Prebuild는 구성 플러그인으로 수정하기 전에 템플릿 파일을 생성합니다. 템플릿 파일은 Expo SDK 버전에 기반하며 npm
패키지 [`expo-template-bare-minimum`](https://github.com/expo/expo/tree/main/templates/expo-template-bare-minimum)에서
가져옵니다. `npx expo prebuild` 명령어에 `--template /path/to/template.tgz`를 전달하여 사용할 템플릿을 변경할 수 있습니다.
그러나 `@expo/prebuild-config`의 기본
수정자가 템플릿 파일에 대해 일부 문서화되지 않은 가정을 하기 때문에, 사용자 정의 템플릿을 유지하는 것이 까다로울 수 있으므로 일반적으로 권장되지 않습니다.

> 참고: 모든 패키지가 비공개 레지스트리에서 다운로드되고 npm 공용 레지스트리 액세스가 차단된 네트워크 환경에서는, 로컬에서 사용 가능한 템플릿을 prebuild 명령어에 전달해야 합니다. 기본 템플릿의 로컬
> 버전을 사용하는 방법에 대해 자세히 알아보세요.

## 부작용

`npx expo prebuild`는 **android** 및 **ios** 디렉토리를 생성하는 것 외에도 몇 가지 부작용을 수행합니다. 이러한 부작용을 제거하기 위해 작업이 진행 중입니다.
이상적으로는 `npx expo prebuild`를 실행하면 **Android** 및 **iOS** 프로젝트가 생성되고 나머지 프로젝트는 변경되지 않아야 합니다.

네이티브 폴더를 생성하는 것 외에도 prebuild는 다음과 같은 수정을 수행합니다.

1. package.json의 `scripts` 필드를 수정합니다.
2. package.json의 `dependencies` 필드를 수정합니다.

`scripts` 필드에 대한 편의 변경은 개발자가 prebuild 이전/이후에 앱 작업 방식을 변경하는 유일한 부작용입니다. 모든 다른 변경 사항은 그대로 두고 git에 커밋하여 prebuild를 실행할 때
diff를 최소화할 수 있습니다.

## 선택성

Prebuild는 완전히 선택 사항이며 Expo에서 제공하는 모든 도구 및 서비스와 잘 작동합니다. `npx expo prebuild`를 사용하지 않는 프로젝트를 _bare workflow_ 로 지칭했습니다. 이는
prebuild처럼 수시로 네이티브 프로젝트를 생성하는 대신, 개발자가 네이티브 프로젝트에 직접 변경을 가하는 프로젝트를 의미합니다.

Expo에서 제공하는 모든 것(예: [EAS](https://docs.expo.dev/eas), Expo CLI 및 Expo SDK의 라이브러리)은 _bare workflow_ 를 완벽하게 지원하도록 구축되었습니다.
이는 `npx expo prebuild`를 사용하는 프로젝트를 지원하기 위한 최소 요구 사항입니다. 유일한 예외는 [Expo Go](https://expo.dev/go) 앱으로, 이는 bare workflow
프로젝트를 로드할 수 있지만, Expo Go 런타임에 존재하지 않는 네이티브 코드에 대한 JavaScript 대체를 제공하는 경우에만 가능합니다.

## Pitch

단일 네이티브 프로젝트 자체는 유지 관리, 확장 및 성장하기에 복잡합니다. 크로스 플랫폼 앱에서는 여러 네이티브 프로젝트를 유지 관리하고, 모든 타사 종속성에서 너무 뒤처지지 않도록 최신 운영 체제 릴리스와 함께
유지해야 합니다. 이 프로세스를 간소화하기 위해 선택적 Expo Prebuild 시스템을 만들었습니다. React Native 컨텍스트에서 네이티브 개발과 관련된 몇 가지 문제와 이에 대한 Expo Prebuild가
이러한 문제를 해결하는 이유를 아래에 나열했습니다.

> Prebuild는 모든 React Native 프로젝트에서 사용할 수 있습니다. 자세한 내용은 [Adopt prebuild](https://docs.expo.dev/guides/adopting-prebuild)를
> 참조하세요.

### 합리적인 업그레이드

네이티브 코드를 빌드하려면 해당 네이티브 플랫폼의 기본 도구에 대한 기본적인 이해가 필요하여 상당히 어려운 학습 곡선을 초래합니다. 크로스 플랫폼의 경우, 이 곡선은 개발하려는 플랫폼 수만큼 곱해집니다. 크로스
플랫폼 도구는 플랫폼별 네이티브 코드에서 많은 기능을 개별적으로 구현해야 할 경우 이 문제를 해결하지 않습니다.

네이티브 앱을 부트스트랩할 때 시작하는 데 필요 없는 많은 코드와 구성이 있습니다. 하지만 이제 이를 소유하게 됩니다. 결국 네이티브 애플리케이션을 업그레이드하고자 할 때, 모든 초기 코드가 어떻게 작동하는지
익숙해져야 안전하게 업그레이드할 수 있습니다. 이는 매우 도전적이며, 사용자는 종종 중요한 변경 사항을 놓치고 앱을 잘못 업그레이드하거나, 새 앱을 부트스트랩하고 모든 소스 코드를 새 애플리케이션에 복사합니다.

**Prebuild**를 사용하면 순수 JavaScript 애플리케이션을 업그레이드하는 것과 훨씬 유사하게 업그레이드됩니다. **package.json**의 버전을 올리고 네이티브 프로젝트를 재생성합니다.

### 크로스 플랫폼 구성

앱 아이콘, 이름, 스플래시 화면 등과 같은 크로스 플랫폼 구성은 네이티브 코드에서 수동으로 구현해야 합니다. 이는 플랫폼마다 매우 다르게 구현되는 경우가 많습니다.

**Prebuild를 사용하면** 크로스 플랫폼 구성은 구성 플러그인 수준에서 처리되며, 개발자는 `"icon": "./icon.png"`와 같은 단일 값을 설정하여 모든 아이콘 생성이 처리됩니다.

### 종속성 부작용

많은 복잡한 네이티브 패키지는 설치 및 [자동 링크](https://docs.expo.dev/more/glossary-of-terms#autolinking) 외에 추가 설정이 필요합니다. 예를 들어, 카메라
라이브러리는 Android의 경우 **AndroidManifest.xml**, iOS의 경우
**Info.plist**에 권한 설정을 추가해야 합니다. 이 추가 설정은 패키지의 구성 부작용으로 간주될 수 있습니다. 프로젝트의 네이티브 파일에 필요한 부작용 코드를 붙여넣으면 어려운 네이티브 컴파일 오류로
이어질 수
있으며, 이제는 이를 소유하고 유지 관리해야 하는 코드입니다.

**Prebuild를 사용하면** 라이브러리 작성자는 자신의 라이브러리를 설정하는 방법을 가장 잘 알고 있는
사람으로서 [구성 플러그인](https://docs.expo.dev/config-plugins/introduction)이라는 테스트 가능한 버전된 스크립트를 만들어 라이브러리에
필요한 구성 부작용을 자동으로 추가할 수 있습니다. 이는 라이브러리 부작용이 더 표현적이고 강력하며 안정적일 수 있음을 의미합니다. 네이티브 코드 부작용의
경우, [AppDelegate Subscribers](https://docs.expo.dev/modules/appdelegate-subscribers) 및
[Android Lifecycle Listeners](https://chatgpt.com/modules/android-lifecycle-listeners)를
기본 [prebuild 템플릿](https://chatgpt.com/c/02c72dce-9738-4c1c-8084-f399ee89e7d9#templates)에 표준으로 제공합니다.

### 버려진 코드

패키지를 제거할 때 해당 패키지를 작동시키기 위해 필요한 모든 부작용을 제거했는지 확실히 해야 합니다. 누락된 경우, 특정 패키지로 추적할 수 없는 고아 코드가 발생하며, 이 코드는 누적되어 프로젝트를 이해하고 유지
관리하기 어렵게 만듭니다.

**Prebuild를 사용하면** 유일한 부작용은 프로젝트의 Expo 구성(**app.json**)에 있는 [구성 플러그인](https://docs.expo.dev/config-plugins/introduction)
이며, 해당 노드 모듈이 제거된 경우 오류를 발생시켜 고아 구성 요소가 훨씬
적어집니다.

## 반론

다음은 Expo Prebuild가 특정 프로젝트에 적합하지 않을 수 있는 몇 가지 이유입니다.

### React Native 버전 관리

`npx expo prebuild`는 프로젝트에 설치된 `expo` 버전 기반으로 네이티브 코드를 생성합니다. 예를 들어 SDK 50`(expo@50.0.0`)이 설치된 프로젝트는 `react-native@0.73`
앱을
생성합니다.

Expo는 분기마다 새 버전을 릴리스하며, `react-native`는 일정 기반 릴리스 스케줄을 따르지 않습니다. 이는 최신 React Native 릴리스와 함께 `npx expo prebuild`를 사용할 수
없는
시기가 있을 수 있음을 의미합니다. 실험할 의향이 있는 경우 사용자 정의 prebuild 템플릿을 사용하여 이를 우회할 수 있습니다. 또한, 필요한 변경 사항을 최신 React Native 버전에서 포크로 선택하고
프로젝트에서 해당 포크를 사용하는 방법으로 이를 완화할 수 있습니다.

### 플랫폼 호환성

Prebuild는 현재 Expo SDK에서 지원하는 네이티브 플랫폼에만 사용할 수 있습니다. 이는 현재로서는 Android와 iOS를 의미합니다. 웹은 사용자 정의 네이티브 런타임 대신 브라우저를
사용하므로 `npx expo prebuild`가 필요하지 않습니다.

향후에는 `react-native-macos` 및 `react-native-windows`와 같은 추가 플랫폼을 지원하고자 하지만, 현재로서는 우선순위가 아닙니다.

추가 플랫폼을 대상으로 하는 경우, **android** 및 **ios** 프로젝트에 대해 prebuild를 계속 사용할 수 있습니다. 추가 플랫폼은 그동안 수동으로 구성해야 합니다.

### 모듈화 및 자동화보다 직접 변경하는 것이 빠름

모든 네이티브 변경 사항은 네이티브 모듈(React Native의 내장 네이티브 모듈 API 또는 Expo Modules API 사용) 및 구성 플러그인으로 추가해야 합니다. 이는 프로젝트에 네이티브 파일을 빠르게
추가하여 실험하려는 경우 prebuild를 실행하고 파일을 수동으로 추가한 다음 [monorepo](https://docs.expo.dev/guides/monorepos)로 시스템에 다시 작업하는 것이 더 나을 수
있음을 의미합니다. 이 프로세스를 가속화하기 위해 [Expo
Autolinking](https://docs.expo.dev/more/glossary-of-terms#expo-autolinking)에 네이티브 폴더 외부의 프로젝트 네이티브 파일을 찾아 빌드 전에 링크하는 기능을
추가할 계획입니다.

구성 파일(**gradle.properties** 파일 등)을 수정하려면 플러그인을 작성해야
합니다([예제](https://github.com/expo/expo/blob/1c994bb042ad47fbf6878e3b5793d4545f2d1208/apps/native-component-list/app.config.js#L21-L28)).
이는 도우미 플러그인 라이브러리를 사용하여 쉽게 자동화할 수 있지만, 자주 수행해야 하는 경우 다소 느릴 수
있습니다.

### 커뮤니티에서의 구성 플러그인 지원

모든 패키지가 _Expo Prebuilding_ 을 지원하는 것은 아닙니다. 설치 후 추가 설정이 필요한 라이브러리 중 구성 플러그인이 아직 없는 경우, 유지 관리자가 기능 요청을 인식할 수 있도록 풀 리퀘스트 또는
이슈를 열어두는 것이 좋습니다.

[`react-native-blurhash`](https://github.com/mrousavy/react-native-blurhash)와 같은 많은
패키지는 [자동 링크](https://docs.expo.dev/more/glossary-of-terms#autolinking)에서 처리하는 것 외에 추가 네이티브 구성이 필요하지 않으므로 구성 플러그인이 필요하지
않습니다.

[`react-native-ble-plx`](https://github.com/Polidea/react-native-ble-plx)와 같은 다른 패키지는 추가 설정이 필요하며,
따라서 `npx expo prebuild`와 함께 사용하려면 구성 플러그인이 필요합니다(이 경우
[`@config-plugins/react-native-ble-plx`](https://github.com/expo/config-plugins/tree/main/packages/react-native-ble-plx)
라는 외부 플러그인이 있습니다).

또한 시스템을 아직 채택하지 않은 인기 있는 패키지에 대한 플러그인을 제공하는 [트리 외부 구성 플러그인](https://github.com/expo/config-plugins)을 위한 저장소도 있습니다. 이를
TypeScript의 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)와 유사하게 생각하세요.
패키지가 자체 구성 플러그인을 제공하는 것이 바람직하지만, 시스템을 아직 채택하지 않은 경우 커뮤니티에서 저장소에 나열된 패키지를 사용할 수 있습니다.

## 시용하는 주요 이유 정리

1. 네이티브 프로젝트 초기화
   Prebuild는 네이티브 소스 코드를 자동으로 생성하여 Android 및 iOS 디렉토리를 만듭니다. 이를 통해 개발자는 별도의 초기 설정 없이 프로젝트를 바로 시작할 수 있습니다.

2. 일관된 프로젝트 설정
   Prebuild는 프로젝트의 설정과 구성을 일관되게 유지합니다. 구성 플러그인을 사용하여 네이티브 프로젝트에 필요한 설정을 자동으로 추가하고, 수동으로 설정할 때 발생할 수 있는 실수를 줄여줍니다.

3. 간편한 업그레이드
   Prebuild를 사용하면 네이티브 코드를 쉽게 업그레이드할 수 있습니다. package.json 파일에서 버전을 업데이트하고 Prebuild를 다시 실행하여 최신 버전의 네이티브 프로젝트를 생성할 수
   있습니다. 이는 순수 JavaScript 애플리케이션을 업그레이드하는 것과 비슷한 방식입니다.

4. 크로스 플랫폼 설정
   앱 아이콘, 이름, 스플래시 화면 등 크로스 플랫폼 설정을 단일 구성 파일에서 관리할 수 있습니다. 예를 들어, "icon": "./icon.png"와 같이 설정하면 모든 플랫폼에서 아이콘 생성이 자동으로
   처리됩니다.

5. 종속성 관리
   많은 네이티브 패키지는 추가 설정이 필요합니다. Prebuild는 이러한 설정을 자동으로 추가하여 복잡한 네이티브 컴파일 오류를 방지하고, 필요한 설정을 놓치는 실수를 줄입니다.

6. 네이티브 코드의 부작용 관리
   Prebuild를 사용하면 구성 플러그인으로 네이티브 코드의 부작용을 관리할 수 있습니다. 이는 라이브러리 작성자가 자신의 라이브러리에 필요한 설정을 자동화하고, 이를 테스트할 수 있게 해줍니다. 결과적으로,
   더 안정적이고 강력한 네이티브 코드 설정이 가능합니다.

7. 고아 코드 방지
   패키지를 제거할 때 필요한 부작용을 자동으로 제거하여 고아 코드를 방지합니다. 이는 프로젝트를 깨끗하고 유지 관리하기 쉽게 만듭니다.

8. EAS Build와의 통합
   Prebuild는 EAS Build와 통합되어 네이티브 디렉토리가 없는 프로젝트에서도 자동으로 네이티브 디렉토리를 생성합니다. 이는 빌드 프로세스를 간소화하고, 개발자가 빌드 환경에 집중할 수 있게 합니다.

Prebuild는 React Native와 Expo를 사용하는 프로젝트에서 네이티브 코드 관리를 단순화하고, 프로젝트 설정을 일관되게 유지하며, 업그레이드와 종속성 관리를 쉽게 만들어줍니다. 이를 통해 개발자는
네이티브 코드의 복잡성을 줄이고, 더 빠르고 안정적으로 개발을 진행할 수 있습니다.