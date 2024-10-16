# Introduction
This is a simple browser using python, which gives you a basic understanding of browser programming.

# Dependence
* **PyQT5**
```bash
sudo apt install PyQt5
```

# Usage
```bash
python3 browser.py
```

# Code
```python
import sys
from PyQt5.QtCore import *
from PyQt5.QtWidgets import *
from PyQt5.QtWebEngineWidgets import *
from PyQt5.QtGui import QIcon

ICON_PATH = "obito.jpg"

class Browser(QMainWindow):
    def __init__(self) -> None:
        super().__init__()
        self.browser = QWebEngineView()
        self.browser.setUrl(QUrl("http://google.com"))
        self.setWindowIcon(QIcon(ICON_PATH))
        self.setCentralWidget(self.browser)
        self.showMaximized()
        
        navbar = QToolBar()
        self.addToolBar(navbar)
        
        back_btn = QAction("Back", self)
        back_btn.triggered.connect(self.browser.back)
        navbar.addAction(back_btn)
        
        forward_btn = QAction("Forward", self)
        forward_btn.triggered.connect(self.browser.forward)
        navbar.addAction(forward_btn)
        
        home_btn = QAction("Home", self)
        home_btn.triggered.connect(self.navigate_home)
        navbar.addAction(home_btn)
        
        reload_btn = QAction("Reload", self)
        reload_btn.triggered.connect(self.browser.reload)
        navbar.addAction(reload_btn)
        
        self.urlbar = QLineEdit()
        self.urlbar.returnPressed.connect(self.navigate_to_url)
        self.urlbar.mousePressEvent = self.select_all_text
        navbar.addWidget(self.urlbar)
        
        self.browser.urlChanged.connect(self.update_url)
        
    def navigate_home(self) -> None:
        """ Navigate to the home page. """
        self.browser.setUrl(QUrl("http://google.com"))
    
    def navigate_to_url(self) -> None:
        """ Navigate to the specified url. """
        url_text = self.urlbar.text()
        q = QUrl(url_text)
        if q.scheme() == "":
            if '.' in url_text:
                q = QUrl(f"http://{url_text}")
            else:
                q = QUrl(f"http://google.com/search?q={url_text}")
        self.browser.setUrl(q)
        
    def update_url(self, q) -> None:
        """ Update the url bar.

        Args:
            q (QUrl): The new url.
        """
        self.urlbar.setText(q.toString())
        
    def select_all_text(self, event) -> None:
        """ Select all text in the url bar.

        Args:
            event (QMouseEvent): The mouse event.
        """
        self.urlbar.selectAll()

    def keyPressEvent(self, event) -> None:
        """ Handle key press events. 

        Args:
            event (QKeyEvent): The key press event.
        """
        if event.key() == Qt.Key_F5:
            self.browser.reload()
        elif event.key() == Qt.Key_F6:
            self.urlbar.selectAll()

app = QApplication(sys.argv)
QApplication.setApplicationName("Fire Chrome")
window = Browser()
app.exec_()
```

