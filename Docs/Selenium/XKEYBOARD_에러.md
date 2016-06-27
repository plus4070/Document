# Problem
## 현상
서버에서 Xvfb 실행시킬 경우 아래와 같은 현상 발생
```
The XKEYBOARD keymap compiler (xkbcomp) reports:
> Internal error:   Could not resolve keysym XF86AudioMicMute
Errors from xkbcomp are not fatal to the X server
```

## 원인
띄우려는 가상 키보드에 `XF86AudioMicMute`버튼이 존재하지 않아서 발생하는 문제라고 생각됨.

## 해결

```
sudo vim /usr/share/X11/xkb/symbols/inet
```
스크립트 내부에서 `XF86AudioMicMute`를 검색 후 해당 라인 주석처리
