---
tags:
  - publish/done
tistory: https://jn-silveryoung.tistory.com/53
---


### 에러 로그

```
java.lang.NoClassDefFoundError: org/eclipse/compare/internal/TabFolderLayout at java.base/java.lang.Class.getDeclaredConstructors0(Native Method) at java.base/java.lang.Class.privateGetDeclaredConstructors(Class.java:3549) at java.base/java.lang.Class.getConstructor0(Class.java:3754) at java.base/java.lang.Class.getDeclaredConstructor(Class.java:2930) at org.eclipse.core.internal.registry.osgi.RegistryStrategyOSGI.createExecutableExtension(RegistryStrategyOSGI.java:236) at org.eclipse.core.internal.registry.ExtensionRegistry.createExecutableExtension(ExtensionRegistry.java:1034) at org.eclipse.core.internal.registry.ConfigurationElement.createExecutableExtension(ConfigurationElement.java:286) at org.eclipse.core.internal.registry.ConfigurationElementHandle.createExecutableExtension(ConfigurationElementHandle.java:65) at org.eclipse.ui.internal.WorkbenchPlugin.lambda$0(WorkbenchPlugin.java:285) at org.eclipse.swt.custom.BusyIndicator.showWhile(BusyIndicator.java:67) at org.eclipse.ui.internal.WorkbenchPlugin.createExtension(WorkbenchPlugin.java:283) at org.eclipse.ui.internal.dialogs.WorkbenchPreferenceNode.createPage(WorkbenchPreferenceNode.java:49) at org.eclipse.jface.preference.PreferenceDialog.createPage(PreferenceDialog.java:1274) at org.eclipse.ui.internal.dialogs.FilteredPreferenceDialog.createPage(FilteredPreferenceDialog.java:326) at org.eclipse.jface.preference.PreferenceDialog.showPage(PreferenceDialog.java:1160) at org.eclipse.ui.internal.dialogs.FilteredPreferenceDialog.showPage(FilteredPreferenceDialog.java:618) at org.eclipse.jface.preference.PreferenceDialog$5.lambda$0(PreferenceDialog.java:659) at org.eclipse.swt.custom.BusyIndicator.showWhile(BusyIndicator.java:67) at org.eclipse.jface.preference.PreferenceDialog$5.selectionChanged(PreferenceDialog.java:656) at org.eclipse.jface.viewers.StructuredViewer$3.run(StructuredViewer.java:819) at org.eclipse.core.runtime.SafeRunner.run(SafeRunner.java:47) at org.eclipse.jface.util.SafeRunnable.run(SafeRunnable.java:167) at org.eclipse.jface.viewers.StructuredViewer.firePostSelectionChanged(StructuredViewer.java:816) at org.eclipse.jface.viewers.ColumnViewer.firePostSelectionChanged(ColumnViewer.java:1062) at org.eclipse.jface.viewers.StructuredViewer.handlePostSelect(StructuredViewer.java:1184) at org.eclipse.swt.events.SelectionListener$1.widgetSelected(SelectionListener.java:83) at org.eclipse.jface.util.OpenStrategy.firePostSelectionEvent(OpenStrategy.java:283) at org.eclipse.jface.util.OpenStrategy$1.lambda$1(OpenStrategy.java:437) at org.eclipse.swt.widgets.RunnableLock.run(RunnableLock.java:40) at org.eclipse.swt.widgets.Synchronizer.runAsyncMessages(Synchronizer.java:132) at org.eclipse.swt.widgets.Display.runAsyncMessages(Display.java:4111) at org.eclipse.swt.widgets.Display.readAndDispatch(Display.java:3727) at org.eclipse.jface.window.Window.runEventLoop(Window.java:823) at org.eclipse.jface.window.Window.open(Window.java:799) at org.eclipse.ui.internal.OpenPreferencesAction.run(OpenPreferencesAction.java:64) at org.eclipse.jface.action.Action.runWithEvent(Action.java:474) at org.eclipse.jface.action.ActionContributionItem.handleWidgetSelection(ActionContributionItem.java:581) at org.eclipse.jface.action.ActionContributionItem.lambda$4(ActionContributionItem.java:415) at org.eclipse.swt.widgets.EventTable.sendEvent(EventTable.java:91) at org.eclipse.swt.widgets.Display.sendEvent(Display.java:4338) at org.eclipse.swt.widgets.Widget.sendEvent(Widget.java:1214) at org.eclipse.swt.widgets.Display.runDeferredEvents(Display.java:4136) at org.eclipse.swt.widgets.Display.readAndDispatch(Display.java:3724) at org.eclipse.e4.ui.internal.workbench.swt.PartRenderingEngine$5.run(PartRenderingEngine.java:1151) at org.eclipse.core.databinding.observable.Realm.runWithDefault(Realm.java:339) at org.eclipse.e4.ui.internal.workbench.swt.PartRenderingEngine.run(PartRenderingEngine.java:1042) at org.eclipse.e4.ui.internal.workbench.E4Workbench.createAndRunUI(E4Workbench.java:153) at org.eclipse.ui.internal.Workbench.lambda$3(Workbench.java:678) at org.eclipse.core.databinding.observable.Realm.runWithDefault(Realm.java:339) at org.eclipse.ui.internal.Workbench.createAndRunWorkbench(Workbench.java:583) at org.eclipse.ui.PlatformUI.createAndRunWorkbench(PlatformUI.java:173) at org.eclipse.ui.internal.ide.application.IDEApplication.start(IDEApplication.java:185) at org.eclipse.equinox.internal.app.EclipseAppHandle.run(EclipseAppHandle.java:219) at org.eclipse.core.runtime.internal.adaptor.EclipseAppLauncher.runApplication(EclipseAppLauncher.java:149) at org.eclipse.core.runtime.internal.adaptor.EclipseAppLauncher.start(EclipseAppLauncher.java:115) at org.eclipse.core.runtime.adaptor.EclipseStarter.run(EclipseStarter.java:467) at org.eclipse.core.runtime.adaptor.EclipseStarter.run(EclipseStarter.java:298) at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103) at java.base/java.lang.reflect.Method.invoke(Method.java:580) at org.eclipse.equinox.launcher.Main.invokeFramework(Main.java:627) at org.eclipse.equinox.launcher.Main.basicRun(Main.java:575) at org.eclipse.equinox.launcher.Main.run(Main.java:1431) Caused by: java.lang.ClassNotFoundException: org.eclipse.compare.internal.TabFolderLayout cannot be found by org.eclipse.team.svn.ui_4.8.0 at org.eclipse.osgi.internal.loader.BundleLoader.generateException(BundleLoader.java:567) at org.eclipse.osgi.internal.loader.BundleLoader.findClass0(BundleLoader.java:562) at org.eclipse.osgi.internal.loader.BundleLoader.findClass(BundleLoader.java:438) at org.eclipse.osgi.internal.loader.ModuleClassLoader.loadClass(ModuleClassLoader.java:195) at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:526) ... 62 more
```


