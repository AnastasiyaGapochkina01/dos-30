### Задача 1
1) Установить на ВМ postgresql НЕ из стандартных репозиториев
2) Настроить хранение данных установленного postgres на отдельном lvm разделе размером 10G

### Задача 2
Развернуть Python-веб-сервер, настроить управление через systemd от имени пользователя `webadmin` (без домашней директории и shell-доступа), организовать логирование;\
Код приложения
- server.py
```python
from http.server import SimpleHTTPRequestHandler, HTTPServer
import os

os.chdir("/srv/webapp/content")
server = HTTPServer(('0.0.0.0', 8000), SimpleHTTPRequestHandler)
server.serve_forever()
```
- index.html
```html
<h1> Hello, DOS-30 <\h1>
```
