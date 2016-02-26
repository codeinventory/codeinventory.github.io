---
layout: post
title:  "[IDE] Sublime Text 사용"
comments: false
categories: IDE
tags: IDE, Text Editor, Sublime Text
---

### Download Sublime Text 3
[https://www.sublimetext.com/3](https://www.sublimetext.com/3)

### Package Control
Sublime Text 를 사용하는 이유는 다양한 플러그인 패키지를 마음데로 사용할 수 있기 때문이다.
이를 사용하기 위해 `Package Control`을 준비해야 한다.

#### Installation
* [https://packagecontrol.io/installation](https://packagecontrol.io/installation)

1. ``ctrl+` ``  단축키를 사용하거나, `View > Show Console` 메뉴를 선택한다.
1. Sublime Text 아래에 편집창이 나타나면 아래의 text를 붙여넣기 하고 실행한다.
    
        import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
    
1.  Sublime Text를 재시작한다.

### [Package] File Browser
사이드바에 File Tree를 보여주고 File Browser 역할을 하는 Package이다.

#### Installation
* [https://github.com/aziz/SublimeFileBrowser](https://github.com/aziz/SublimeFileBrowser)

1. `cmd+shift+p` 단축키를 사용하여 Package Control을 실행시킨다.
2. `Install Package`로 검색하고 실행한다.
3. `FileBrowser`로 검색하고 설치한다.
4. `Preferences > Package Settings > File Browser > Key Bindings > User`를 선택하고 다음을 추가하면, `f1`단축키로 File Browser를 실행할 수 있다.

	{
	  "keys": ["f1"],
	  "command": "dired",
	  "args": {
	    "immediate": true,
	    "single_pane": true,
	    "other_group": "left",
	    "project": true
	  }
	}