이 에러는 Eclipse에서 Subversive SVN 플러그인 (`org.eclipse.team.svn.ui` 버전 4.8.0)이 내부적으로 사용하는 클래스 `org.eclipse.compare.internal.TabFolderLayout`를 찾지 못해서 발생하는 `NoClassDefFoundError`이다.


---

### 문제 상황

- Eclipse 2025-03 버전 새 설치 후
- `Windows -> Preferences -> Version Control -> SVN` 선택 시 오류 발생

```
org/eclipse/compare/internal/tabfolderlayout error
```
- `org.eclipse.compare.internal.TabFolderLayout` 클래스는 Eclipse 내부 패키지(비공개/internal API)이기 때문에, Eclipse 버전 간에 해당 클래스가 없거나 위치가 바뀌었을 수 있습니다.
- 즉, 설치한 Subversive SVN 플러그인 버전이 현재 사용하는 Eclipse 버전과 호환되지 않아 필요한 내부 클래스를 찾지 못하는 상황입니다.
- 보통 이런 문제는 플러그인 버전과 Eclipse 버전이 맞지 않을 때 발생합니다.


---

### 해결 방법

1. 아래 업데이트 저장소 URL을 Eclipse에 추가 (링크가 아닌 텍스트 복사)
    

```
https://download.eclipse.org/technology/subversive//updates/release/latest
```

2. 이전에 설치된 Subversive 플러그인을 삭제 후, 위 저장소에서 최신 버전의 플러그인을 다시 설치
    
3. Eclipse 재시작 후, 새로 설치한 플러그인의 연결 커넥터(Connectors) 설치 제안이 뜨면 반드시 설치
    
4. 커넥터 설치 완료 후 SVN 저장소에 정상 연결 가능
    
5. **Eclipse와 Subversive 플러그인 버전 호환성 확인**
    
    - Eclipse 2025-03(또는 최신 버전)과 호환되는 Subversive 최신 버전인지 확인
    - 구버전 Subversive 플러그인을 설치했다면 최신 버전으로 업데이트 권장
        
6. **Subversive 플러그인 재설치**
    
    - 기존 플러그인 삭제 후, 공식 업데이트 사이트에서 최신 버전 재설치
    - 업데이트 사이트 URL 예:
        
        ```
        https://download.eclipse.org/technology/subversive/updates/release/latest
        ```
        
7. **Eclipse 최신 버전 사용 권장**
    
    - 가능하면 Eclipse IDE도 최신 버전으로 업데이트
        
8. **Eclipse 설치 상태 점검**
    
    - Eclipse 설치가 제대로 되지 않아 일부 필수 번들이 누락됐을 가능성도 있음
    - `Help > About Eclipse IDE > Installation Details`에서 설치된 플러그인 확인
        



### 해결 방법

Subversive SVN 플러그인이 현재 Eclipse 내부 API와 맞지 않는 버전을 사용 중이여서
설치된 내부 플러그인 업데이트 모두 진행하고, SVN Connector 정상적으로 설치 완료 



### 참고 링크

- https://stackoverflow.com/questions/79351203/svn-eclipse-plugin-subversive-not-working-when-i-have-upgraded-to-eclipse-2024
- with ChatGPT