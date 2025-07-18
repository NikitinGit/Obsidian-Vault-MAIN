1. Поиск файла по имени быстрый если примерно известна папка нахождения 
   sudo grep -rliw "nikitin" /var/www/html/ 2>/dev/null ^find
2. стоит попробовать быстрый поиск по всему серверу sudo grep -rliP --exclude-dir={proc,sys,dev,tmp,run,var/cache} "Hi\s+Nikitin" / 2>/dev/null 
3. Загрузить файл на сервер - 
   scp /home/igor/Pictures/Screenshots/ANginx.png root@109.71.242.134:/home/root/images  (можно использовать псевдоним)
4. создать папку mkdir images/nikotin 
5. перенести файл в папку, с которой работает вебсервер (например nginx) mv /home/root/images/ANginx.png /var/www/html/images/ - после этого файл будет открываться по http://boxcompetition.ru/images/ANginx.png ^media
6. ssh продлить время бездействия 
   сервер - sudo nano /etc/ssh/sshd_config 
      ClientAliveInterval 300      # Проверять активность каждые 300 секунд (5 минут)
     ClientAliveCountMax 3        # Максимум 3 проверки перед отключением (итого 15 минут)
    клиент -  nano ~/.ssh/config
        добавить 
        Host your-server.com
         ServerAliveInterval 60    # Отправлять "keepalive" каждые 60 секунд
         ServerAliveCountMax 5     # Максимум 5 попыток (итого 5 минут)
        или дудосить 
        while true; do echo -n ''; sleep 60; done  ^keepconnect
        
        
        новый файл на клиенте 
          # Глобальные настройки для ВСЕХ хостов (опционально)
Host *
  ServerAliveInterval 120     # Проверка активности каждые 2 минуты
  ServerAliveCountMax 3       # Разорвать соединение после 3 пропущенных проверок (итого 6 минут)
  TCPKeepAlive yes            # Включить TCP-keepalive
  IdentitiesOnly yes          # Использовать ТОЛЬКО указанные ключи

#Настройки для Bitbucket (оставлены как у вас)
Host bitbucket.org
  HostName bitbucket.org
  User git                    # Обязательно для Git
  AddKeysToAgent yes
  IdentityFile ~/.ssh/sshigor # Ваш ключ для Bitbucket
  IdentitiesOnly yes

#Настройки для вашего сервера по IP
Host myvps                    # Алиас для удобства (можно вызвать просто `ssh myvps`)
  HostName 109.71.242.134     # IP сервера
  User root                   # Логин (root)
  # IdentityFile ~/.ssh/sshigor # Раскомментируйте, если используете ключ
  # Port 2222                 # Раскомментируйте, если сменили порт SSH
        оригиналдьный файл 
```
#bitbucket
Host bitbucket.org
  AddKeysToAgent yes
  IdentityFile ~/.ssh/sshigor
```
7. Изменить пут ьхранения картинок с помощью nginx - добавить в /etc/nginx/sites-available/default 
   `location /images/ {         alias /home/root/images/;        autoindex on;          # опционально, чтобы видеть список файлов в браузере        try_files $uri =404;   # отдавать 404, если файл не найден    }`
   перезагрузить nginx 
   sudo systemctl reload nginx 
   
8. 
   




Чтобы найти файл **с неизвестным именем**, содержащий текст **`Hi Nikitin`** (без учёта регистра), используйте следующие команды:

---

### **1. Поиск во всех файлах сервера (медленно, но надёжно)**

bash

sudo grep -rli "hi nikitin" / 2>/dev/null

#### **Разбор команды:**

- `grep` — поиск текста.
    
- `-r` — рекурсивно (включая подпапки).
    
- `-l` — выводить только имена файлов (без самого текста).
    
- `-i` — игнорировать регистр (`Hi Nikitin` = `hi nikitin`).
    
- `"hi nikitin"` — искомый текст.
    
- `/` — поиск от корня сервера.
    
- `2>/dev/null` — скрыть ошибки "Permission denied".
    

**Пример вывода:**

/var/www/site/index.html
/home/user/project/unknown_file.txt
/etc/config/some_config.conf

---

### **2. Ускоренный поиск (только в текстовых файлах)**

Если искать только в файлах с текстовым содержимым (исключая бинарные файлы):

bash

sudo find / -type f -exec grep -l -i "hi nikitin" {} + 2>/dev/null

---

### **3. Поиск в конкретных директориях**

Если догадываетесь, где может лежать файл (например, в `/var/www/` или `/home/`):

bash

sudo grep -rli "hi nikitin" /var/www/ /home/ 2>/dev/null

---

### **4. Просмотр содержимого найденного файла**

bash

cat /путь/к/файлу

или для редактирования:

bash

nano /путь/к/файлу

---

### **5. Если файлов слишком много**

Можно ограничить поиск по расширению (например, только `.html`, `.txt`, `.conf`):

bash

sudo grep -rli --include="*.{html,txt,conf}" "hi nikitin" / 2>/dev/null

---

### **Пример работы**

**Шаг 1:** Поиск

bash

sudo grep -rli "hi nikitin" / 2>/dev/null

**Вывод:**

/var/www/site/index.html
/etc/nginx/config.conf

**Шаг 2:** Проверка

bash

cat /etc/nginx/config.conf

**Содержимое:**

# Конфиг для Nikitin
server {
    listen 80;
    server_name hi_nikitin.example.com;
}

---
### 2. **Поиск с учётом пробелов и форматирования**

Если текст записан с нестандартными пробелами или переносами:

bash

sudo grep -rliP "Hi\s+Nikitin" /var/www/html/ 2>/dev/null

- `-P` — включает **Perl-совместимые регулярные выражения**.
    
- `\s+` — ищет 1 или более пробельных символов (обычный пробел, табуляция, перенос строки).
    

---

### 3. **Поиск в файлах с нестандартными кодировками**

Если файл сохранён в кодировке **UTF-16** или другой:

bash

sudo find /var/www/html/ -type f -exec file {} + | grep "text"

Затем для найденных текстовых файлов:

bash

sudo iconv -f UTF-16 -t UTF-8 /var/www/html/file.txt | grep -i "nikitin"

---

### 4. **Проверка на скрытые символы или HTML-теги**

Если текст внутри HTML/CSS/JS:

bash

sudo grep -rli "<[^>]*>Nikitin" /var/www/html/ 2>/dev/null

Ищет в тегах (`<p>Hi Nikitin</p>`).

---

### 5. **Альтернативные инструменты**

#### Ripgrep (быстрее `grep`):

bash

sudo rg -i "hi nikitin" /var/www/html/

Установка: `sudo apt install ripgrep`.

#### Silver Searcher (`ag`):

bash

sudo ag -i "hi nikitin" /var/www/html/

Установка: `sudo apt install silversearcher-ag`.

--- 
### **Важно!**

- Поиск по всему серверу (`/`) может занять **много времени** (особенно на больших дисках).
    
- Если знаете примерное расположение файла, укажите конкретную папку (например, `/var/www/`).
    
- Для сложных случаев можно использовать `ag` (`the_silver_searcher`) или `ripgrep` (`rg`), но их нужно предварительно установить.
    

---